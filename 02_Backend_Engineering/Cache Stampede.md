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
> When a huge number of items in cache gets timed out almost at same time, all tries to refresh it's value from DB at the same time, which consumes all the thread and blocks db from serving to user request. Sometimes it could lead to system crash.  

## Internal Mechanics
There are 4 ways to fix this cache stampede:
#### 1. Cache locking
A lock lets 1 request rebuild the cache while other request wait in the line.




## Lifecycle / Runtime Flow

## Concurrency Implications

## Transactional Implications

## API Design Considerations

## Performance Implications

## Security Implications

## Common Bugs

## Production Learnings

## Monitoring / Debugging

## Real-World Use Cases

## Trade-Offs

| Approach | Pros | Cons |
|---|---|---|
|  |  |  |

## Related Notes

- [[ ]]

## Revision Notes
