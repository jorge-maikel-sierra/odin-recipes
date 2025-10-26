# 🔌 Configuración de Servidores MCP (Model Context Protocol)

Los servidores MCP permiten a GitHub Copilot acceder a servicios externos y bases de datos en tiempo real, proporcionando contexto dinámico durante el desarrollo.

## 📋 Índice
1. [¿Qué son los Servidores MCP?](#qué-son-los-servidores-mcp)
2. [Configuración Básica](#configuración-básica)
3. [Servidores Comunes para APIs](#servidores-comunes-para-apis)
4. [Configuración por Entorno](#configuración-por-entorno)
5. [Casos de Uso Específicos](#casos-de-uso-específicos)

---

## 🤖 ¿Qué son los Servidores MCP?

Los servidores MCP actúan como bridges entre GitHub Copilot y servicios externos, permitiendo:

- **Acceso a bases de datos** en tiempo real
- **Integración con APIs** externas
- **Consulta de documentación** dinámica
- **Acceso a servicios de monitoreo**
- **Conexión con herramientas de deployment**

---

## ⚙️ Configuración Básica

### 1. Archivo de Configuración MCP

Crear `.mcp-settings.json` en la raíz del proyecto:

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_CONNECTION_STRING": "postgresql://user:pass@localhost:5432/dbname"
      }
    },
    "redis": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-redis"],
      "env": {
        "REDIS_URL": "redis://localhost:6379"
      }
    },
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "--base-path", "./"]
    },
    "web": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-web"]
    }
  }
}
```

### 2. Configuración en VS Code

En `settings.json`:

```json
{
  "github.copilot.enable": {
    "*": true,
    "yaml": false,
    "plaintext": false
  },
  "github.copilot.advanced": {
    "debug.overrideEngine": "copilot-chat",
    "mcp.enabled": true,
    "mcp.configPath": ".mcp-settings.json"
  }
}
```

---

## 🗄️ Servidores Comunes para APIs

### 1. Servidor de Base de Datos PostgreSQL

```json
{
  "database": {
    "command": "npx",
    "args": ["@modelcontextprotocol/server-postgres"],
    "env": {
      "POSTGRES_CONNECTION_STRING": "postgresql://api_user:password@localhost:5432/api_db",
      "POSTGRES_SCHEMA": "public"
    }
  }
}
```

**Comandos disponibles:**
```bash
# Consultar tablas
@database "Show me all users where role = 'admin'"

# Generar queries
@database "Create a query to find orders from last 30 days with total > 100"

# Analizar schema
@database "What are the relationships between users and orders tables?"
```

### 2. Servidor Redis

```json
{
  "redis": {
    "command": "npx",
    "args": ["@modelcontextprotocol/server-redis"],
    "env": {
      "REDIS_URL": "redis://localhost:6379",
      "REDIS_DB": "0"
    }
  }
}
```

**Comandos disponibles:**
```bash
# Consultar cache
@redis "Show me cached user sessions"

# Analizar keys
@redis "What cache keys exist for user:123?"

# Performance insights
@redis "What are the most accessed cache keys?"
```

### 3. Servidor de APIs Externas

```json
{
  "stripe": {
    "command": "npx",
    "args": ["@modelcontextprotocol/server-web"],
    "env": {
      "STRIPE_SECRET_KEY": "sk_test_...",
      "API_BASE_URL": "https://api.stripe.com/v1"
    }
  }
}
```

### 4. Servidor de Monitoreo (Sentry/DataDog)

```json
{
  "monitoring": {
    "command": "npx",
    "args": ["@modelcontextprotocol/server-sentry"],
    "env": {
      "SENTRY_AUTH_TOKEN": "your_token",
      "SENTRY_ORG": "your_org",
      "SENTRY_PROJECT": "your_project"
    }
  }
}
```

### 5. Servidor de Documentación

```json
{
  "docs": {
    "command": "npx",
    "args": ["@modelcontextprotocol/server-filesystem", "--base-path", "./docs"],
    "env": {
      "SEARCH_PATTERN": "*.md,*.yaml,*.json"
    }
  }
}
```

---

## 🌍 Configuración por Entorno

### Development Environment

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_CONNECTION_STRING": "postgresql://dev_user:dev_pass@localhost:5432/api_dev"
      }
    },
    "redis": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-redis"],
      "env": {
        "REDIS_URL": "redis://localhost:6379/0"
      }
    },
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "--base-path", "./src"]
    }
  }
}
```

### Production Environment

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_CONNECTION_STRING": "${DATABASE_URL}",
        "SSL_MODE": "require"
      }
    },
    "monitoring": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-sentry"],
      "env": {
        "SENTRY_AUTH_TOKEN": "${SENTRY_TOKEN}",
        "SENTRY_ORG": "${SENTRY_ORG}"
      }
    }
  }
}
```

---

## 🎯 Casos de Uso Específicos

### 1. Desarrollo de APIs REST

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_CONNECTION_STRING": "postgresql://localhost:5432/api_db"
      }
    },
    "swagger": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-web"],
      "env": {
        "API_DOCS_URL": "http://localhost:3000/api-docs"
      }
    },
    "testing": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "--base-path", "./tests"]
    }
  }
}
```

**Flujo de trabajo:**
```bash
# 1. Analizar schema de base de datos
@database "Show me the user table structure"

# 2. Generar endpoint basado en tabla
@api-architect "Create REST endpoint for users table with validation"

# 3. Crear tests basados en documentación
@testing "Generate integration tests for user endpoints based on swagger docs"

# 4. Verificar documentación
@swagger "Update API documentation for new user endpoints"
```

### 2. Microservicios

```json
{
  "mcpServers": {
    "service-registry": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-web"],
      "env": {
        "CONSUL_URL": "http://localhost:8500",
        "SERVICE_NAME": "user-service"
      }
    },
    "message-queue": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-rabbitmq"],
      "env": {
        "RABBITMQ_URL": "amqp://localhost:5672"
      }
    }
  }
}
```

### 3. E-commerce APIs

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_CONNECTION_STRING": "postgresql://localhost:5432/ecommerce_db"
      }
    },
    "stripe": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-stripe"],
      "env": {
        "STRIPE_SECRET_KEY": "sk_test_..."
      }
    },
    "inventory": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-redis"],
      "env": {
        "REDIS_URL": "redis://localhost:6379/1"
      }
    }
  }
}
```

---

## 🛠️ Instalación de Servidores MCP

### Instalación Global

```bash
# Servidores básicos
npm install -g @modelcontextprotocol/server-postgres
npm install -g @modelcontextprotocol/server-redis
npm install -g @modelcontextprotocol/server-filesystem
npm install -g @modelcontextprotocol/server-web

# Servidores especializados
npm install -g @modelcontextprotocol/server-sentry
npm install -g @modelcontextprotocol/server-stripe
npm install -g @modelcontextprotocol/server-slack
```

### Instalación Local en Proyecto

```bash
# Para desarrollo
npm install --save-dev @modelcontextprotocol/server-postgres
npm install --save-dev @modelcontextprotocol/server-redis

# Añadir a package.json scripts
{
  "scripts": {
    "mcp:start": "mcp-server start",
    "mcp:postgres": "mcp-server-postgres",
    "mcp:redis": "mcp-server-redis"
  }
}
```

---

## 🔧 Configuración Avanzada

### 1. Servidor Personalizado

Crear `mcp-server.js`:

```javascript
const { MCPServer } = require('@modelcontextprotocol/server');
const express = require('express');

class CustomAPIServer extends MCPServer {
  constructor() {
    super();
    this.name = 'custom-api-server';
  }

  async initialize() {
    // Registrar herramientas personalizadas
    this.registerTool('query_users', {
      description: 'Query users from API',
      schema: {
        type: 'object',
        properties: {
          filter: { type: 'string' },
          limit: { type: 'number' }
        }
      }
    }, this.queryUsers.bind(this));
  }

  async queryUsers(params) {
    // Lógica personalizada para consultar usuarios
    const response = await fetch(`/api/users?filter=${params.filter}&limit=${params.limit}`);
    return await response.json();
  }
}

// Iniciar servidor
const server = new CustomAPIServer();
server.start(3001);
```

### 2. Variables de Entorno

`.env.mcp`:
```env
# Database
POSTGRES_CONNECTION_STRING=postgresql://user:pass@localhost:5432/api_db
POSTGRES_MAX_CONNECTIONS=10

# Redis
REDIS_URL=redis://localhost:6379
REDIS_TTL=3600

# External APIs
STRIPE_SECRET_KEY=sk_test_...
SENDGRID_API_KEY=SG...

# Monitoring
SENTRY_DSN=https://...
DATADOG_API_KEY=...
```

---

## 📈 Comandos de Ejemplo

### Desarrollo de API

```bash
# Análisis de base de datos
@database "Show me all tables and their relationships"
@database "Find performance bottlenecks in query logs"

# Generación de código
@api-architect "Create CRUD endpoints for products table"
@backend-dev "Implement authentication middleware with JWT"

# Testing
@testing "Generate unit tests for UserController"
@security "Audit authentication implementation"

# Deployment
@deployment "Generate Kubernetes manifests for this API"
@monitoring "Setup health checks and metrics"
```

### Debugging en Producción

```bash
# Monitoreo
@monitoring "Show me errors from last 24 hours"
@monitoring "What's the API response time trend?"

# Base de datos
@database "Find slow queries from production logs"
@redis "Check cache hit rate for user sessions"

# Infraestructura
@deployment "Check service status in Kubernetes"
@deployment "Show resource usage metrics"
```

---

## ✅ Checklist de Configuración

- [ ] Archivo `.mcp-settings.json` creado
- [ ] Servidores MCP instalados
- [ ] Variables de entorno configuradas
- [ ] Conexiones a base de datos verificadas
- [ ] Acceso a servicios externos configurado
- [ ] Permisos y autenticación configurados
- [ ] Tests de conectividad realizados
- [ ] Documentación de comandos creada

---

## 🚀 Beneficios del Setup Completo

Con esta configuración, GitHub Copilot puede:

✅ **Acceder a datos en tiempo real** de tu base de datos  
✅ **Consultar APIs externas** para contexto adicional  
✅ **Analizar logs de producción** para debugging  
✅ **Generar código** basado en schema de DB real  
✅ **Sugerir optimizaciones** basadas en métricas reales  
✅ **Crear tests** basados en datos reales  
✅ **Automatizar deployments** con contexto de infraestructura  

**Resultado**: IA con contexto completo de tu stack tecnológico 🧠✨