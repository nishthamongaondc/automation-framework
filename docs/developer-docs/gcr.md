# Global Catalog Repository (GCR)

The **Global Catalog Repository (GCR)** is ONDC's centralized catalog service for discovery. Instead of every buyer app broadcasting `/search` to every seller app and assembling the results itself, the GCR holds a consolidated, queryable view of seller catalogs across the network, so discovery can be served faster and with wider coverage.

## What it does

- **Catalog aggregation** — Seller Apps publish and keep their catalogs in sync with the GCR, which maintains a unified index of providers and items across the network.
- **Discovery at scale** — A Buyer App can query the GCR for matching providers/items rather than fanning a full-catalog `/search` out to each Seller App, reducing load and improving search relevance and latency.
- **Standard protocol** — GCR requests and responses use the same ONDC conventions as the rest of the network, including Ed25519 request signing (only public keys live in the Registry) and the standard `context` envelope.

## Where it fits

GCR sits in the **discovery** path alongside the Gateway. The Gateway multicasts `/search` to Seller Apps that respond directly via `on_search`; the GCR complements this by letting buyers discover against a pre-aggregated catalog. The two are used together depending on the flow.

> This page is a high-level overview. For the exact GCR endpoints, publish/sync model, and request/response schemas, refer to the current ONDC GCR specification.
