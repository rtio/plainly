---
type: llm
weight: 1
---

Here is the source text the response explains:

> PgBouncer is a lightweight connection pooler for PostgreSQL. It supports three pool modes: session, transaction, and statement. In transaction mode, a server connection is assigned to a client only for the duration of a transaction, which lets 1,000 client connections share a pool of 20 server connections. Prepared statements were historically incompatible with transaction mode because they live on a single server connection; PgBouncer 1.21 added protocol-level prepared statement support through the max_prepared_statements setting. Session-level features such as SET, LISTEN, and advisory locks that are utilizing session state remain unsafe in transaction mode. Operators should ensure default_pool_size is tuned prior to commencing load tests. When all default_pool_size connections are busy, clients wait; after reserve_pool_timeout seconds, PgBouncer opens up to reserve_pool_size extra connections.

The response adds no factual claims about PgBouncer beyond this text. Rewording, splitting a sentence, headings, and stating something the text directly implies (for example, that a pooler keeps a pool of server connections) are fine. Fail only for a new factual claim that the text neither states nor directly implies and that is not labeled as outside knowledge.
