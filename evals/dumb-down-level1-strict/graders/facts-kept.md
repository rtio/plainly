---
type: llm
weight: 1
---

The response keeps every fact from the source text, without changing any of them:

- There are three pool modes: session, transaction, and statement.
- In transaction mode, 1,000 client connections share 20 server connections.
- Prepared statements did not work in transaction mode until PgBouncer 1.21 added support through the max_prepared_statements setting.
- SET, LISTEN, and advisory locks are unsafe in transaction mode.
- default_pool_size should be tuned before load tests.
- When the pool is busy, clients wait; after reserve_pool_timeout seconds, PgBouncer opens up to reserve_pool_size extra connections.

It adds no claims that are not in the source, unless they are clearly labeled as outside knowledge. Fail if any fact above is missing or changed.
