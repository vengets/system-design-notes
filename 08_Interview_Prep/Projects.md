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


# Scenario 2 — Kafka Consumer Lag Incident (STAR Format)

## Situation

At Telstra Health, we had an event-driven participant processing platform using Kafka and microservices. During a high-volume period, we started seeing significant delays in participant workflows, and some downstream screening actions were taking hours instead of minutes.

---

## Task

As one of the senior backend engineers, my responsibility was to investigate the production degradation, stabilize the system quickly, and prevent backlog growth from impacting critical participant processing SLAs.

---

## Action

I started by analyzing Kafka consumer lag metrics, partition distribution, application logs, and downstream service latency. We discovered that a subset of Kafka partitions were becoming hotspots because certain participant events triggered expensive downstream enrichment queries.

Since Kafka guarantees ordering within a partition, a single slow event blocked processing for that entire partition. At the same time, aggressive retry policies without proper backoff created a retry storm, which saturated consumer thread pools and increased database pressure further.

To stabilize the platform:

- we temporarily paused problematic consumers,
    
- redirected poison messages faster into DLQs,
    
- introduced exponential backoff with jitter,
    
- optimized slow database queries,
    
- added caching for repeated enrichment lookups,
    
- and scaled consumers horizontally after validating partition distribution.
    

I also coordinated incident communication across platform, database, and support teams while providing regular recovery updates.

---

## Result

We successfully stabilized the system, reduced Kafka lag back to normal levels, and restored workflow processing SLAs. After the incident, we implemented stronger observability around partition-level lag, retry metrics, thread pool saturation, and DLQ growth.

One of the biggest long-term improvements was redesigning parts of the enrichment workflow to reduce synchronous dependencies and make the platform more resilient under production-scale load.

---

# Scenario 3 — Duplicate Processing / Idempotency Incident (STAR Format)

## Situation

We had a distributed participant onboarding workflow involving multiple async microservices and APIs. During a production incident, duplicate participant records started appearing intermittently, causing downstream workflow inconsistencies and operational concerns.

The challenging part was that APIs were still returning successful HTTP responses, so the issue was not immediately obvious.

---

## Task

My responsibility was to identify why duplicate processing was happening, contain the impact safely, and improve the reliability of the workflow under retries and partial failures.

---

## Action

I began by tracing request flows across services and correlating retry patterns with workflow execution logs. We discovered that during timeout cascades, some upstream services retried requests before downstream processing had fully completed.

In certain scenarios:

- the original request completed successfully after a delay,
    
- the retry request also succeeded,
    
- and both executions triggered downstream workflows independently.
    

The platform lacked a strong centralized idempotency enforcement mechanism across services.

To mitigate the issue:

- we temporarily paused affected workflows,
    
- introduced stricter duplicate validation,
    
- created reconciliation scripts for impacted records,
    
- and reduced retry aggressiveness across dependent services.
    

I then worked on implementing centralized idempotency handling using request tokens, deduplication persistence, transactional checkpoints, and safer retry behavior for async workflows.

We also improved timeout hierarchy configuration between services to reduce cascading retries.

---

## Result

We eliminated duplicate workflow execution scenarios for the affected flows and significantly improved overall reliability during transient failures and retries.

Following the incident, we added:

- idempotency metrics,
    
- retry tracing,
    
- duplicate detection alerts,
    
- and workflow reconciliation dashboards.
    

The incident became an important engineering learning around distributed-system reliability, especially the fact that successful HTTP responses alone do not guarantee correctness in asynchronous systems.
