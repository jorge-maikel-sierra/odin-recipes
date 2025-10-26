---
mode: 'agent'
description: 'Crear estructura completa de API con estándares industriales'
---

# Scaffold API Project

Tu objetivo es crear una estructura completa de API siguiendo los estándares más altos de la industria.

## 📋 Información Requerida

Solicita la siguiente información si no se proporciona:

- **Runtime**: ${input:runtime:Node.js (Express/NestJS) | Python (FastAPI/Django) | Go (Gin/Echo)}
- **Framework**: ${input:framework}
- **Base de datos**: ${input:database:PostgreSQL | MongoDB | MySQL}
- **Cache**: ${input:cache:Redis | Memcached}
- **Autenticación**: ${input:auth:JWT | OAuth2 | SAML}
- **Deployment**: ${input:deploy:Docker | Kubernetes | Serverless}

## 🏗️ Estructura del Proyecto

Genera la siguiente estructura de carpetas:

```
${workspaceFolder}/
├── .github/                          # GitHub Actions y configuración
│   ├── workflows/
│   │   ├── ci.yml                   # Pipeline CI/CD
│   │   ├── security.yml             # Security scanning
│   │   └── deploy.yml               # Deployment pipeline
│   ├── copilot-instructions.md      # Instrucciones del proyecto
│   ├── chatmodes/                   # Modos especializados
│   └── prompts/                     # Prompts reutilizables
├── src/
│   ├── controllers/                 # REST Controllers
│   ├── services/                    # Business Logic Layer
│   ├── models/                      # Data Models
│   ├── middleware/                  # Custom Middleware
│   ├── utils/                       # Utilities
│   ├── config/                      # Configuration
│   ├── types/                       # TypeScript Types
│   └── app.js|ts                    # Application Entry Point
├── tests/
│   ├── unit/                        # Unit Tests
│   ├── integration/                 # Integration Tests
│   ├── e2e/                        # End-to-End Tests
│   └── fixtures/                    # Test Data
├── docs/
│   ├── api/                         # API Documentation
│   ├── architecture/                # Architecture Docs
│   └── deployment/                  # Deployment Guides
├── docker/
│   ├── Dockerfile                   # Production Image
│   ├── Dockerfile.dev               # Development Image
│   └── docker-compose.yml           # Local Development
├── scripts/
│   ├── setup.sh                     # Project Setup
│   ├── migrate.sh                   # Database Migrations
│   └── deploy.sh                    # Deployment Script
├── infrastructure/                   # Infrastructure as Code
│   ├── terraform/                   # Terraform configs
│   └── k8s/                        # Kubernetes manifests
├── .env.example                     # Environment Variables Template
├── .gitignore                       # Git Ignore Rules
├── README.md                        # Project Documentation
└── package.json|requirements.txt|go.mod  # Dependencies
```

## 📄 Archivos Base Requeridos

### 1. Package Configuration

**Node.js (package.json)**:
```json
{
  "name": "api-project",
  "version": "1.0.0",
  "description": "Production-ready API with industry standards",
  "main": "src/app.js",
  "scripts": {
    "start": "node src/app.js",
    "dev": "nodemon src/app.js",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "lint": "eslint src/",
    "lint:fix": "eslint src/ --fix",
    "build": "tsc",
    "migrate": "knex migrate:latest",
    "seed": "knex seed:run"
  },
  "dependencies": {
    "express": "^4.18.2",
    "helmet": "^7.0.0",
    "cors": "^2.8.5",
    "express-rate-limit": "^6.7.0",
    "joi": "^17.9.2",
    "jsonwebtoken": "^9.0.0",
    "bcryptjs": "^2.4.3",
    "pg": "^8.11.0",
    "redis": "^4.6.7",
    "winston": "^3.9.0"
  },
  "devDependencies": {
    "jest": "^29.5.0",
    "supertest": "^6.3.3",
    "nodemon": "^2.0.22",
    "eslint": "^8.42.0"
  }
}
```

**Python (requirements.txt)**:
```txt
fastapi==0.100.0
uvicorn==0.22.0
sqlalchemy==2.0.15
alembic==1.11.1
psycopg2-binary==2.9.6
redis==4.5.5
python-jose[cryptography]==3.3.0
passlib[bcrypt]==1.7.4
python-multipart==0.0.6
pytest==7.3.1
pytest-asyncio==0.21.0
httpx==0.24.1
```

### 2. Environment Configuration

**.env.example**:
```env
# Application
NODE_ENV=development
PORT=3000
API_VERSION=v1

# Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=api_db
DB_USER=api_user
DB_PASSWORD=your_password

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# Authentication
JWT_SECRET=your_super_secret_jwt_key
JWT_EXPIRES_IN=7d

# External Services
STRIPE_SECRET_KEY=sk_test_...
SENDGRID_API_KEY=SG...

# Monitoring
LOG_LEVEL=info
SENTRY_DSN=https://...
```

### 3. Docker Configuration

**Dockerfile**:
```dockerfile
FROM node:18-alpine AS base
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production && npm cache clean --force

FROM base AS development
RUN npm ci
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]

FROM base AS production
COPY . .
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nextjs -u 1001
USER nextjs
EXPOSE 3000
CMD ["npm", "start"]
```

**docker-compose.yml**:
```yaml
version: '3.8'
services:
  api:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    depends_on:
      - postgres
      - redis
    volumes:
      - .:/app
      - /app/node_modules

  postgres:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: api_db
      POSTGRES_USER: api_user
      POSTGRES_PASSWORD: password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  postgres_data:
```

### 4. CI/CD Pipeline

**.github/workflows/ci.yml**:
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run linting
        run: npm run lint
      
      - name: Run tests
        run: npm run test:coverage
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test_db
      
      - name: Upload coverage
        uses: codecov/codecov-action@v3
        
      - name: Security audit
        run: npm audit --audit-level high
      
      - name: Build application
        run: npm run build

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Deploy to production
        run: |
          echo "Deploying to production..."
          # Add deployment commands here
```

## 🔧 Implementaciones Base

### 1. Express.js Application (src/app.js)

```javascript
const express = require('express');
const helmet = require('helmet');
const cors = require('cors');
const rateLimit = require('express-rate-limit');

const { errorHandler, notFound } = require('./middleware/errorHandler');
const authRoutes = require('./controllers/authController');
const userRoutes = require('./controllers/userController');
const logger = require('./utils/logger');

const app = express();

// Security middleware
app.use(helmet());
app.use(cors());

// Rate limiting
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});
app.use('/api/', limiter);

// Body parsing
app.use(express.json({ limit: '10mb' }));
app.use(express.urlencoded({ extended: true }));

// Health check
app.get('/health', (req, res) => {
  res.status(200).json({ status: 'OK', timestamp: new Date().toISOString() });
});

// API routes
app.use('/api/v1/auth', authRoutes);
app.use('/api/v1/users', userRoutes);

// Error handling
app.use(notFound);
app.use(errorHandler);

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  logger.info(`Server running on port ${PORT}`);
});

module.exports = app;
```

### 2. Error Handler Middleware

```javascript
// src/middleware/errorHandler.js
const logger = require('../utils/logger');

const errorHandler = (err, req, res, next) => {
  let error = { ...err };
  error.message = err.message;

  // Log error
  logger.error(err);

  // Mongoose bad ObjectId
  if (err.name === 'CastError') {
    const message = 'Resource not found';
    error = { message, statusCode: 404 };
  }

  // Mongoose duplicate key
  if (err.code === 11000) {
    const message = 'Duplicate field value entered';
    error = { message, statusCode: 400 };
  }

  // Mongoose validation error
  if (err.name === 'ValidationError') {
    const message = Object.values(err.errors).map(val => val.message);
    error = { message, statusCode: 400 };
  }

  res.status(error.statusCode || 500).json({
    success: false,
    error: {
      message: error.message || 'Server Error',
      ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
    }
  });
};

const notFound = (req, res, next) => {
  const error = new Error(`Not found - ${req.originalUrl}`);
  res.status(404);
  next(error);
};

module.exports = { errorHandler, notFound };
```

## 📚 Documentación

### README.md Template

```markdown
# API Project

Production-ready REST API built with [Framework] following industry best practices.

## 🚀 Features

- ✅ RESTful API design
- ✅ JWT Authentication
- ✅ Role-based Authorization
- ✅ Input Validation
- ✅ Error Handling
- ✅ Security Headers
- ✅ Rate Limiting
- ✅ Logging & Monitoring
- ✅ Unit & Integration Tests
- ✅ API Documentation
- ✅ Docker Support
- ✅ CI/CD Pipeline

## 🛠️ Tech Stack

- **Runtime**: [Runtime]
- **Framework**: [Framework]
- **Database**: [Database]
- **Cache**: [Cache]
- **Authentication**: [Auth Method]

## 📋 Prerequisites

- [Runtime] >= [Version]
- [Database] >= [Version]
- Docker & Docker Compose

## 🚀 Quick Start

1. **Clone repository**
   ```bash
   git clone [repository-url]
   cd api-project
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Setup environment**
   ```bash
   cp .env.example .env
   # Edit .env with your values
   ```

4. **Start with Docker**
   ```bash
   docker-compose up
   ```

## 📖 API Documentation

API documentation is available at:
- **Swagger UI**: http://localhost:3000/api-docs
- **OpenAPI Spec**: http://localhost:3000/api-docs.json

## 🧪 Testing

```bash
# Run all tests
npm test

# Run with coverage
npm run test:coverage

# Run specific test file
npm test -- users.test.js
```

## 🚀 Deployment

### Docker

```bash
docker build -t api-project .
docker run -p 3000:3000 api-project
```

### Kubernetes

```bash
kubectl apply -f infrastructure/k8s/
```

## 📊 Monitoring

- **Health Check**: `GET /health`
- **Metrics**: `GET /metrics`
- **Logs**: Available in `logs/` directory

## 🤝 Contributing

1. Fork the repository
2. Create feature branch
3. Run tests
4. Submit pull request

## 📝 License

[License Type]
```

## ✅ Checklist Post-Creación

Después de generar la estructura, verifica:

- [ ] Todos los archivos de configuración están presentes
- [ ] Dependencies están instaladas correctamente
- [ ] Variables de entorno están configuradas
- [ ] Base de datos está conectada
- [ ] Tests pasan correctamente
- [ ] Docker build funciona
- [ ] CI/CD pipeline está configurado
- [ ] Documentación está actualizada
- [ ] Security scanning está habilitado
- [ ] Monitoring está configurado

---

**Tiempo estimado**: 30-45 minutos para scaffold completo 🚀