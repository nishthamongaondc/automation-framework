# Message & Transaction ID

Every ONDC call is **tracked** by two identifiers in its `context`:

- A **`transaction_id`** that is the same for the whole transaction, so both apps can group every call of one order together.
- A **`message_id`** that links each request to the callback that answers it.

A request and its callback share one `message_id`; an unsolicited callback gets its own. `bpp_id` and `bpp_uri` are known from `on_search` onwards.

### How the identifiers flow through a transaction

```mermaid
flowchart LR
    subgraph T["<b>transaction_id T1</b>: the same in every call of the order"]
        direction LR
        M1["<b>message_id m1</b><br/>search<br/>on_search × many"] --> M2["<b>m2</b><br/>select<br/>on_select"] --> M3["<b>m3</b><br/>init<br/>on_init"] --> M4["<b>m4</b><br/>confirm<br/>on_confirm<br/><small>order.id set</small>"] --> M5["<b>m5</b><br/>on_status<br/><small>unsolicited</small>"] --> M6["<b>m6</b><br/>status<br/>on_status"]
    end
    style T fill:#0081cc0f,stroke:#0081cc,stroke-width:1.5px
    classDef unsol fill:#f59e0b1a,stroke:#f59e0b,stroke-width:1.5px
    class M5 unsol
```

### The identifiers at a glance

| Identifier | Created by | Stays the same |
| --- | --- | --- |
| `transaction_id` | BAP, on `search` | In every call of the transaction, from `search` to the last post-order call. |
| `message_id` | The sender of each request | Between a request and its callback (`select` and `on_select`). Each unsolicited `on_status`, `on_update` or `on_cancel` has a new one. |
| `bpp_id`, `bpp_uri` | BPP, in `on_search` | In every call with that seller app after `on_search`. |
| Order ID | Set in `confirm` | In every post-order call, as `order.id` or `order_id`. |

### Why it matters

- Because the `transaction_id` never changes, logs, issues and settlements for one order can all be correlated, whichever app produced them.
- Because a request and its callback share a `message_id`, the sender can match an asynchronous callback back to the exact request it answers, even when many are in flight.
- Because each **unsolicited** callback (such as a seller-initiated `on_status`) carries a fresh `message_id`, it is clearly distinguished from a callback that answers a specific request.

For where these identifiers sit in the message envelope, see **Context**. For the request-and-callback call sequence they thread through, see **API Calls**.
