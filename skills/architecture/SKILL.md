---
name: system-architecture
description: System design, scalability patterns, microservices, databases, caching, APIs, performance, cost optimization.
---

# System Architecture Skills

## Load Balancing

```python
import hashlib

class ConsistentHash:
    def __init__(self, num_nodes, virtual_nodes=150):
        self.num_nodes = num_nodes
        self.virtual_nodes = virtual_nodes
        self.ring = {}
        self.sorted_keys = []
        self._build_ring()
    
    def _hash(self, key):
        return int(hashlib.md5(str(key).encode()).hexdigest(), 16)
    
    def _build_ring(self):
        for node in range(self.num_nodes):
            for i in range(self.virtual_nodes):
                virtual_key = f"{node}:{i}"
                hash_value = self._hash(virtual_key)
                self.ring[hash_value] = node
        
        self.sorted_keys = sorted(self.ring.keys())
    
    def get_node(self, key):
        hash_value = self._hash(key)
        
        for ring_key in self.sorted_keys:
            if hash_value <= ring_key:
                return self.ring[ring_key]
        
        return self.ring[self.sorted_keys[0]]
```

## Database Sharding

```python
class ShardManager:
    def __init__(self, num_shards):
        self.num_shards = num_shards
    
    def get_shard(self, user_id):
        """Range-based sharding"""
        return hash(user_id) % self.num_shards
    
    def get_shard_range(self, shard_id):
        """Get range of IDs for a shard"""
        shard_size = 2**64 // self.num_shards
        return (shard_id * shard_size, (shard_id + 1) * shard_size)

# Usage
shards = ShardManager(num_shards=4)
user_id = 12345
shard = shards.get_shard(user_id)
# Route query to shard_db[shard]
```

## Caching Layer Strategy

```python
class CacheLayer:
    def __init__(self, db):
        self.db = db
        self.cache = {}
        self.ttl = 3600
    
    def get(self, key):
        # L1: In-memory cache
        if key in self.cache:
            if not self._is_expired(key):
                return self.cache[key]
            del self.cache[key]
        
        # L2: Database
        value = self.db.get(key)
        if value:
            self._set_cache(key, value)
        return value
    
    def set(self, key, value):
        self.db.set(key, value)
        self._set_cache(key, value)
    
    def _set_cache(self, key, value):
        self.cache[key] = {'value': value, 'expires': time() + self.ttl}
    
    def _is_expired(self, key):
        return time() > self.cache[key]['expires']
```

## Rate Limiting

```python
class RateLimiter:
    def __init__(self, rate: int, capacity: int):
        self.rate = rate  # tokens per second
        self.capacity = capacity
        self.tokens = capacity
        self.last_update = time()
    
    def allow_request(self, weight: int = 1) -> bool:
        self._refill()
        
        if self.tokens >= weight:
            self.tokens -= weight
            return True
        
        return False
    
    def _refill(self):
        now = time()
        elapsed = now - self.last_update
        self.tokens = min(self.capacity, self.tokens + elapsed * self.rate)
        self.last_update = now
```

