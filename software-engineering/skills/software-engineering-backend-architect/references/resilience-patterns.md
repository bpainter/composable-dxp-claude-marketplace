---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[backend-architect]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Resilience & Failure Patterns (2025)

## Cascading Failures Prevention

### Problem
Service A depends on Service B. B gets slow. A times out while waiting for B. A's thread pool is exhausted. A is now unavailable. C depends on A. C fails. Cascade.

### Solutions

#### 1. Timeouts
Always set timeouts. Never wait indefinitely.

```python
# Good
response = requests.get(url, timeout=5)  # Fail fast

# Bad
response = requests.get(url)  # Can wait forever
```

#### 2. Circuit Breaker
Stop calling failing service, fail fast, try again later.

```
State: CLOSED (normal) → OPEN (failing) → HALF_OPEN (recovery)

CLOSED: Requests pass through, count failures
OPEN (after threshold): Fast fail, no requests sent
HALF_OPEN: Send test request, move to CLOSED or OPEN based on result
```

#### 3. Bulkhead Pattern
Isolate resources. One failing service doesn't starve others.

```
Thread pool A (for service A): 10 threads
Thread pool B (for service B): 10 threads
Thread pool C (for service C): 10 threads

If B exhausts its threads, A and C still have resources
```

#### 4. Retry with Exponential Backoff
Retry failed requests with increasing delays.

```python
def retry_with_backoff(fn, max_retries=5):
    for attempt in range(max_retries):
        try:
            return fn()
        except Exception as e:
            if attempt == max_retries - 1:
                raise
            wait_time = 2 ** attempt  # 1s, 2s, 4s, 8s, 16s
            time.sleep(wait_time + random.random())  # Jitter
```

## Request Deduplication (Idempotency)

Distributed systems can issue requests twice (network timeouts, retries). Idempotent operations are safe to repeat.

```
POST /payments
{
  "amount": 100,
  "idempotency_key": "order-123-payment-1"  ← Unique key
}

// Server checks: have I processed this key before?
// If yes: return cached response (no double charge)
// If no: process and cache result
```

## Graceful Degradation

When a service fails, degrade features instead of returning 500 errors.

```
Normal: Show user recommendations (calls recommendation service)
Degraded: Show static "popular items" instead (no service call)
Failed: Show empty state with helpful message

// Always return 200 OK with degraded content, don't cascade failure
```

## Health Checks & Readiness Probes

```
/health → Service is alive (can respond)
/ready → Service is ready to accept traffic (deps are up, migrations done)

Load balancer removes non-ready instances from rotation
Auto-scaler doesn't spin up if new instance isn't ready
```

## Rate Limiting & Quota

Prevent one client from overwhelming server.

```
Per-IP rate limit: 1000 requests/minute
Per-user quota: 10,000 requests/day
Per-API-key tier: 100 QPS (standard), 1000 QPS (premium)
```

Return 429 Too Many Requests when exceeded.

## 2025 Best Practices

- **Fail open vs. closed**: For availability, fail open (allow degraded service). For security, fail closed (deny access).
- **Observability first**: Log every failure, decision, retry. Make debugging possible.
- **Test failures**: Chaos engineering in production (small blast radius). Know your limits.
- **Document SLOs**: Define "successful" response rate and latency targets.
