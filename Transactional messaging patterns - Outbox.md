## Transactional Outbox

```mermaid
flowchart TD
    Svc["Sender"]
    Worker["Message Relay"]
    Queue[["Message Broker"]]

    subgraph Database
        Outbox[("Outbox<br>table")]
        Entity[("Projection<br>table")]
    end

    Svc -- 1a Upsert/Delete --> Entity
    Svc -- 1b Append --> Outbox
    Worker -- 2 Read --> Outbox
    Worker -- 3 Publish --> Queue
```

### Key Features
 * Sender writes event to an outbox table within the same transaction as the business entity.
 * Outbox table is polled, and events are published to the broker.
 * Reliable, avoids dual-write issues.
 * Simpler than full event sourcing, but no full event history.
