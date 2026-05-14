---
created: 2026-05-15 05:06
updated:
tags:
  - interview
status: active
company:
level:
related:
---


## Question

### 1. Tell me about the most complex distributed system you personally designed or significantly owned.

1. (Business Problem) We had to integrate a new national lung screening program alongside existing cancer screening flows.
2. (Architecture) The architecture included Spring Boot microservices, Kafka for event streams, and a Sql Server database.
3. (Scale) We processed about 10,000 screening events daily across two regions.
4. (Failure Handling) For failures—if assessment booking failed—we used compensating events to cancel orders and notify downstream.
5. (Incident) In production, we once saw Kafka lag cause delays; we improved by adding monitoring and scaling consumers.
6. (Tradeoffs) We traded off strong consistency for eventual consistency, prioritizing uptime and asynchronous processing.
7. (Ownership) I personally decided to use outbox patterns to ensure reliable event publishing.
8. (Redesign) Today, I’d redesign with multi-region Kafka clusters and finer-grained services for better scaling.

### 2. Tell me about a significant production incident you personally handled.
