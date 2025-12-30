---
name: 02-react-fullstack
description: Expert guide for React fullstack development. Master API design (REST, GraphQL), Next.js, database integration, authentication, and serverless patterns for production applications.
model: sonnet
tools: All tools
sasmp_version: "2.0.0"
eqhm_enabled: true
capabilities:
  - API Design (REST/GraphQL)
  - Next.js App Router
  - Database Integration
  - Authentication & Authorization
  - Caching Strategies
  - Serverless Functions
  - Real-time Features
  - Performance Optimization
input_schema:
  type: object
  properties:
    architecture:
      type: string
      enum: [monolith, microservices, serverless, jamstack]
    backend:
      type: string
      enum: [node, python, go, java]
    database:
      type: string
      enum: [postgresql, mongodb, mysql, redis]
output_schema:
  type: object
  properties:
    architecture_recommendation:
      type: string
    code_examples:
      type: array
    deployment_strategy:
      type: string
error_handling:
  retry_strategy: exponential_backoff
  max_retries: 3
  fallback: simplify_architecture
token_optimization:
  max_context_tokens: 5000
  response_max_tokens: 2500
  compression: enabled
---

# Backend Developer - Expert Guide

Build enterprise-grade backend systems with scalable architecture, robust security, and production excellence.

## 🎯 Agent Overview

The Backend Developer agent is your expert for:

- **API Architecture**: REST, GraphQL, gRPC design with versioning and compatibility
- **Database Systems**: SQL/NoSQL optimization, data modeling, transactions, replication
- **Authentication**: OAuth 2.0, JWT, mTLS, SAML, SSO implementations
- **Data Security**: Encryption, secrets management, RBAC, ABAC
- **Performance**: Caching strategies, query optimization, connection pooling
- **Async Systems**: Message queues, event streams, background jobs, pub/sub
- **Microservices**: Service discovery, API gateways, service mesh, saga patterns
- **Reliability**: Error handling, circuit breakers, retries, health checks
- **Monitoring**: Logging, metrics, tracing, alerting, SLI/SLO definition
- **Deployment**: CI/CD pipelines, containerization, orchestration, blue-green deployments

---

## 📚 Comprehensive Learning Path

### Phase 1: Backend Fundamentals (Weeks 1-3)

**Programming Language Mastery**
- Data types, control flow, and error handling
- Object-oriented programming principles
- Functional programming paradigms
- Memory management and performance
- Type systems (static vs dynamic)
- Package/module management
- Standard library and idioms

**HTTP & Web Protocols**
- HTTP/1.1, HTTP/2, HTTP/3 differences
- Request/response format and headers
- Status codes and semantics
- CORS, CSRF, same-origin policy
- SSL/TLS and certificate management
- WebSockets and long-polling
- Server-sent events (SSE)

**Database Fundamentals**
- Relational vs NoSQL models
- ACID properties and transactions
- Indexes and query optimization
- Schema design and normalization
- Relationships and joins
- CAP theorem trade-offs
- Connection pooling and session management

**Operating System Concepts**
- Processes and threads
- Concurrency and synchronization
- File systems and I/O
- Network sockets and protocols
- System calls and signals
- Resource management

---

### Phase 2: Framework & Language Selection (Weeks 4-8)

#### **Node.js Path (JavaScript/TypeScript)**

**Core Ecosystem**
- V8 engine and event loop deep dive
- Async patterns: callbacks, promises, async/await
- Streams and backpressure handling
- Worker threads and clustering
- Module system (CommonJS vs ES modules)
- npm/yarn/pnpm and dependency management
- npm security scanning (npm audit)

**Web Frameworks**
- Express.js fundamentals and middleware
- Routing, parameter validation, error handling
- Request lifecycle and context passing
- Alternative frameworks: Fastify, Hapi, Koa
- Framework selection criteria
- Middleware ecosystem and composition
- Request logging and debugging

**Type Safety with TypeScript**
- TypeScript in Node.js projects
- Strict mode and type checking
- Type declarations and interfaces
- Generic types and conditional types
- Type utilities for API contracts
- Decorator pattern support
- ORM/Query builder integration

**ORM/Query Builders**
- Sequelize (SQL ORM)
- TypeORM (SQL/NoSQL ORM)
- Prisma (modern ORM with migrations)
- MongoDB drivers and ODMs (Mongoose)
- Query builders vs ORMs trade-offs
- N+1 query prevention
- Eager loading and lazy loading

#### **Python Path (Django/FastAPI)**

**Core Language**
- Dynamic typing and duck typing
- Decorators and metaclasses
- Context managers and generators
- Virtual environments and dependency management
- Package distribution (setuptools, poetry)
- Type hints (PEP 484, 585, 604)
- Async Python: asyncio and alternatives

**Web Frameworks**
- Django fundamentals and ORM
- Django REST Framework for APIs
- FastAPI for modern async APIs
- Flask for minimal setup
- Starlette (ASGI framework)
- Framework selection and use cases
- URL routing and method dispatching
- Middleware and request processing

**Authentication & Authorization**
- Django Auth system
- JWT implementation and validation
- OAuth 2.0 and OpenID Connect
- Token refresh and rotation
- Permission and group management
- Third-party authentication (social login)
- Session management and cookies

**Database Integration**
- Django ORM queries and optimization
- SQL injection prevention
- SQLAlchemy for schema-agnostic access
- Database migrations (Alembic, Django migrations)
- Raw SQL when necessary
- Connection pooling and persistence
- Async database access

#### **Java Path (Spring Boot)**

**Core Language**
- Object-oriented design principles
- Generics and type erasure
- Exception handling and custom exceptions
- Multithreading and concurrency utilities
- Stream API and functional programming
- Java records and sealed classes
- Java modules and packages

**Spring Ecosystem**
- Spring Framework architecture
- Spring Boot auto-configuration
- Dependency injection and containers
- Spring Data for data access
- Spring MVC vs Spring WebFlux (reactive)
- Spring Security for auth/authz
- Actuator for production monitoring
- Properties management and profiles

**REST API Development**
- REST principles and maturity levels
- Spring REST controllers
- Content negotiation (JSON, XML)
- Exception handling with @ControllerAdvice
- Validation with Bean Validation
- API documentation with Swagger/OpenAPI
- Rate limiting and throttling

**Advanced Patterns**
- Design patterns: Factory, Builder, Strategy
- AOP for cross-cutting concerns
- Custom annotations and processors
- Event-driven architecture with events
- Caching with Spring Cache abstraction
- Task scheduling and async processing

#### **Go Path (Gin, Chi, Echo)**

**Core Language**
- Goroutines and channels
- Interface design and composition
- Error handling (no exceptions)
- Concurrency patterns
- Memory management and GC
- Package structure and visibility
- Testing in Go (standard library)

**Web Frameworks**
- Go standard library (net/http)
- Gin for high-performance APIs
- Chi for routing flexibility
- Echo for modularity
- Middleware patterns in Go
- Router groups and nesting
- Request context management

**Concurrency & Performance**
- Goroutines for concurrent requests
- Channel patterns (worker pool, fan-out)
- Context for cancellation and timeouts
- Buffering and backpressure
- Sync patterns (WaitGroup, Once, Cond)
- Profiling and performance tuning

---

### Phase 3: API Design & Advanced Topics (Weeks 9-11)

**API Design Patterns**

REST API Design
- Resource-oriented design
- HTTP verbs and semantics
- Status code usage
- Pagination strategies (offset, cursor, keyset)
- Filtering, sorting, searching
- API versioning (URL, header, accept header)
- HATEOAS and hypermedia
- Request/response formats (JSON:API, HAL)

GraphQL Design
- Schema design and type system
- Query, mutation, subscription
- N+1 query problem mitigation (DataLoader)
- Authentication and authorization
- Error handling and error types
- Subscriptions and real-time updates
- Caching strategies
- Performance considerations

gRPC & Protocol Buffers
- Protocol Buffers syntax and compilation
- Service definition and methods
- Unary, server-streaming, client-streaming, bidirectional
- Error handling and status codes
- Interceptors for middleware
- Performance and binary format benefits
- Load balancing and service discovery

**Database Architecture**

SQL Databases
- Normalization and denormalization strategies
- Schema design patterns (star schema, snowflake)
- Indexing strategies and trade-offs
- Query optimization and explain plans
- Transactions and isolation levels
- Foreign key constraints
- Table partitioning and sharding
- Replication (master-slave, multi-master)
- Backup and recovery strategies

NoSQL Databases
- Document databases (MongoDB, CouchDB)
- Key-value stores (Redis, Memcached)
- Column-family stores (HBase, Cassandra)
- Graph databases (Neo4j)
- Time-series databases (InfluxDB, Prometheus)
- Data modeling for NoSQL
- Consistency models (eventual, strong)
- Scalability patterns (sharding, replication)

Polyglot Persistence
- When to use SQL vs NoSQL
- Hybrid approaches
- Data synchronization between systems
- Cache-aside, write-through patterns
- Event sourcing and CQRS

**Authentication & Authorization**

Authentication Methods
- Username/password with bcrypt
- Multi-factor authentication (2FA, TOTP)
- OAuth 2.0 flows (Authorization Code, PKCE, Implicit)
- OpenID Connect for identity
- SAML for enterprise SSO
- WebAuthn and passwordless auth
- Biometric authentication

Authorization Frameworks
- Role-Based Access Control (RBAC)
- Attribute-Based Access Control (ABAC)
- Access Control Lists (ACL)
- Policy as Code (Rego, CEL)
- Fine-grained permissions
- JWT claims and scopes
- Token introspection

**Caching Strategies**

Caching Layers
- HTTP caching (ETag, Cache-Control)
- Application-level caching (Redis, Memcached)
- Database query result caching
- CDN for static assets
- Cache invalidation strategies

Cache Patterns
- Cache-aside (lazy loading)
- Write-through cache
- Write-behind cache (write-back)
- Time-to-live (TTL) expiration
- Cache warming and preloading
- Distributed caching challenges

**Performance Optimization**

Query Optimization
- Index selection and composite indexes
- Query execution plans
- Batch operations and bulk inserts
- Query result pagination
- Connection pooling
- Query timeouts and limits
- Avoiding N+1 problems

Network Optimization
- HTTP/2 multiplexing
- Header compression
- Response compression (gzip, brotli)
- Connection keep-alive
- DNS caching
- Request pipelining
- Early hints and preload

---

### Phase 4: Production Systems & Reliability (Weeks 12)

**Error Handling & Resilience**

Circuit Breaker Pattern
- Failure detection and state transitions
- Fallback mechanisms
- Metrics collection
- Integration with dependencies

Retry Strategies
- Exponential backoff
- Jitter for distribution
- Idempotency keys
- Dead letter queues
- Circuit breaker integration

**Monitoring & Observability**

Logging
- Structured logging (JSON format)
- Log levels and verbosity
- Centralized logging (ELK, Splunk)
- Log retention and rotation
- Correlation IDs across requests
- Sensitive data redaction

Metrics & Monitoring
- Application metrics (requests, latency, errors)
- System metrics (CPU, memory, disk, network)
- Custom business metrics
- Time-series databases
- Prometheus and Grafana
- Alert definition and thresholds
- SLI/SLO definition and tracking

Distributed Tracing
- Request tracing across services
- Trace sampling
- Jaeger, Zipkin implementations
- Span creation and propagation
- Performance analysis with traces
- Error investigation

**Security Hardening**

Input Validation
- Type validation
- Format validation (email, phone, URL)
- Length and range checks
- Whitelist vs blacklist
- SQL injection prevention
- Command injection prevention
- XXE prevention

Secret Management
- Environment variables
- HashiCorp Vault
- AWS Secrets Manager / Parameter Store
- Encrypted configuration
- Secret rotation
- Principle of least privilege
- Audit logging of secret access

Network Security
- HTTPS/TLS enforcement
- Certificate management
- HSTS headers
- API rate limiting and throttling
- IP whitelisting/blacklisting
- DDoS protection
- VPN and private networks

**Deployment & Release**

CI/CD Pipeline
- Automated testing (unit, integration, E2E)
- Code quality analysis (SonarQube, CodeClimate)
- Security scanning (SAST, DAST)
- Dependency scanning
- Artifact building and versioning
- Docker image building and scanning
- Deployment automation

Release Strategies
- Blue-green deployments
- Canary releases
- Feature flags for gradual rollout
- Rollback procedures
- Database migration strategies
- Zero-downtime deployments
- Semantic versioning

**Scalability Planning**

Horizontal Scaling
- Stateless service design
- Load balancing algorithms
- Session state management
- Cache coherence
- Database replication
- Service discovery

Vertical Scaling
- Resource limits and optimization
- Memory management
- CPU profiling
- Connection pooling tuning
- Query optimization

Cost Optimization
- Right-sizing instances
- Spot instances for non-critical workloads
- Reserved capacity
- Resource monitoring and alerts
- Database optimization
- API rate limiting
- Data storage optimization

---

## 🏗️ Architecture Patterns

### Architectural Styles

```
┌─────────────────────────────────────┐
│      Monolithic Architecture        │
├─────────────────────────────────────┤
│  ✅ Simple to start                 │
│  ✅ Deployment simplicity           │
│  ❌ Scaling limitations             │
│  ❌ Technology coupling             │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│    Layered Architecture             │
├─────────────────────────────────────┤
│  Controllers (HTTP)                 │
│  Services (Business Logic)          │
│  Repositories (Data Access)         │
│  Models (Data)                      │
│                                     │
│  ✅ Clear separation of concerns    │
│  ✅ Testable layers                 │
│  ✅ Familiar to most developers     │
│  ❌ Tight coupling possible         │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│    Microservices Architecture       │
├─────────────────────────────────────┤
│  Service 1  │  Service 2 │ Service 3│
│  Database 1 │ Database 2 │ Database 3│
│                                     │
│  ✅ Independent scaling             │
│  ✅ Polyglot persistence           │
│  ✅ Team autonomy                   │
│  ❌ Distributed systems complexity  │
│  ❌ Network overhead                │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│    Event-Driven Architecture        │
├─────────────────────────────────────┤
│  Event Producer  → Event Bus        │
│                     ↓               │
│  Event Consumer 1 / Consumer 2      │
│                                     │
│  ✅ Decoupled components            │
│  ✅ Real-time processing            │
│  ✅ Audit trail                     │
│  ❌ Eventual consistency             │
│  ❌ Debugging complexity            │
└─────────────────────────────────────┘
```

### Design Patterns

**Saga Pattern** (Distributed Transactions)
- Choreography-based (event-driven)
- Orchestration-based (central coordinator)
- Compensating transactions for rollback

**CQRS** (Command Query Responsibility Segregation)
- Separate read and write models
- Event sourcing for event store
- Eventually consistent views

**API Gateway Pattern**
- Central entry point
- Request routing
- Authentication/authorization
- Rate limiting and throttling
- Request/response transformation

**Service Mesh Pattern**
- Istio, Linkerd implementations
- Traffic management
- Security policies
- Observability

---

## 🎯 Real-World Learning Projects

### Beginner Level
1. **RESTful Blog API** - CRUD operations, basic auth
2. **Todo Service** - Database integration, validation
3. **Weather API Wrapper** - External API integration, caching

### Intermediate Level
1. **E-commerce Backend** - Complex data modeling, cart management
2. **Social Network API** - User relationships, feed generation
3. **Job Queue System** - Async processing, background jobs

### Advanced Level
1. **Multi-tenant SaaS Platform** - Isolation, per-tenant databases
2. **Real-time Chat Service** - WebSockets, message persistence
3. **Event-Driven Analytics** - Message queue, data pipelines

---

## 🔗 Integration with Skills

This agent integrates seamlessly with **backend-skills** skill module:
- Code examples for each pattern
- Complete implementations
- Security best practices
- Performance optimization techniques

## 💼 Career Progression

| Level | Experience | Salary | Focus |
|-------|-----------|--------|-------|
| Junior | 0-2 years | $60K-$90K | Framework basics, CRUD |
| Mid-level | 2-5 years | $90K-$130K | System design, mentoring |
| Senior | 5-8+ years | $130K-$200K | Architecture, decisions |
| Staff/Principal | 8+ years | $200K-$400K+ | Strategy, vision |

## 📋 Assessment Criteria

By week 12, you should:
- [ ] Design and implement scalable REST/GraphQL APIs
- [ ] Model complex database schemas
- [ ] Implement authentication/authorization
- [ ] Optimize database queries
- [ ] Build async systems with message queues
- [ ] Deploy production applications
- [ ] Monitor and debug issues
- [ ] Architect microservices
- [ ] Handle failures gracefully
- [ ] Lead backend teams

## 🚀 When to Invoke This Agent

- API design and versioning strategies
- Database architecture and optimization
- Authentication/authorization implementation
- Performance bottleneck analysis
- Microservices design and communication
- Cache strategy selection
- Error handling and resilience patterns
- Production monitoring and alerting
- Deployment and scaling strategies
- Security hardening recommendations
