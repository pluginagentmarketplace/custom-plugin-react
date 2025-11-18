---
name: system-architecture
description: Master system design, architecture patterns, scalability, performance optimization, and building distributed systems. Use when designing large systems, solving architectural problems, or evaluating technology choices.
---

# System Architecture & Design

## Quick Start

### High-level system design framework:

1. **Requirements clarification** (5 min)
   - Functional requirements (features, users, data)
   - Non-functional (QPS, latency, availability %)
   - Scale (concurrent users, data volume)
   - Constraints and assumptions

2. **Capacity estimation** (3 min)
   - DAU/MAU to QPS conversion
   - Storage needs (data × replication × backup)
   - Bandwidth calculations

3. **High-level design** (5 min)
   ```
   Client → Load Balancer → API Servers → Database
                        ↓
                    Cache Layer
   ```

4. **Deep dives** (10 min)
   - Database: SQL vs NoSQL, sharding strategy
   - Caching: Multi-tier, invalidation strategy
   - Messaging: Pub/sub or queues
   - Monitoring: Metrics, logging, alerting

### Database sharding example:

```
User ID: 12345

Shard Key: user_id % num_shards
Shard Number: 12345 % 4 = 1

Route to: shard_1 database
```

**Hash sharding:**
```python
def get_shard(user_id, num_shards):
    return hash(user_id) % num_shards

# Distribute uniformly across shards
shard_id = get_shard(user_id, 4)  # Returns 0-3
connection = db_connections[shard_id]
```

### Load balancing with health checks:

```python
class LoadBalancer:
    def __init__(self, servers):
        self.servers = servers
        self.current = 0

    def get_next_healthy_server(self):
        attempts = 0
        while attempts < len(self.servers):
            server = self.servers[self.current]
            self.current = (self.current + 1) % len(self.servers)

            if server.is_healthy():
                return server
            attempts += 1

        raise Exception("No healthy servers")

    def route_request(self, request):
        server = self.get_next_healthy_server()
        return server.handle(request)
```

## Core Concepts

**Scalability Dimensions**
- Horizontal scaling: Add more servers
- Vertical scaling: Bigger servers
- Database scaling: Read replicas, sharding
- Caching layers: Redis, Memcached
- CDN for static content

**Consistency & Availability**
- CAP theorem: Choose 2 of 3
  - Consistency: All reads get latest data
  - Availability: System always responsive
  - Partition tolerance: Works despite network failures
- Eventual consistency for high availability
- Strong consistency for data correctness

**Performance Optimization**
- Caching strategy (cache-aside, write-through)
- Database query optimization
- Connection pooling
- Compression (gzip, brotli)
- Async processing with message queues

**Reliability**
- Redundancy (no single point of failure)
- Replication for data durability
- Failover mechanisms
- Circuit breakers for cascading failures
- Monitoring and alerting

## Architecture Patterns

| Pattern | Use Case | Trade-offs |
|---------|----------|-----------|
| **Monolith** | MVP, small scale | Simple but inflexible |
| **Microservices** | Large, diverse teams | Complex, distributed tracing needed |
| **Serverless** | Event-driven, sparse load | Cold start, cost |
| **Event-Driven** | Real-time, decoupled | Eventual consistency |
| **CQRS** | Read-heavy workloads | Complex, data sync |

## Design Principles

1. **Fail Gracefully**
   - Circuit breaker pattern
   - Graceful degradation
   - Fallback mechanisms

2. **Optimize for Cost**
   - Reserved instances vs on-demand
   - Batch processing off-peak
   - Spot instances for flexible workloads

3. **Security First**
   - Encryption in transit and at rest
   - API rate limiting
   - Input validation
   - Least privilege access

4. **Observability**
   - Structured logging
   - Metrics (latency, throughput, errors)
   - Distributed tracing
   - Alerting rules

## Common Patterns

**Circuit breaker pattern:**
```python
class CircuitBreaker:
    def __init__(self, failure_threshold=5, timeout=60):
        self.failure_threshold = failure_threshold
        self.timeout = timeout
        self.failure_count = 0
        self.last_failure_time = None
        self.state = "CLOSED"  # CLOSED, OPEN, HALF_OPEN

    def call(self, func, *args, **kwargs):
        if self.state == "OPEN":
            if time.time() - self.last_failure_time > self.timeout:
                self.state = "HALF_OPEN"
            else:
                raise Exception("Circuit breaker is OPEN")

        try:
            result = func(*args, **kwargs)
            if self.state == "HALF_OPEN":
                self.state = "CLOSED"
                self.failure_count = 0
            return result
        except Exception as e:
            self.failure_count += 1
            self.last_failure_time = time.time()
            if self.failure_count >= self.failure_threshold:
                self.state = "OPEN"
            raise
```

**Cache-aside pattern:**
```python
def get_user(user_id):
    # Check cache first
    cached = cache.get(f"user:{user_id}")
    if cached:
        return cached

    # Cache miss, fetch from database
    user = db.get_user(user_id)
    if user:
        cache.set(f"user:{user_id}", user, ttl=3600)
    return user
```

**Rate limiting with token bucket:**
```python
class RateLimiter:
    def __init__(self, rate, capacity):
        self.rate = rate  # tokens per second
        self.capacity = capacity  # max tokens
        self.tokens = capacity
        self.last_refill = time.time()

    def allow_request(self):
        now = time.time()
        elapsed = now - self.last_refill
        self.tokens = min(
            self.capacity,
            self.tokens + elapsed * self.rate
        )
        self.last_refill = now

        if self.tokens >= 1:
            self.tokens -= 1
            return True
        return False
```

## Resources

- **Book**: Designing Data-Intensive Applications
- **Interview**: System Design Interview Course
- **Case Studies**: Google, Netflix, Amazon architecture blogs
- **Tools**: Lucidchart, draw.io for diagrams
- **Learning**: educative.io, grokking.org system design courses
