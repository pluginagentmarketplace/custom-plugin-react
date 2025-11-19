---
name: backend-development
description: Master API design, database architecture, authentication, caching, microservices, and deployment. Build scalable, secure, performant backend systems.
---

# Backend Development Skills

## REST API Design

### Resource-Oriented Design
```javascript
// Good: Resource-oriented endpoints
GET /api/v1/users                    // List users
POST /api/v1/users                   // Create user
GET /api/v1/users/:id                // Get specific user
PUT /api/v1/users/:id                // Update user
DELETE /api/v1/users/:id             // Delete user
GET /api/v1/users/:id/orders         // User's orders
POST /api/v1/users/:id/orders        // Create order

// Proper HTTP semantics
POST   /api/users              // 201 Created
GET    /api/users/:id          // 200 OK or 404 Not Found
PUT    /api/users/:id          // 200 OK or 204 No Content
DELETE /api/users/:id          // 204 No Content
```

### Error Handling
```javascript
const errorHandler = (err, req, res, next) => {
  const status = err.status || 500;
  const message = err.message || 'Internal Server Error';
  
  res.status(status).json({
    error: {
      status,
      message,
      timestamp: new Date().toISOString(),
      path: req.path,
      ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
    }
  });
};
```

## Database Optimization

### Query Performance
```sql
-- Proper indexing for common queries
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_orders_user_id_date ON orders(user_id, created_at DESC);

-- Efficient query with EXPLAIN
EXPLAIN ANALYZE
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.created_at > NOW() - INTERVAL '30 days'
GROUP BY u.id;

-- Avoid N+1 queries
-- Bad:
users.forEach(u => {
  const orders = query('SELECT * FROM orders WHERE user_id = ?', u.id);
});

-- Good:
const users = query('SELECT * FROM users');
const ordersByUser = query('SELECT user_id, * FROM orders WHERE user_id IN (?)', userIds);
```

### Connection Pooling
```javascript
const pool = new Pool({
  max: 20,
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
});

// Reuse connections
const client = await pool.connect();
try {
  const result = await client.query('SELECT * FROM users');
} finally {
  client.release();
}
```

## Authentication & Security

### JWT Implementation
```javascript
const jwt = require('jsonwebtoken');

const generateToken = (user) => {
  return jwt.sign(
    { userId: user.id, email: user.email, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: '7d', issuer: 'app', audience: 'web' }
  );
};

const verifyToken = (token) => {
  try {
    return jwt.verify(token, process.env.JWT_SECRET, {
      issuer: 'app',
      audience: 'web'
    });
  } catch (err) {
    throw new AuthError('Invalid token');
  }
};
```

### Secure Password Handling
```javascript
const bcrypt = require('bcrypt');

// Hash password
const hashPassword = async (password) => {
  const salt = await bcrypt.genSalt(12);
  return bcrypt.hash(password, salt);
};

// Verify password
const verifyPassword = async (password, hash) => {
  return bcrypt.compare(password, hash);
};

// Never store plaintext, always use bcrypt
```

## Caching Strategies

### Redis Cache Pattern
```javascript
const redis = require('redis');
const client = redis.createClient();

async function getUserWithCache(userId) {
  // Try cache first
  const cached = await client.get(`user:${userId}`);
  if (cached) return JSON.parse(cached);

  // Cache miss, fetch from DB
  const user = await db.query('SELECT * FROM users WHERE id = ?', userId);
  
  // Cache for 1 hour
  await client.setEx(`user:${userId}`, 3600, JSON.stringify(user));
  return user;
}

// Cache invalidation on update
async function updateUser(userId, data) {
  await db.query('UPDATE users SET ? WHERE id = ?', data, userId);
  await client.del(`user:${userId}`);
}
```

### Cache-Aside Pattern
```javascript
async function getDataWithFallback(key, fetchFn, ttl = 3600) {
  try {
    // Try cache
    const cached = await cache.get(key);
    if (cached) return JSON.parse(cached);
  } catch (err) {
    console.error('Cache error:', err);
  }

  // Fetch from source
  const data = await fetchFn();
  
  // Update cache
  try {
    await cache.setEx(key, ttl, JSON.stringify(data));
  } catch (err) {
    console.error('Cache write error:', err);
  }

  return data;
}
```

## Microservices Patterns

### Service Discovery
```javascript
// Register service
const consul = require('consul');
const consulClient = new consul();

await consulClient.agent.service.register({
  id: 'user-service-1',
  name: 'user-service',
  address: 'localhost',
  port: 3000,
  check: {
    http: 'http://localhost:3000/health',
    interval: '10s',
    timeout: '5s'
  }
});

// Discover service
const services = await consulClient.health.service({
  service: 'user-service',
  passing: true
});
```

### Circuit Breaker Pattern
```javascript
class CircuitBreaker {
  constructor(fn, opts = {}) {
    this.fn = fn;
    this.failures = 0;
    this.threshold = opts.threshold || 5;
    this.timeout = opts.timeout || 60000;
    this.state = 'CLOSED';
  }

  async execute(...args) {
    if (this.state === 'OPEN') {
      if (Date.now() - this.lastFailTime > this.timeout) {
        this.state = 'HALF_OPEN';
      } else {
        throw new Error('Circuit breaker is OPEN');
      }
    }

    try {
      const result = await this.fn(...args);
      this.onSuccess();
      return result;
    } catch (err) {
      this.onFailure();
      throw err;
    }
  }

  onSuccess() {
    this.failures = 0;
    this.state = 'CLOSED';
  }

  onFailure() {
    this.failures++;
    this.lastFailTime = Date.now();
    if (this.failures >= this.threshold) {
      this.state = 'OPEN';
    }
  }
}
```

## API Rate Limiting

### Token Bucket Algorithm
```javascript
class RateLimiter {
  constructor(capacity, refillRate) {
    this.capacity = capacity;
    this.tokens = capacity;
    this.refillRate = refillRate;
    this.lastRefillTime = Date.now();
  }

  allowRequest() {
    this.refill();
    if (this.tokens >= 1) {
      this.tokens--;
      return true;
    }
    return false;
  }

  refill() {
    const now = Date.now();
    const timePassed = (now - this.lastRefillTime) / 1000;
    const tokensToAdd = timePassed * this.refillRate;
    this.tokens = Math.min(this.capacity, this.tokens + tokensToAdd);
    this.lastRefillTime = now;
  }
}
```

## Monitoring & Logging

### Structured Logging
```javascript
const winston = require('winston');

const logger = winston.createLogger({
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

logger.info('User login', {
  userId: 123,
  timestamp: new Date(),
  ip: req.ip,
  userAgent: req.get('user-agent')
});
```

### Health Checks
```javascript
app.get('/health', (req, res) => {
  res.json({
    status: 'ok',
    timestamp: new Date(),
    uptime: process.uptime(),
    database: await checkDatabase(),
    cache: await checkCache()
  });
});
```

