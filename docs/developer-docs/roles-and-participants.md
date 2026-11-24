# Roles & Participants

Two kinds of actors make up the network: **Network Participants**, who transact, and the shared **ecosystem participants** that every transaction relies on.

---

## Network Participant roles

A **Network Participant (NP)** is any entity registered on the network. Each NP has a unique **subscriber ID** (an FQDN-based identifier, a domain name like `example-buyer.com`) and is listed in the Registry.

An NP transacts in one of two roles:

- **BAP (Beckn Application Platform)** — the **buyer-side** app. It sends requests on behalf of consumers: search, order, status. Examples: a shopping, food or mobility app.
- **BPP (Beckn Provider Platform)** — the **seller-side** app. It returns catalogs, confirms orders and sends fulfillment updates. Examples: a store or inventory platform, a logistics provider.

A single company can act as both a BAP and a BPP, but they are separate roles with separate subscriber IDs.

---

## Ecosystem participants

Beyond the buyer and seller apps, two shared services hold the network together. They are infrastructure, not transacting NPs:

- **Gateway (BG)** — broadcasts a BAP's `search` to the relevant BPPs. It takes part **only** in discovery; everything after that is direct between BAP and BPP.
- **Registry** — the trust anchor. It stores every NP's subscriber ID, URLs, supported domains and public keys, so a receiver can verify who sent a request.

---

## Domains

Every NP operates within one or more **domains** — verticals with their own code and rules, for example `ONDC:RET10` (Grocery), `ONDC:LOG10` (Logistics) or `ONDC:FIS12` (Credit). The core protocol is the same in every domain, so the roles above behave the same way everywhere on the network.
