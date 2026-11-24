# What ONDC Is

> A short introduction to the Open Network for Digital Commerce for developers who are new to ONDC.

---

Traditional e-commerce platforms are **closed**. A seller on Platform A is only visible to buyers on Platform A; to reach buyers elsewhere, it must integrate with every other platform separately.

**ONDC (Open Network for Digital Commerce)** is an open, decentralized network backed by the Government of India's Department for Promotion of Industry and Internal Trade (DPIIT). It separates **buyer applications** from **seller applications**, so any buyer app can discover and transact with any seller app on the network. No single entity owns the transaction, the data, or the relationship.

Think of it as **UPI for e-commerce**: just as UPI lets any bank's app pay any other bank's app, ONDC lets any buyer app buy from any seller app. Like email, where Gmail and Outlook users can write to each other because both speak SMTP, every participant speaks one shared protocol, based on [Beckn](/developer-guide/docs/protocol).

```mermaid
flowchart LR
    subgraph Closed["<b>Closed platforms</b>"]
        direction TB
        BA["Buyers on Platform A"]:::bap --> PA["<b>Platform A</b><br/><small>owns buyers, sellers and data</small>"]:::closed --> SA["Sellers on Platform A"]:::bpp
        BB["Buyers on Platform B"]:::bap --> PB["<b>Platform B</b><br/><small>owns buyers, sellers and data</small>"]:::closed --> SB["Sellers on Platform B"]:::bpp
    end
    subgraph Open["<b>ONDC open network</b>"]
        direction TB
        B1["Buyer app 1"]:::bap & B2["Buyer app 2"]:::bap & B3["Buyer app 3"]:::bap --> N(("<b>ONDC</b><br/><small>one shared protocol</small>")):::ondc
        N --> S1["Seller app 1"]:::bpp & S2["Seller app 2"]:::bpp & S3["Seller app 3"]:::bpp
    end
    Closed ~~~ Open
    classDef bap fill:#3b82f61a,stroke:#3b82f6,stroke-width:1.5px
    classDef bpp fill:#10b9811a,stroke:#10b981,stroke-width:1.5px
    classDef ondc fill:#0081cc1f,stroke:#0081cc,stroke-width:2px
    classDef closed fill:#64748b1a,stroke:#64748b,stroke-width:1.5px
```

On a closed platform, a seller reaches only that platform's buyers. On ONDC, every buyer app can reach every seller app.

---

## Developer Resources

| Resource | URL |
| --- | --- |
| **ONDC GitHub (Official)** | [github.com/ONDC-Official](https://github.com/ONDC-Official) |
| **Protocol Specs** | [github.com/ONDC-Official/automation-specifications](https://github.com/ONDC-Official/automation-specifications) |
| **Network Extension Layer** | [github.com/ONDC-Official/protocol-network-extension](https://github.com/ONDC-Official/protocol-network-extension) |
| **Signing & Verification** | [developer-docs/registry/signing-verification.md](https://github.com/ONDC-Official/developer-docs/blob/main/registry/signing-verification.md) |
| **ONDC Portal** | [portal.ondc.org](https://portal.ondc.org) |
