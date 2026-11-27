# Key Concepts

Every participant on ONDC speaks one shared protocol, based on **Beckn**, the open-source decentralized commerce protocol that ONDC is built on. The core protocol is the same in every domain, so the same building blocks appear everywhere on the network.

---

## Core building blocks

| Concept | What It Is |
| --- | --- |
| **Network Participant (NP)** | Any entity registered on the network. Each NP has a unique **subscriber ID** (a domain name like `example-buyer.com`) and is listed in the Registry. |
| **BAP** (Beckn Application Platform) | The **buyer-side** app. It sends requests for consumers: search, order, status. Examples: a shopping, food or mobility app. |
| **BPP** (Beckn Provider Platform) | The **seller-side** app. It returns catalogs, confirms orders and sends fulfillment updates. Examples: a store or inventory platform, a logistics provider. |
| **Gateway (BG)** | Broadcasts a BAP's `search` to the relevant BPPs. It takes part **only** in discovery; everything after that is direct between BAP and BPP. |
| **Registry** | The trust anchor. It stores every NP's subscriber ID, URLs, supported domains and public keys, so a receiver can verify who sent a request. |
| **Domain** | A vertical with its own code and rules, for example `ONDC:RET10` (Grocery), `ONDC:LOG10` (Logistics) or `ONDC:FIS12` (Credit). The core protocol is the same in every domain. |

---

## Other terms & acronyms

The core network roles (NP, BAP, BPP, Gateway, Registry) are defined in **Core building blocks** above. This is a quick reference for the remaining protocol terms you will meet across the guide.

| Term | Definition |
| --- | --- |
| **Context** | Metadata envelope carried by every API request and callback |
| **Transaction ID** | Unique identifier that ties together all API calls of one transaction |
| **Message ID** | Unique identifier of one request and its callback (for example `search`–`on_search`); every unsolicited callback, such as `on_status`, has its own |
| **ACK** | Positive acknowledgment — request accepted for processing |
| **NACK** | Negative acknowledgment — request rejected with error details |
| **IGM** | Issue & Grievance Management — APIs for complaints and their resolution |
| **RSF** | Reconciliation & Settlement Framework — APIs for financial reconciliation |
| **NO** | Network Observability — transaction log monitoring framework |
| **Ed25519** | Elliptic curve signing algorithm used to sign requests |
| **X25519** | Elliptic curve algorithm used for encryption between NPs, where a use case requires it |
| **Subscriber ID** | The FQDN-based unique identifier of a Network Participant |
