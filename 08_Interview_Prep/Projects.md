---
created: 2026-05-14 04:27
updated:
tags:
  - interview
status: active
company:
level:
related:
---


# Scenario 2 — Kafka Consumer Lag Causing System-Wide Processing Delays

## Situation

We had an event-driven participant processing pipeline using Kafka and microservices. One evening, we noticed participant workflows were getting delayed significantly. New events were entering Kafka successfully, but downstream processing SLAs started failing, and some participant actions were delayed by hours.

This became critical because downstream screening actions and notifications depended on timely processing.

---

## Detection

The incident was identified through:

- Kafka consumer lag alerts
    
- Spike in workflow processing times
    
- Increase in DLQ entries
    
- Growing queue depth in monitoring dashboards
    
- Support team reporting delayed participant updates
    

Grafana showed:

- steady producer throughput
    
- slowing consumer throughput
    
- increasing lag on a few partitions only
    

---

## Root Cause

The issue turned out to be uneven partition hot-spotting combined with database contention.

A small subset of participant events triggered expensive downstream enrichment queries. Because Kafka preserves ordering within a partition:

- one slow event blocked the entire partition
    
- lag accumulated rapidly
    
- retries amplified processing pressure
    
- thread pools became saturated
    

Additionally:

- retry logic was too aggressive
    
- failed messages were retried immediately
    
- consumers spent more time retrying than processing healthy traffic
    

This created a retry storm.

---

## Mitigation

Immediate mitigation steps:

- temporarily reduced consumer concurrency pressure
    
- paused problematic consumers briefly
    
- redirected poison messages to DLQ faster
    
- introduced backoff with jitter
    
- increased partition count strategically
    
- scaled consumers horizontally carefully
    

We also:

- isolated expensive enrichment calls
    
- cached repeated DB lookups
    
- optimized slow SQL queries
    

---

## Communication

I coordinated communication between:

- platform engineers
    
- DB teams
    
- support teams
    
- product owners
    

We created:

- impact timelines
    
- recovery ETA updates
    
- processing backlog visibility
    

---

## Long-Term Prevention

We implemented:

- adaptive retry strategies
    
- exponential backoff with jitter
    
- partition balancing analysis
    
- consumer lag SLO dashboards
    
- circuit breakers around enrichment services
    
- bulkhead isolation for heavy workflows
    
- better DLQ routing policies
    

We also redesigned parts of the enrichment flow to reduce synchronous dependencies.

---

## Observability Improvements

We added:

- per-partition lag dashboards
    
- retry rate monitoring
    
- DLQ growth alerts
    
- tracing across Kafka consumers
    
- thread pool saturation metrics
    
- processing time percentile dashboards
    

---

# Staff-Level Talking Point

“The real challenge wasn’t Kafka itself — it was understanding how retry behavior, partition ordering guarantees, and downstream dependency latency interacted together under production-scale load.”

That sounds VERY senior.

---

# Scenario 3 — Duplicate Processing Incident Despite “Successful” APIs

## Situation

We had a critical participant onboarding workflow involving multiple async microservices. During a production incident, duplicate participant records started appearing intermittently, creating downstream inconsistencies and operational risk.

The issue was especially dangerous because APIs were returning HTTP 200 successfully, so from the client perspective everything appeared healthy.

---

## Detection

The issue was detected through:

- reconciliation job mismatches
    
- duplicate participant alerts
    
- operational support escalation
    
- unusual increase in downstream workflow triggers
    

Initially, logs looked normal because requests themselves succeeded.

---

## Root Cause

The root cause was a distributed idempotency failure during timeout cascades.

What happened:

1. upstream service timed out waiting for response
    
2. client retried request
    
3. original request actually completed later
    
4. retry request also succeeded
    
5. duplicate workflow execution occurred
    

The system lacked a strong centralized idempotency enforcement layer.

Compounding factors:

- retries occurred across multiple services
    
- eventual consistency delayed visibility
    
- race conditions appeared under concurrent load
    
- duplicate detection existed only partially
    

This only emerged under production concurrency patterns.

---

## Mitigation

Immediate actions:

- paused affected workflow processing
    
- identified impacted records
    
- created reconciliation scripts
    
- temporarily enforced stricter duplicate validation
    
- reduced retry aggressiveness
    

We then:

- implemented centralized idempotency key validation
    
- added request deduplication storage
    
- enforced transactional workflow checkpoints
    

---

## Communication

I worked closely with:

- business stakeholders
    
- operations teams
    
- platform engineers
    
- database teams
    

We communicated:

- scope of duplicate impact
    
- rollback safety
    
- reconciliation progress
    
- temporary operational workarounds
    

---

## Long-Term Prevention

We introduced:

- idempotency tokens across APIs
    
- deduplication tables with unique constraints
    
- workflow state validation
    
- exactly-once processing safeguards where possible
    
- retry governance policies
    
- timeout hierarchy improvements
    

We also redesigned parts of the workflow to become safely retryable.

---

## Observability Improvements

We added:

- duplicate processing alerts
    
- retry correlation tracing
    
- idempotency hit/miss metrics
    
- workflow replay dashboards
    
- timeout dependency tracing
    
- reconciliation health dashboards
    

---

# VERY Strong Staff-Level Line

“One important lesson was that successful HTTP responses do not necessarily guarantee correct distributed-system behavior. Reliability in distributed systems is often about handling retries, partial failures, and eventual consistency safely.”

That line sounds extremely strong in senior interviews.
