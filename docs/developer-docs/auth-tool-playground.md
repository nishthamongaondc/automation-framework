# Auth Tool Playground

The Auth Tool Playground is an interactive utility for generating and verifying ONDC request signatures and building the `Authorization` header by hand. Use it to confirm your understanding of the signing flow before you embed signing logic into your network participant (NP) middleware, and to debug signature failures against a known-good reference.

## What it does

The playground walks through the exact steps ONDC uses to sign and verify a request:

- **Digest the body.** It computes a BLAKE-512 hash of the request body (the digest the `Authorization` header references).
- **Sign the digest.** It signs the digest with your Ed25519 `signing_private_key` and assembles the signing string from the `(created)` and `(expires)` timestamps plus the `digest`.
- **Build the header.** It produces the full header in the standard form:

```
Authorization: Signature keyId="<subscriber_id>|<unique_key_id>|ed25519",
  algorithm="ed25519",
  created="<timestamp>",
  expires="<timestamp>",
  headers="(created) (expires) digest",
  signature="<base64-encoded-signature>"
```

- **Verify a signature.** Paste a header and payload, supply the sender's Ed25519 public key, and the playground checks whether the signature is valid — the same operation a recipient performs after fetching the public key from the Registry via `subscriber_id`.

## When to use it

- **Onboarding / site verification.** Sign a unique `request_id` with your `signing_private_key` (Ed25519, without hashing) to produce the `SIGNED_UNIQUE_REQ_ID` used for domain verification during `/subscribe`.
- **Debugging signature failures.** Most verification errors come from signing a different payload than you send — for example, signing minified JSON but transmitting a beautified body, stray escape characters in the header instead of spaces, or changing the `context.timestamp` after signing. The playground lets you reproduce and isolate these mismatches against a controlled reference.
- **Interpreting registry errors.** Use it to reproduce conditions behind `Subscriber not found`, algorithm-mismatch, and expired-request errors, so you can confirm the header format strictly matches the expected structure.

## Key reminders

- Sign the **exact** bytes you send — stringify (and prefer minified) JSON, and do not let an editor re-format the payload after signing.
- The `signing_private_key` stays on your side (in your NP environment). Only your **public** key lives in the Registry.
- ONDC request signing is Ed25519 with a BLAKE-512 digest — not HMAC or RSA.

The playground is a learning and debugging aid. For production, move the same logic into your platform's middleware using the ONDC signing utilities (see **SDKs & Utilities**).
