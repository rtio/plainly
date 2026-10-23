---
name: dumb-down-level1-strict
tags: [dumb-down, level1]
max_turns: 6
allowed_tools: [Read, Glob, Grep, Skill]
---

Use the dumb-down-this-for-me skill on the text below at level 1, 100% ASD-STE100.

PgBouncer is a lightweight connection pooler for PostgreSQL. It supports three pool modes: session, transaction, and statement. In transaction mode, a server connection is assigned to a client only for the duration of a transaction, which lets 1,000 client connections share a pool of 20 server connections. Prepared statements were historically incompatible with transaction mode because they live on a single server connection; PgBouncer 1.21 added protocol-level prepared statement support through the max_prepared_statements setting. Session-level features such as SET, LISTEN, and advisory locks that are utilizing session state remain unsafe in transaction mode. Operators should ensure default_pool_size is tuned prior to commencing load tests. When all default_pool_size connections are busy, clients wait; after reserve_pool_timeout seconds, PgBouncer opens up to reserve_pool_size extra connections.
