# Signing & Verification

> Every request and callback on the ONDC network is signed with the sender's Ed25519 private key and verified by the receiver using the sender's public key fetched from the Registry. This is how the network establishes who sent a message and that it was not tampered with in transit.

---

## Key Pairs

Every Network Participant (NP) holds **two key pairs**. Signing is done with the first; the second exists for the subset of use cases that also require encryption.

| Key pair | Algorithm | Used for |
| --- | --- | --- |
| **Signing keys** | Ed25519 | Signing every request and callback, and verifying the counterparty's signature. |
| **Encryption keys** | X25519 | Encrypting data between two NPs, only in use cases whose specification requires it. |

Both **public keys** are registered on the [ONDC Portal](https://portal.ondc.org) and stored in the ONDC Registry. **Private keys always stay with the NP** — they are held in the NP's own environment (for example its `.env` / secret store) and must never leave it or be shared.

| Key | Where it lives | Who uses it |
| --- | --- | --- |
| Ed25519 private key | NP's own environment (kept secret) | Sign every request and callback you send |
| Ed25519 public key | Registry | The counterparty verifies your signatures (fetched from the Registry) |

The Registry therefore holds **only public keys**. Public keys are verified by the Registry before they are stored.

---

## Key Format

Signing keys are Ed25519 ([RFC 8032](https://datatracker.ietf.org/doc/html/rfc8032)).

| Key | Size | Format |
| --- | --- | --- |
| Ed25519 public key | 32 bytes | Raw binary, Base64 encoded |
| Ed25519 private key | 32 bytes | Raw binary — keep secret |

---

## Signing a Request

Signing produces a digest of the request body, builds a signing string over that digest and a validity window, signs it with the Ed25519 private key, and places the result in the `Authorization` header.

**1. Generate the digest** of the request body using BLAKE-512:

```text
digest = Base64.encode( BLAKE512( request_body ) )
```

**2. Build the signing string** from `created`, `expires` and the digest:

```text
signing_string = "(created): {created_unix_ts}\n(expires): {expires_unix_ts}\ndigest:BLAKE-512={digest}"
```

**3. Sign the signing string** with the NP's Ed25519 signing private key:

```text
signature = Ed25519.sign(
    data       = signing_string,
    privateKey = NP_sign_private_key
)

encoded_signature = Base64.encode(signature)
```

**4. Set the Authorization header:**

```text
Authorization: Signature
  keyId="{subscriber_id}|{unique_key_id}|ed25519",
  algorithm="ed25519",
  created="{created_unix_ts}",
  expires="{expires_unix_ts}",
  headers="(created) (expires) digest",
  signature="{encoded_signature}"
```

Example header:

```text
Signature keyId="buyer-app.ondc.org|207|ed25519",algorithm="ed25519",created="1641287875",expires="1641291475",headers="(created) (expires) digest",signature="fKQWvXhln4UdyZdL87ViXQObdBme0dHnsclD2LvvnHoNxIgcvAwUZOmwAnH5QKi9Upg5tRaxpoGhCFGHD+d+Bw=="
```

The `keyId` tells the receiver which NP sent the request (`subscriber_id`) and which registered key to verify against (`unique_key_id`). The signature travels in the `Authorization` header, never in the request body.

---

## Verifying a Request

The receiver reconstructs the signing string from the incoming request and checks the signature against the sender's **public** key, which it fetches from the Registry.

```text
// 1. Parse the Authorization header
auth        = parseAuthorizationHeader(request.headers["Authorization"])
created     = auth.created
expires     = auth.expires
signature   = auth.signature
subscriber  = auth.keyId.subscriber_id

// 2. Fetch the sender's signing public key from the Registry
sender_sign_public_key = registry.fetchSigningPublicKey(subscriber)

// 3. Recompute the digest and signing string over the received body
digest = Base64.encode( BLAKE512( request_body ) )
signing_string = "(created): {created}\n(expires): {expires}\ndigest:BLAKE-512={digest}"

// 4. Verify
isValid = Ed25519.verify(
    data      = signing_string,
    signature = Base64.decode(signature),
    publicKey = sender_sign_public_key
)

if NOT isValid → REJECT("Signature verification failed")
```

Because the digest is recomputed from the body that actually arrived, any change to the payload in transit makes the signature fail. Because only the sender holds the matching Ed25519 private key, a valid signature proves the sender's identity.

---

## Registry Lookup

The public keys needed for verification are fetched from the Registry — the network's central trust store of NP public keys. Counterparty keys are retrieved with the **lookup** and **vlookup** APIs. The NP never needs the counterparty's private key, and never exposes its own.

| Term | Definition |
| --- | --- |
| **Registry** | Central trust infrastructure storing NP public keys, accessible through lookup and vlookup endpoints. |
| **Lookup API** | Registry query used by participants to retrieve a counterparty's public signing (and encryption) keys. |

---

## Terminology

| Term | Definition |
| --- | --- |
| **Network Participant (NP)** | Any registered entity on the ONDC network holding cryptographic key pairs for signing and encryption. |
| **Ed25519** | High-performance elliptic-curve digital signature algorithm (RFC 8032) used for request and callback authentication. |
| **Signing Key Pair** | The Ed25519 private key (kept secret in the NP's environment) and public key (registered in the Registry) used to sign and verify payloads. |
| **BLAKE-512** | Cryptographic hashing algorithm used to compute the digest of a request body before signing. |
| **`keyId`** | Identifier in the Authorization header combining the sender's `subscriber_id`, the `unique_key_id`, and the algorithm, telling the receiver which registered public key to verify against. |
