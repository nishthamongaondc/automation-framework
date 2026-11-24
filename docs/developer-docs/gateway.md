# Gateway

The Beckn Gateway (BG) is a **search multicast router**. Its single job is to take a `search` request from a BAP and fan it out to all BPPs that are relevant to that search's context (domain, city, and so on). Like the Registry, ONDC operates it — you only ever call it or receive from it.

---

## Why the Gateway Exists

Without the Gateway, a BAP sending a search would need to:

1. Know every BPP on the network.
2. Maintain an up-to-date list of which BPPs serve which domains and cities.
3. Send the search to each one individually.

The Gateway removes that burden. The BAP sends **one** `search` to the Gateway, and the Gateway handles the fan-out.

---

## The /search Multicast

The Gateway is involved **only** in the `search` → `on_search` flow. Everything from `select` onward goes **directly** to the BPP.

**If you are a BAP:** you send `search` to the Gateway instead of to individual BPPs. From `select` onward you talk straight to each BPP, using the `bpp_id` and `bpp_uri` you received in the `on_search` callback.

**If you are a BPP:** you receive `search` from the Gateway. The `context` tells you who the original BAP is (`bap_id`, `bap_uri`). You process the search and send `on_search` **directly back to the BAP** — not back through the Gateway.

```mermaid
sequenceDiagram
    participant BAP as Buyer App (BAP)
    participant GW as Gateway
    participant B1 as Seller App 1
    participant B2 as Seller App 2
    BAP->>GW: search
    GW-->>BAP: ACK
    par Broadcast to matching seller apps
        GW->>B1: search (+ X-Gateway-Authorization)
    and
        GW->>B2: search (+ X-Gateway-Authorization)
    end
    B1->>BAP: on_search (direct, not via the Gateway)
    B2->>BAP: on_search (direct)
    Note over BAP,B2: Everything after search is direct
    BAP->>B2: select (to bpp_uri from on_search)
    B2->>BAP: on_select
```

---

## Why on_search Is Peer-to-Peer

The Gateway deliberately drops out of the path once it has fanned the search out. `on_search` callbacks travel **directly** from each BPP to the BAP's `bap_uri` — the Gateway neither routes nor stores them. A single `search` can therefore produce **many** `on_search` responses, one per responding BPP, arriving independently.

As a BAP, you accumulate these responses over a window defined by the `ttl` in the context. As a BPP, you always reply straight to the BAP and never back to the Gateway.

---

## What the Gateway Does NOT Do

- It does **not** participate in `select`, `init`, `confirm`, `status`, or any API other than `search`.
- It does **not** store or cache catalog data.
- It does **not** route `on_search` callbacks — BPPs send those directly to the BAP.
- It does **not** handle payments, fulfillment, or grievances.
- It is **not** a proxy or middleware for ongoing transactions.

---

## Gateway Authorization

When the Gateway forwards a `search` to a BPP, the request carries **two** authorization headers:

| Header | Signed by | Proves |
| ------------------------- | ------------------- | ------------------------------------------------------ |
| `Authorization` | The originating BAP | The search came from a legitimate BAP |
| `X-Gateway-Authorization` | The Gateway | The Gateway legitimately forwarded this request |

As a BPP developer you should verify **both** signatures. The Gateway's public key is looked up from the Registry, just like any other NP's.

```mermaid
flowchart LR
    BAP["<b>BAP</b><br/><small>signs the body</small>"]:::bap -- "search<br/>+ Authorization" --> GW["<b>Gateway</b><br/><small>adds its own signature</small>"]:::gw
    GW -- "search + Authorization<br/>+ X-Gateway-Authorization" --> BPP["<b>Your BPP</b>"]:::bpp
    BPP --> V1{"BAP's<br/>signature<br/>valid?"}
    V1 -- "yes" --> V2{"Gateway's<br/>signature<br/>valid?"}
    V2 -- "yes" --> ACK(["<b>ACK</b>"]):::ok
    V1 -- "no" --> NACK(["<b>NACK</b>"]):::bad
    V2 -- "no" --> NACK
    classDef bap fill:#3b82f61a,stroke:#3b82f6,stroke-width:1.5px
    classDef bpp fill:#10b9811a,stroke:#10b981,stroke-width:1.5px
    classDef gw fill:#f59e0b1a,stroke:#f59e0b,stroke-width:1.5px
    classDef ok fill:#22c55e1f,stroke:#22c55e,stroke-width:1.5px
    classDef bad fill:#ef44441f,stroke:#ef4444,stroke-width:1.5px
```

---

## Common Pitfalls

- **Sending non-search APIs to the Gateway** — only `search` goes through it. Sending `select` or `init` to the Gateway will fail.
- **Sending `on_search` back to the Gateway** — BPPs must send `on_search` directly to the BAP's `bap_uri`.
- **Ignoring `X-Gateway-Authorization`** — as a BPP, verify both the BAP signature and the Gateway signature.
- **Assuming a single `on_search` response** — a BAP may receive `on_search` from many BPPs for one search. Accumulate results over the `ttl` window.
