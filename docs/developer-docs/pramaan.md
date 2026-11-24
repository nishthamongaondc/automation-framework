# Pramaan

Pramaan is ONDC's automation tool for testing and validating Network Participant applications. It runs automated checks and simulated scenarios against a BAP or BPP so an NP can confirm that its flows behave correctly and generate the logs required for certification and log verification.

## What it does

Pramaan acts as a complete validation toolkit: it drives protocol flows, checks responses automatically, and simulates counterparties so an integrator does not always need a live partner on the other side. It supports both buyer-side and seller-side testing for flows, and is used for domains such as the FIS (financial services) flows as well as other ONDC domains.

## How NPs use it

The typical path to start testing with Pramaan:

1. Go to the ONDC Pramaan tool at `https://ondc.org/pramaan/` and click **Start Testing**.
2. Enter your application's subscriber ID.
3. Select your testing environment.
4. Choose your domain.
5. Begin testing and validate your workflows.

## Logs and certification

Pramaan produces logs that can be submitted as part of the certification process. Where a particular flow is not supported in Pramaan, that flow's logs have to be generated another way, for example with an approved seller NP or by using a reference seller application for log generation. An NP that has completed testing can submit its logs for review.

## Where it fits

Pramaan is one of the core building blocks of the ONDC integration test bench, alongside mock servers, reference applications, and the log validation utility. It is used during staging and pre-production testing, before an NP moves to the production environment and go-live.

## Related tools

- **Mock Servers** - reference buyer/seller endpoints for flows Pramaan does not cover, or for manual scenario testing.
- **Reference Applications** - buyer and seller apps (maintained by ONDC partners) used to validate APIs, flows, and compliance.
- **Log Validation Utility** - validates captured request/response logs for various domains.
