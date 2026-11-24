# The Context Object

Every ONDC message has two top-level parts: a **`context`** and a **`message`**. The `message` carries the domain payload (the intent, the catalog, the order, and so on). The **`context`** is the envelope that wraps every request and callback: it names the API being called, routes the call between apps, and ties related calls together so both sides can follow one transaction from start to finish.

The context travels unchanged in spirit through the whole transaction: the same `transaction_id` appears in every call, while the `message_id` links each request to its callback.

### Fields the context carries

| Field | Role |
| --- | --- |
| `transaction_id` | The same value in every call of one transaction, from `search` to the last post-order call. Created by the BAP on `search`. |
| `message_id` | Links a request to its callback (for example `select` and `on_select`). Each unsolicited callback gets a new one. |
| `city` (for example `context.city: "std:080"`) | Targets a city for discovery. The Gateway uses it to fan `search` out to the seller apps registered for that domain and city. |
| `bap_uri` | The buyer app's callback URL. Seller apps send `on_search` **directly** to this URL rather than through the Gateway. |
| `bpp_id`, `bpp_uri` | The seller app's identifier and callback URL. Known from `on_search` onward, and used to address every direct call to that seller. |

The context carries further fields defined in the core schema. See [API Reference](/developer-guide/docs/api-reference) for the full context schema and every field.

### Related reading

- The `transaction_id` and `message_id` have their own lifecycle rules. See **Message & Transaction ID** for the full detail and diagrams.
- Every call carrying a context is also signed with an `Authorization` header; the receiver checks the signature before acting on the message. See **ACK / NACK**.
