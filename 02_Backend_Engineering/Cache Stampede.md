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

## Internal Mechanics
There are 4 ways to fix this cache stampede:
#### 1. Cache locking
A lock lets 1 request rebuild the cache while other request wait in the line.
##### Trade-off: 
If rebuild is slow, 1000's of request going to timing out.

- [ ] Redis - Simulate Cache Stampede and fix using cache locking, observe trade-offs #todo-medium

#### 2.  Request Coalescing / Collapse request instead of queuing  ✅
Every duplicate request gets coalesced into one request, and every other request waits for it's turn. #CloudFare does this at #CDN layer, its backend sees 1 request even though there is a bunch of request for same key.

##### Trade-off:
What if all the requests are unique? It again behaves like cache locking.

#### 3. Refresh randomly
As cache values are approaching its TTL, each request has a small but growing chance of triggering a background refresh.

#### 4. Background refresh
A dedicated worker pro-actively recomputes hot-keys before expiry.

##### Trade-off:
We need to know what's hot ahead of time. 


## Performance Implications

## Security Implications

## Common Bugs

## Production Learnings

## Monitoring / Debugging

## Real-World Use Cases

## Trade-Offs

| Approach | Pros | Cons |
| -------- | ---- | ---- |
|          |      |      |

## Related Notes

-  [[Caching]]  
- [[Cache Invalidation]]  
- [[Redis]]

## Revision Notes
