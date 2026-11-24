# Mock Servers

Mock servers are reference BAP and BPP implementations that ONDC provides so a Network Participant (NP) can test its integration without a live counterparty on the other side. If you are building a Seller App, a mock buyer drives requests at you; if you are building a Buyer App, a mock seller responds. They let you exercise full flows in a controlled, repeatable way.

## Why they matter

Integration testing normally needs a partner app to transact with, which is not always available for a given domain or flow. Mock servers remove that dependency: they support both buyer-side and seller-side testing across flows, so an NP can validate behaviour early and independently.

## What you can test

- **Happy-path flows** - run the standard request/response sequence for your domain end to end.
- **Error scenarios** - use mock BAP/BPP endpoints or inject invalid payloads to confirm your error handling behaves correctly.
- **Flow coverage** - the mock server supports both buyer and seller sides for all flows, making it useful where an automated tool does not yet cover a specific flow.

## Logs and certification

Logs generated while testing against the mock server can be submitted for review. An NP that has completed testing with the mock server may submit those mock server logs as part of its log verification. As with all ONDC testing, logs are captured per enabled API and validated before certification.

## Mock servers vs reference applications

Both let you test without a production partner, and the terms are sometimes used together:

- **Mock servers** are lightweight reference endpoints that simulate a BAP or BPP for driving flows and error cases.
- **Reference applications** are fuller sample buyer/seller apps (maintained by ONDC partners) that NPs onboard to and test against to validate APIs, flows, and compliance for a specific domain. Certification for a domain generally requires testing against both buyer and seller reference apps.

## Where it fits

Mock servers are part of the shared ONDC test bench used during staging and pre-production, alongside Pramaan, reference applications, sandbox environments, and the log validation utility. They are a testing aid only and are not part of the live production network.

## Related tools

- **Pramaan** - automation tool that runs simulated scenarios and automated checks across flows.
- **Reference Applications** - buyer/seller sample apps for domain validation and log generation.
- **Log Validation Utility** - validates the request/response logs produced during mock-server testing.
