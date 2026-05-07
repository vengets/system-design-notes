---
created: 2026-05-08 04:32
updated: 2026-05-08 04:32
tags:
  - backend
  - "#cache"
  - "#issue"
status: active
language:
source:
related:
url: https://www.youtube.com/shorts/MPFkGLZGe9M?feature=share
---


> [!info]
> Backend engineering concept or implementation pattern.

## Purpose

Cache stampede happens when many requests observe the same cache key as expired or missing and all try to recompute it from the database at the same time. This can exhaust application threads, overload the database, increase latency, and sometimes trigger cascading failure.

## Mitigation Strategies

### 1. Request Coalescing / Single Flight

Only one request regenerates the cache while duplicate requests wait for the same result.

This prevents multiple concurrent requests from overwhelming the backend for the same key.

Examples:
- Go singleflight
- CDN request collapsing
- Cloudflare edge caching

#### Trade-Offs
- Unique requests still hit backend independently
- Waiting requests may timeout if regeneration is slow

---

### 2. Cache Locks

A distributed or local lock ensures only one worker rebuilds the cache entry.

Other requests either:
- wait
- retry later
- return stale data

#### Trade-Offs
- Lock contention
- Risk of deadlocks
- Slow regeneration increases latency

---

### 3. Background Refresh / Cache Warming

Hot keys are refreshed proactively before expiration using workers or scheduled jobs.

This avoids sudden synchronized expiration.

#### Trade-Offs
- Requires identifying hot keys
- Extra infrastructure complexity
- Possible wasted recomputation

---

### 4. Probabilistic Early Expiration

Requests gradually refresh cache before TTL expiry using randomized probability.

This spreads regeneration over time.

#### Trade-Offs
- More complex implementation
- Possible unnecessary refreshes

---

### 5. Exponential Backoff

When regeneration is already happening, retry attempts use randomized delays.

This reduces synchronized retry storms. Usually used together with [[#2. Cache Locks]].

#### Trade-Offs
- Increased latency
- More operational tuning required

---

### 6. Stale-While-Revalidate

Serve stale cached data temporarily while refreshing asynchronously in background.

Improves availability during regeneration.

#### Trade-Offs
- Clients may observe stale data
- Requires careful expiration semantics
## Related Notes

-  [[Caching]]  
- [[Cache Invalidation]]  
- [[Redis]]

## Revision Notes

- [ ]  Redis - Simulate Cache Stampede and fix using cache locking, observe trade-offs #todo-medium