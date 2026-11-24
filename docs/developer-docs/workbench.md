# Workbench

The ONDC Workbench is the shared integration test bench that Network Participants (NPs) use to exercise protocol flows against a controlled environment before transacting on the live network. It is where a Buyer App (BAP) or Seller App (BPP) runs end-to-end flows, captures request/response logs, validates them, and assembles the evidence needed for certification.

## What it is

Rather than a single tool, the Workbench is the set of ONDC-provided testing capabilities an integrator works through during onboarding:

- **Flow execution** against mock servers and reference applications so flows can be driven without a live counterparty.
- **Log capture and validation** of all enabled APIs for a given scenario, which produce the records used for debugging, validation, and certification.
- **Report and conformance output** that demonstrates an application's flows and compliance with the ONDC protocol.

## Where it fits in the journey

ONDC operates across three isolated environments, and the Workbench activity lives in the pre-production stages, not in production:

- **Staging** - development and alpha testing. NPs onboard and subscribe here first, then give a staging demo.
- **Pre-Production** - beta testing and final validation. After the staging demo, the NP requests whitelisting of its `subscriber_id` on the NP Portal and calls `/subscribe` to register on pre-prod.
- **Production** - the live environment with real orders, reached only after functional testing, the final checklist, and a probationary period.

Each environment is separate: participants must onboard and subscribe to each one independently, so testing and go-live never share an environment.

## Who uses it

Every NP integrating as a BAP or BPP uses the Workbench. To certify for a domain, an NP must test its application against both buyer and seller reference applications for that domain and complete log verification. Testing tooling such as Pramaan, mock servers, reference apps, and the log validation utility are the building blocks an NP works through here.

## Typical workflow

1. Subscribe to the staging environment and configure your application.
2. Drive the required flows against mock servers or reference apps for your domain.
3. Capture logs for each enabled API and validate them with the log validation utility.
4. Submit logs for review (commonly by forking the verification-logs repository and raising a pull request).
5. Pass conformance, give the staging demo, then get whitelisted and promote to pre-prod for final validation before go-live.

## Related tools

- **Pramaan** - automation tool for validating flows through automated checks and simulated scenarios.
- **Mock Servers** - reference BAP/BPP endpoints for testing flows without a live counterparty.
- **Log Validation Utility** - validates request/response logs across domains.
