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

1. What broke: We had an alert in our log metrics dashboard, Describe the incident—what failed, and how you noticed.
2. Detection: How did monitoring or alerts help you detect it, and how did you investigate?
3. Immediate actions: What steps did you take to mitigate or stop the impact?
4. Root cause: What was the underlying issue, and how did you confirm it?
5. Tradeoffs: What short-term tradeoffs did you make (e.g., disabling a feature, prioritizing uptime)?
6. Permanent fix: What long-term improvements did you implement—like architectural changes or new policies?
7. Leadership: How did you coordinate the response and ensure the team learned from it?
8. Lessons: What would you do differently next time or scale up?