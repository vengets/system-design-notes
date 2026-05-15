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
3. (Scale) We processed about 10,000 screening events daily across AU region.
4. (Failure Handling) For failures—if assessment booking failed—we used compensating events to cancel orders and notify downstream.
5. (Incident) In production, we once saw Kafka lag cause delays; we improved by adding monitoring and scaling consumers.
6. (Tradeoffs) We traded off strong consistency for eventual consistency, prioritizing uptime and asynchronous processing.
7. (Ownership) I personally decided to use outbox patterns to ensure reliable event publishing.
8. (Redesign) Today, I’d redesign with multi-region Kafka clusters and finer-grained services for better scaling.

### 2. Tell me about a significant production incident you personally handled.

- **What broke:** We received critical alerts for rising Kafka consumer lag, elevated P95 latency, and delayed participant-processing workflows caused by downstream database contention.
- **Detection:** OpenTelemetry traces, RED metrics dashboards, Kafka lag monitoring, and correlation-ID-based log analysis helped us identify retry storms and thread pool saturation in a specific consumer service.
- **Immediate actions:** We scaled consumer pods, reduced retry aggressiveness, paused non-critical workloads, enabled stricter circuit breakers, and redirected failing events to retry topics and DLQs.
- **Root cause:** A recent concurrency increase combined with poorly tuned retry/backoff settings caused synchronized retries that amplified database contention and Kafka lag during peak load.
- **Tradeoffs:** We temporarily disabled optional enrichment flows and accepted eventual consistency delays to prioritize core participant-processing stability and restore throughput quickly.
- **Permanent fix:** We implemented retry topics with exponential backoff and jitter, DLQ policies, idempotent consumers, bulkhead isolation, improved partition distribution, and stronger observability dashboards.
- **Leadership:** I coordinated the incident bridge, assigned subsystem owners, communicated mitigation progress, and later led the postmortem and operational improvement discussions.
- **Lessons:** We learned that retries can amplify failures at scale, so we improved retry governance, load testing, adaptive throttling, and downstream protection mechanisms for future resilience.

---

### 3. How do you handle disagreements?
In one project, there was a disagreement around whether we should use synchronous REST calls between services or move to Kafka-based asynchronous communication for participant assessment processing. 
Some engineers preferred REST because it was simpler to debug and easier to reason about transactionally, 
while I argued that the downstream systems were unreliable and tightly coupling services through synchronous calls would create cascading failures and latency spikes during peak processing. I supported my argument using production metrics showing increasing timeout rates and thread pool exhaustion during downstream slowness, and I proposed an event-driven approach with retries, DLQs, idempotency, and eventual consistency instead. We finally adopted a hybrid approach where critical real-time validations remained synchronous while long-running workflows moved asynchronously through Kafka, which reduced failure propagation significantly. Looking back, I think the compromise was better than my original position because some synchronous flows were genuinely needed for user experience and operational simplicity.

### 3. “API latency increased. What do you check first?”

Strong answer:

```
- Error rates
- - P95/P99 latency
- - Recent deployments
- - CPU/memory
- - DB latency
- - Kafka lag
- - downstream dependency health
- - traces for slow requests
```

This is VERY common.

---

### 4. “How would you debug a production issue?”

Expected flow:

```
dashboard→ metrics→ logs→ traces→ infra→ dependency map
```

---

### 5. “What metrics would you monitor?”

Very important.

Example:

|Layer|Metrics|
|---|---|
|API|latency, RPS, errors|
|Kafka|lag, retries|
|DB|slow queries, pool usage|
|JVM|heap, GC|
|Kubernetes|pod restarts|
|Business|payment failures|

---
# Example Datadog Query Style

## Logs

```
service:payment-service status:error
```

---

## Metrics

```
avg:system.cpu.user{service:payment-service}
```

---

## Latency

```
p95:trace.http.request.duration{service:payment-service}
```

You are NOT expected to memorize syntax usually.

But understanding the concepts helps a lot.