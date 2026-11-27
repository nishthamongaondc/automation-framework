# Payment Assurance (PA)

**Payment Assurance (PA)** is an ONDC network capability that gives counterparties confidence that funds for an on-network transaction will be settled as agreed. It is used by flows where a seller or logistics participant needs assurance that the buyer-side payment they are owed will reach them, within the agreed settlement window.

## What it addresses

- **Settlement confidence** — Because settlement on ONDC happens off-network (directly bank-to-bank between counterparties), Payment Assurance provides a framework so that a participant performing fulfilment is assured of receiving its share per the agreed terms.
- **Tied to RSF** — PA works alongside the Reconciliation & Settlement Framework (RSF): reconciliation (`recon` / `on_recon`) remains on-network to agree amounts, while PA concerns the assurance that the agreed amounts are actually settled.

## Where it fits

Payment Assurance is a **core network capability** relevant to settlement-bearing domains (for example Retail and TRV/Metro flows). Participants opting into assured-payment arrangements follow the PA terms in addition to the standard RSF reconciliation.

> This page is a high-level overview. The exact Payment Assurance Program terms, eligibility, and onboarding steps are governed by ONDC policy and should be confirmed against the current ONDC Payment Assurance documentation.
