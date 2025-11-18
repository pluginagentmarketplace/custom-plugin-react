---
name: devops-infrastructure
description: Master Docker, Kubernetes, CI/CD pipelines, Infrastructure as Code, and cloud deployment. Use when containerizing applications, setting up orchestration, or automating infrastructure.
---

# DevOps & Infrastructure

## Quick Start

### Docker basics:

```dockerfile
# Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

Build and run:
```bash
docker build -t my-app:1.0 .
docker run -p 3000:3000 my-app:1.0
```

### Docker Compose for development:

```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      DATABASE_URL: postgres://db:5432/mydb
    depends_on:
      - db

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: mydb
      POSTGRES_PASSWORD: password
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
```

### Kubernetes basic deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app:1.0
        ports:
        - containerPort: 3000
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
---
apiVersion: v1
kind: Service
metadata:
  name: my-app
spec:
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 3000
  type: LoadBalancer
```

## Core Concepts

**Containerization**
- Docker images and containers
- Dockerfile best practices
- Image optimization and layering
- Registry management
- Container networking and volumes

**Orchestration**
- Kubernetes architecture and concepts
- Pods, Services, Deployments
- ConfigMaps and Secrets
- StatefulSets and DaemonSets
- Persistent volumes and claims

**Infrastructure as Code**
- Terraform basics and modules
- AWS/GCP/Azure with IaC
- State management
- Version control for infrastructure
- Planning and applying changes

**CI/CD Pipelines**
- GitHub Actions workflows
- Jenkins declarative pipelines
- GitLab CI configuration
- Build, test, and deploy stages
- Artifact management

## Best Practices

1. **Container Hygiene**
   - Minimal base images (Alpine)
   - Single responsibility per container
   - No secrets in images
   - Regular updates and patching

2. **Kubernetes Practices**
   - Resource requests and limits
   - Health checks (liveness, readiness)
   - Rolling updates and rollbacks
   - Namespace isolation
   - Network policies

3. **Infrastructure**
   - Infrastructure as code for all resources
   - Immutable infrastructure
   - Infrastructure versioning
   - Automated testing
   - Change management process

4. **Security**
   - Image scanning for vulnerabilities
   - Secrets management (not in code)
   - RBAC and network policies
   - Regular security audits
   - Compliance and governance

## Common Patterns

**Multi-stage Docker build:**
```dockerfile
# Build stage
FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY package*.json ./
RUN npm ci --only=production
CMD ["node", "dist/index.js"]
```

**Terraform module:**
```hcl
# main.tf
resource "aws_ecs_cluster" "main" {
  name = var.cluster_name
}

resource "aws_ecs_service" "app" {
  name            = var.service_name
  cluster         = aws_ecs_cluster.main.id
  task_definition = aws_ecs_task_definition.app.arn
  desired_count   = var.desired_count
}
```

**GitHub Actions CI/CD:**
```yaml
name: CI/CD

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run test
      - run: npm run build
      - name: Deploy
        run: npm run deploy
```

## Resources

- **Docker**: docs.docker.com
- **Kubernetes**: kubernetes.io/docs
- **Terraform**: terraform.io/docs
- **AWS**: docs.aws.amazon.com
- **GitHub Actions**: github.com/features/actions
