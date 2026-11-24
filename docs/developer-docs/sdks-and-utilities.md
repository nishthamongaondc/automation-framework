# SDKs & Utilities

ONDC does not ship custom code for individual participants or specific use cases. Instead, it publishes a set of openly available utilities, SDKs, and reference applications on GitHub to accelerate onboarding, QA, and protocol understanding. These demonstrate standard protocol implementations and best practices that you adapt into your own platform.

## Signing & verification utilities

ONDC provides sample scripts and SDKs in **multiple programming languages (JavaScript, Python, and others)** for generating and verifying signatures. They are designed to be dropped into or adapted by your platform's middleware, so you don't have to implement Ed25519 signing from scratch.

Typical capabilities:

- **Key generation.** Create an Ed25519 signing key pair and an X25519 encryption key pair. Keys are exported as **Raw** (base64 of the raw bytes) or **PEM** (base64 of an X.509-encoded public key); Raw is the more prevalent format and is generally what Registry communication uses.
- **Request signing.** Compute the BLAKE-512 digest of the body and produce the Ed25519 `Authorization` header.
- **Signature verification.** Validate an incoming signature against a sender's public key.
- **Onboarding helpers.** Utilities such as the `on_subscribe` / subscribe helper sign a unique `request_id` to produce the `SIGNED_UNIQUE_REQ_ID` needed for domain/site verification during `/subscribe`.

If you have a custom implementation, use these utilities as the reference for your specific language to match ONDC's expected behavior exactly.

## Reference applications

Sample buyer- and seller-side (BAP/BPP) applications and mock servers let you exercise integration flows and QA without a live counterparty. They are useful for validating your signing and request/response handling end to end. Note that payload formatting matters: the body you sign must be byte-identical to the body you send (minified, no beautification, no stray escape characters), or verification against reference apps and mock servers will fail.

## Log validation

- **Log Utility / Validation Tool** — `https://github.com/ONDC-Official/log-validation-utility` validates request/response logs across domains, and is used in the log-verification process (for example, submitting ordered flow payloads for a domain such as TRV11 to be checked against the schema).

## Where to find them

All of these tools and resources are published on the official ONDC GitHub organization:

- **ONDC-Official GitHub** — `https://github.com/ONDC-Official`

From there you can reach the signing/verification utilities, log-validation utility, and reference applications. Sandbox and LogSubmission tooling is referenced in the ONDC developer guide.
