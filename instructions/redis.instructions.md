---
applyTo: '**/redis/**,**/cache/**,**/*redis*,**/*cache*'
---

# Redis Conventions

## Key Design

- Namespaced keys: `<entity>:<id>` or `<entity>:<scope>:<id>` — e.g., `user:42`, `session:abc123`
- Never use generic keys like `cache` or `data`
- Use consistent separators — pick `:` and stick with it

## Always Set TTL

Every key written to Redis must have a TTL — no exceptions:

```python
# Good — expires in 1 hour
r.setex("product:123", 3600, json.dumps(product))

# Bad — key never expires
r.set("product:123", json.dumps(product))
```

Eviction policy `allkeys-lru` is a safety net, not a design strategy.

## Cache-Aside Pattern

Load-on-miss, set-on-load, invalidate-on-write:

1. Read from Redis
2. On miss: read from DB, write to Redis with TTL, return
3. On DB write: `DEL` the cache key or set it to the new value

## Atomic Operations

- Use `INCR` / `DECR` for counters — single atomic operation
- Use Lua scripts for multi-step atomic operations when `MULTI/EXEC` is not enough
- `SETNX` + `EXPIRE` for distributed locks, or use the `Redlock` algorithm for multi-node

## Rate Limiting Pattern

```python
def rate_limit(user_id: str, limit: int, window_seconds: int) -> bool:
    key = f"ratelimit:{user_id}:{int(time.time() // window_seconds)}"
    count = r.incr(key)
    if count == 1:
        r.expire(key, window_seconds)
    return count <= limit
```

## Connection Pooling

- Always use a connection pool — never create a new connection per request
- Set pool size based on concurrency, not arbitrarily large
- Configure `socket_keepalive=True` and `socket_timeout` to detect dead connections

## Security

- `requirepass` or ACL users in production — never an open Redis on a public network
- Do not store plaintext passwords or PII in Redis
- Enable TLS for Redis connections outside the local network
