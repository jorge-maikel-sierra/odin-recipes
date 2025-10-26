# 🚀 Flujo de Trabajo Completo: API con Estándares Industriales usando GitHub Copilot

## 📋 Índice
1. [Preparación del Entorno de IA](#preparación-del-entorno-de-ia)
2. [Estructura del Proyecto](#estructura-del-proyecto)
3. [Configuración de Herramientas](#configuración-de-herramientas)
4. [Flujo de Desarrollo](#flujo-de-desarrollo)
5. [Estándares Industriales](#estándares-industriales)
6. [Automatización y CI/CD](#automatización-y-cicd)

---

## 🧠 Preparación del Entorno de IA

### 1. Instrucciones Personalizadas (Custom Instructions)
```markdown
.github/copilot-instructions.md
```
- Define la arquitectura del proyecto
- Establece patrones de código
- Especifica tecnologías y frameworks
- Configura convenciones de naming

### 2. Modos de Chat Especializados
```
.github/chatmodes/
├── api-architect.chatmode.md     # Diseño de arquitectura
├── backend-dev.chatmode.md       # Desarrollo backend
├── testing.chatmode.md           # Testing y QA
├── security.chatmode.md          # Revisión de seguridad
├── performance.chatmode.md       # Optimización
└── deployment.chatmode.md        # Deploy y DevOps
```

### 3. Prompts Reutilizables
```
.github/prompts/
├── scaffold-api.prompt.md        # Andamiaje inicial
├── create-endpoint.prompt.md     # Nuevos endpoints
├── implement-auth.prompt.md      # Autenticación
├── add-middleware.prompt.md      # Middleware
├── setup-testing.prompt.md       # Testing
├── security-review.prompt.md     # Auditoría seguridad
└── optimize-performance.prompt.md # Optimización
```

### 4. Servidores MCP (Model Context Protocol)
```json
{
  "mcpServers": {
    "database": "postgresql://...",
    "redis": "redis://...",
    "monitoring": "datadog://...",
    "docs": "swagger://..."
  }
}
```

---

## 🏗️ Estructura del Proyecto

### Andamiaje Inicial
```
project-api/
├── .github/                      # Configuración IA y CI/CD
│   ├── copilot-instructions.md
│   ├── chatmodes/
│   ├── prompts/
│   └── workflows/
├── src/
│   ├── controllers/              # Controladores REST
│   ├── services/                 # Lógica de negocio
│   ├── models/                   # Modelos de datos
│   ├── middleware/               # Middleware personalizado
│   ├── utils/                    # Utilidades
│   ├── config/                   # Configuración
│   └── types/                    # Tipos TypeScript
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── docs/                         # Documentación API
├── docker/                       # Contenedores
├── scripts/                      # Scripts automatización
└── infrastructure/               # IaC (Terraform, etc.)
```

---

## ⚙️ Configuración de Herramientas

### 1. Chat Modes Especializados

#### API Architect Mode
```markdown
---
description: Arquitecto de APIs REST con estándares industriales
tools: ['search', 'files', 'terminal']
model: Claude Sonnet 4
---

# Modo API Architect

Eres un **arquitecto de software senior** especializado en APIs REST con estándares industriales.

## 🎯 Expertise
- **Diseño de APIs**: REST, GraphQL, OpenAPI 3.0
- **Arquitectura**: Microservicios, Event-Driven, CQRS
- **Patrones**: Repository, Service Layer, Dependency Injection
- **Seguridad**: OAuth 2.0, JWT, Rate Limiting
- **Performance**: Caching, CDN, Load Balancing
```

#### Backend Developer Mode
```markdown
---
description: Desarrollador backend con Node.js/Python/Go
tools: ['terminal', 'files', 'search', 'debug']
---

# Modo Backend Developer

Especialista en desarrollo backend con focus en:
- **Node.js**: Express, Fastify, NestJS
- **Python**: FastAPI, Django, Flask
- **Go**: Gin, Echo, Fiber
- **Bases de datos**: PostgreSQL, MongoDB, Redis
- **Testing**: Jest, Pytest, Go test
```

### 2. Prompts Especializados

#### Scaffold API Prompt
```markdown
---
mode: 'agent'
description: 'Crear estructura completa de API con estándares industriales'
tools: ['terminal', 'files']
---

# Scaffold API Project

Crea una API completa siguiendo estos estándares:

## Tecnologías:
- **Runtime**: ${input:runtime:Node.js/Python/Go}
- **Framework**: ${input:framework}
- **Base de datos**: ${input:database:PostgreSQL}
- **Cache**: Redis
- **Documentation**: OpenAPI 3.0

## Estructura requerida:
1. **Configuración del proyecto** (package.json, requirements.txt, go.mod)
2. **Estructura de carpetas** estándar
3. **Configuración de entorno** (.env.example)
4. **Docker** (Dockerfile, docker-compose.yml)
5. **Testing** setup básico
6. **CI/CD** pipeline
7. **Documentación** README.md
```

---

## 🔄 Flujo de Desarrollo

### Fase 1: Planificación y Diseño
```bash
# 1. Usar modo API Architect
@api-architect "Diseña una API para gestión de [dominio] con autenticación JWT"

# 2. Usar prompt de scaffold
@scaffold-api runtime:Node.js framework:Express database:PostgreSQL
```

### Fase 2: Implementación Base
```bash
# 3. Crear estructura inicial
@create-project-structure

# 4. Configurar base de datos
@setup-database

# 5. Implementar autenticación
@implement-auth
```

### Fase 3: Desarrollo de Features
```bash
# 6. Para cada endpoint
@create-endpoint entity:User method:POST

# 7. Añadir middleware
@add-middleware type:validation

# 8. Implementar tests
@setup-testing
```

### Fase 4: Testing y QA
```bash
# 9. Revisar código
@testing "Crear tests unitarios para UserController"

# 10. Auditoría de seguridad
@security-review
```

### Fase 5: Optimización y Deploy
```bash
# 11. Optimizar performance
@performance "Optimizar consultas de base de datos"

# 12. Configurar deployment
@deployment "Setup CI/CD con GitHub Actions"
```

---

## 📚 Estándares Industriales

### 1. Arquitectura REST
- **Recursos**: Nombres en plural (`/users`, `/orders`)
- **Métodos HTTP**: GET, POST, PUT, PATCH, DELETE
- **Códigos de estado**: 200, 201, 400, 401, 403, 404, 500
- **Versionado**: `/api/v1/`

### 2. Seguridad
```javascript
// Ejemplo con Express.js
app.use(helmet()); // Security headers
app.use(cors()); // CORS policy
app.use(rateLimit()); // Rate limiting
app.use(authentication); // JWT validation
app.use(authorization); // Role-based access
```

### 3. Validación de Datos
```javascript
// Joi/Zod validation schema
const userSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
  role: z.enum(['user', 'admin'])
});
```

### 4. Error Handling
```javascript
// Standardized error response
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email format",
    "details": [
      {
        "field": "email",
        "message": "Must be a valid email"
      }
    ]
  }
}
```

### 5. Logging y Monitoring
```javascript
// Structured logging
logger.info('User created', {
  userId: user.id,
  email: user.email,
  timestamp: new Date().toISOString(),
  requestId: req.id
});
```

---

## 🔧 Automatización y CI/CD

### GitHub Actions Workflow
```yaml
name: API CI/CD Pipeline
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      - name: Install dependencies
        run: npm ci
      - name: Run tests
        run: npm test
      - name: Security audit
        run: npm audit
      - name: Build
        run: npm run build
```

### Docker Configuration
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

---

## 🎯 Comandos Clave del Flujo

### Inicio de Proyecto
```bash
# 1. Planificación
@api-architect "Diseña API para ecommerce con microservicios"

# 2. Scaffold
@scaffold-api runtime:Node.js framework:NestJS database:PostgreSQL

# 3. Configuración inicial
@setup-environment
```

### Desarrollo Iterativo
```bash
# Cada feature
@create-endpoint entity:Product method:GET
@add-validation schema:ProductSchema
@create-tests endpoint:products
@security-review endpoint:products
```

### Pre-Deploy
```bash
@performance "Optimizar API para 1000 RPS"
@deployment "Configurar Kubernetes deployment"
@monitoring "Setup Datadog APM"
```

---

## 📈 Métricas y KPIs

### Performance
- **Response Time**: < 100ms p95
- **Throughput**: > 1000 RPS
- **Error Rate**: < 0.1%
- **Uptime**: > 99.9%

### Calidad de Código
- **Test Coverage**: > 90%
- **Security Vulnerabilities**: 0
- **Code Quality**: SonarQube A rating
- **Documentation**: 100% API endpoints

### DevOps
- **Deploy Frequency**: Daily
- **Lead Time**: < 1 day
- **MTTR**: < 1 hour
- **Change Failure Rate**: < 5%

---

## 🚀 Ejemplo Práctico Completo

### 1. Inicio del Proyecto
```bash
# Activar modo arquitecto
@api-architect

# Prompt: "Diseña una API REST para una plataforma de cursos online con:
# - Autenticación JWT
# - Roles: estudiante, instructor, admin
# - Entidades: usuarios, cursos, módulos, evaluaciones
# - Payments con Stripe
# - Notificaciones email
# - Deploy en AWS"
```

### 2. Implementación
```bash
# Generar estructura
@scaffold-api runtime:Node.js framework:NestJS database:PostgreSQL

# Implementar features principales
@create-endpoint entity:Course method:POST
@implement-auth provider:JWT
@add-middleware type:validation
@setup-payments provider:Stripe
```

### 3. Testing y Deploy
```bash
@setup-testing framework:Jest
@security-review
@performance optimization
@deployment platform:AWS
```

---

## 🎉 Resultado Final

Al seguir este flujo obtienes:

✅ **API completa** con estándares industriales  
✅ **Documentación automática** OpenAPI  
✅ **Testing** completo (unit, integration, e2e)  
✅ **CI/CD pipeline** automatizado  
✅ **Seguridad** implementada  
✅ **Monitoring** y observabilidad  
✅ **Deploy** en producción  

**Tiempo estimado**: 2-4 semanas → 3-5 días con IA optimizada 🚀