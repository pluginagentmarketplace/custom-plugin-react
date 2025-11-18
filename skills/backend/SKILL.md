---
name: backend-development
description: Build APIs, work with databases, implement authentication, and design scalable backend systems. Use when developing server-side logic, handling data persistence, or designing system architectures.
---

# Backend Development

## Quick Start

### Node.js with Express:

```javascript
import express from 'express';
import cors from 'cors';

const app = express();
app.use(cors());
app.use(express.json());

// Routes
app.get('/api/users', (req, res) => {
  res.json({ users: [] });
});

app.post('/api/users', (req, res) => {
  const user = req.body;
  // Save to database
  res.status(201).json(user);
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

### Database query with SQL:

```sql
-- Create table
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  name VARCHAR(255),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Optimized query with index
CREATE INDEX idx_users_email ON users(email);

SELECT id, email, name FROM users
WHERE email = $1;
```

### JWT Authentication:

```javascript
import jwt from 'jsonwebtoken';

const SECRET = process.env.JWT_SECRET;

// Generate token
const token = jwt.sign(
  { userId: user.id, email: user.email },
  SECRET,
  { expiresIn: '7d' }
);

// Verify token
const decoded = jwt.verify(token, SECRET);
```

## Core Concepts

**API Design**
- REST principles and conventions
- HTTP methods and status codes
- Request/response serialization
- API versioning strategies
- Error handling and status codes

**Database Design**
- Normalization and schema design
- Indexing strategies
- Query optimization
- Transactions and ACID properties
- Connection pooling

**Authentication & Authorization**
- JWT (JSON Web Tokens)
- OAuth 2.0 flows
- Session management
- Role-based access control (RBAC)
- Encryption and hashing

**Scalability**
- Stateless design principles
- Caching strategies (Redis)
- Database replication and sharding
- Load balancing
- Microservices patterns

## Best Practices

1. **Code Organization**
   - MVC or similar architectural pattern
   - Separation of concerns
   - Dependency injection
   - Clear folder structure

2. **Security**
   - Input validation and sanitization
   - SQL injection prevention
   - CSRF protection
   - Rate limiting
   - HTTPS enforcement

3. **Performance**
   - Query optimization
   - Proper indexing
   - Connection pooling
   - Caching layer (Redis, Memcached)
   - Batch operations

4. **Error Handling**
   - Consistent error response format
   - Proper HTTP status codes
   - Logging and monitoring
   - Graceful degradation

## Common Patterns

**Middleware pattern:**
```javascript
const authenticate = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.status(401).json({ error: 'Unauthorized' });

  try {
    const decoded = jwt.verify(token, SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    res.status(403).json({ error: 'Invalid token' });
  }
};

app.get('/api/protected', authenticate, (req, res) => {
  res.json({ user: req.user });
});
```

**Repository pattern:**
```javascript
class UserRepository {
  async findById(id) {
    return db.query('SELECT * FROM users WHERE id = $1', [id]);
  }

  async create(userData) {
    return db.query(
      'INSERT INTO users (email, name) VALUES ($1, $2) RETURNING *',
      [userData.email, userData.name]
    );
  }
}
```

## Resources

- **Node.js**: nodejs.org/docs
- **Express**: expressjs.com
- **Databases**: postgresql.org, mongodb.com
- **Testing**: jest.io, supertest
- **APIs**: restfulapi.net, swagger.io
