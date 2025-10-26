---
mode: 'agent'
description: 'Crear nuevo endpoint REST siguiendo patrones establecidos'
---

# Crear Nuevo Endpoint REST

Tu objetivo es crear un endpoint REST completo siguiendo los patrones y estándares establecidos en el proyecto.

## 📋 Información Requerida

Si no se proporciona, solicita:

- **Entidad**: ${input:entity:User | Product | Order | etc.}
- **Método HTTP**: ${input:method:GET | POST | PUT | PATCH | DELETE}
- **Funcionalidad**: ${input:functionality:Descripción específica}
- **Autenticación**: ${input:auth:required | optional | public}
- **Permisos**: ${input:permissions:admin | user | owner | public}

## 🏗️ Estructura del Endpoint

### 1. Ruta y Método
```
${input:method} /api/v1/${input:entity:users}/${input:id::id?}
```

### 2. Archivos a Crear/Modificar

#### Controller (src/controllers/${entity}Controller.js)
```javascript
const ${entity}Service = require('../services/${entity}Service');
const { validateInput } = require('../middleware/validation');
const { ${entity}Schema } = require('../utils/schemas');
const logger = require('../utils/logger');

class ${entity}Controller {
  // GET /api/v1/${entity}s
  static async getAll(req, res, next) {
    try {
      const { page = 1, limit = 10, ...filters } = req.query;
      
      const result = await ${entity}Service.getAll({
        page: parseInt(page),
        limit: parseInt(limit),
        filters
      });

      res.status(200).json({
        success: true,
        data: result.data,
        pagination: {
          page: result.page,
          limit: result.limit,
          total: result.total,
          pages: Math.ceil(result.total / result.limit)
        }
      });

      logger.info(`${entity}s retrieved`, { 
        userId: req.user?.id,
        count: result.data.length,
        page,
        limit 
      });

    } catch (error) {
      next(error);
    }
  }

  // GET /api/v1/${entity}s/:id
  static async getById(req, res, next) {
    try {
      const { id } = req.params;
      
      const ${entity.toLowerCase()} = await ${entity}Service.getById(id);
      
      if (!${entity.toLowerCase()}) {
        return res.status(404).json({
          success: false,
          error: {
            message: '${entity} not found',
            code: '${entity.toUpperCase()}_NOT_FOUND'
          }
        });
      }

      res.status(200).json({
        success: true,
        data: ${entity.toLowerCase()}
      });

      logger.info('${entity} retrieved', { 
        userId: req.user?.id,
        ${entity.toLowerCase()}Id: id 
      });

    } catch (error) {
      next(error);
    }
  }

  // POST /api/v1/${entity}s
  static async create(req, res, next) {
    try {
      const validatedData = await validateInput(req.body, ${entity}Schema.create);
      
      const ${entity.toLowerCase()} = await ${entity}Service.create({
        ...validatedData,
        createdBy: req.user.id
      });

      res.status(201).json({
        success: true,
        data: ${entity.toLowerCase()},
        message: '${entity} created successfully'
      });

      logger.info('${entity} created', { 
        userId: req.user.id,
        ${entity.toLowerCase()}Id: ${entity.toLowerCase()}.id 
      });

    } catch (error) {
      next(error);
    }
  }

  // PUT /api/v1/${entity}s/:id
  static async update(req, res, next) {
    try {
      const { id } = req.params;
      const validatedData = await validateInput(req.body, ${entity}Schema.update);
      
      const ${entity.toLowerCase()} = await ${entity}Service.update(id, {
        ...validatedData,
        updatedBy: req.user.id,
        updatedAt: new Date()
      });

      if (!${entity.toLowerCase()}) {
        return res.status(404).json({
          success: false,
          error: {
            message: '${entity} not found',
            code: '${entity.toUpperCase()}_NOT_FOUND'
          }
        });
      }

      res.status(200).json({
        success: true,
        data: ${entity.toLowerCase()},
        message: '${entity} updated successfully'
      });

      logger.info('${entity} updated', { 
        userId: req.user.id,
        ${entity.toLowerCase()}Id: id 
      });

    } catch (error) {
      next(error);
    }
  }

  // DELETE /api/v1/${entity}s/:id
  static async delete(req, res, next) {
    try {
      const { id } = req.params;
      
      const deleted = await ${entity}Service.delete(id);

      if (!deleted) {
        return res.status(404).json({
          success: false,
          error: {
            message: '${entity} not found',
            code: '${entity.toUpperCase()}_NOT_FOUND'
          }
        });
      }

      res.status(200).json({
        success: true,
        message: '${entity} deleted successfully'
      });

      logger.info('${entity} deleted', { 
        userId: req.user.id,
        ${entity.toLowerCase()}Id: id 
      });

    } catch (error) {
      next(error);
    }
  }
}

module.exports = ${entity}Controller;
```

#### Service Layer (src/services/${entity}Service.js)
```javascript
const ${entity}Model = require('../models/${entity}Model');
const { AppError } = require('../utils/AppError');

class ${entity}Service {
  static async getAll({ page = 1, limit = 10, filters = {} }) {
    try {
      const offset = (page - 1) * limit;
      
      const [data, total] = await Promise.all([
        ${entity}Model.findMany({
          where: filters,
          limit,
          offset,
          orderBy: { createdAt: 'desc' }
        }),
        ${entity}Model.count({ where: filters })
      ]);

      return {
        data,
        total,
        page,
        limit
      };
    } catch (error) {
      throw new AppError(`Failed to retrieve ${entity.toLowerCase()}s: ${error.message}`, 500);
    }
  }

  static async getById(id) {
    try {
      const ${entity.toLowerCase()} = await ${entity}Model.findById(id);
      return ${entity.toLowerCase()};
    } catch (error) {
      throw new AppError(`Failed to retrieve ${entity.toLowerCase()}: ${error.message}`, 500);
    }
  }

  static async create(data) {
    try {
      // Business logic validation
      await this.validateBusinessRules(data);
      
      const ${entity.toLowerCase()} = await ${entity}Model.create(data);
      return ${entity.toLowerCase()};
    } catch (error) {
      if (error instanceof AppError) throw error;
      throw new AppError(`Failed to create ${entity.toLowerCase()}: ${error.message}`, 500);
    }
  }

  static async update(id, data) {
    try {
      // Check if exists
      const existing = await ${entity}Model.findById(id);
      if (!existing) {
        return null;
      }

      // Business logic validation
      await this.validateBusinessRules(data, id);
      
      const ${entity.toLowerCase()} = await ${entity}Model.update(id, data);
      return ${entity.toLowerCase()};
    } catch (error) {
      if (error instanceof AppError) throw error;
      throw new AppError(`Failed to update ${entity.toLowerCase()}: ${error.message}`, 500);
    }
  }

  static async delete(id) {
    try {
      // Check if exists
      const existing = await ${entity}Model.findById(id);
      if (!existing) {
        return false;
      }

      // Check business rules for deletion
      await this.validateDeletion(id);
      
      await ${entity}Model.delete(id);
      return true;
    } catch (error) {
      if (error instanceof AppError) throw error;
      throw new AppError(`Failed to delete ${entity.toLowerCase()}: ${error.message}`, 500);
    }
  }

  static async validateBusinessRules(data, id = null) {
    // Implement specific business logic here
    // Example: Check for duplicates, validate relationships, etc.
  }

  static async validateDeletion(id) {
    // Check if entity can be deleted
    // Example: Check for dependencies, business rules, etc.
  }
}

module.exports = ${entity}Service;
```

#### Model (src/models/${entity}Model.js)

**Para PostgreSQL/Knex:**
```javascript
const knex = require('../config/database');

class ${entity}Model {
  static get tableName() {
    return '${entity.toLowerCase()}s';
  }

  static async findMany({ where = {}, limit = 10, offset = 0, orderBy = {} }) {
    let query = knex(this.tableName);
    
    // Apply filters
    Object.keys(where).forEach(key => {
      if (where[key] !== undefined) {
        query = query.where(key, where[key]);
      }
    });
    
    // Apply ordering
    Object.keys(orderBy).forEach(key => {
      query = query.orderBy(key, orderBy[key]);
    });
    
    return await query.limit(limit).offset(offset);
  }

  static async findById(id) {
    return await knex(this.tableName).where('id', id).first();
  }

  static async create(data) {
    const [${entity.toLowerCase()}] = await knex(this.tableName)
      .insert({
        ...data,
        created_at: new Date(),
        updated_at: new Date()
      })
      .returning('*');
    
    return ${entity.toLowerCase()};
  }

  static async update(id, data) {
    const [${entity.toLowerCase()}] = await knex(this.tableName)
      .where('id', id)
      .update({
        ...data,
        updated_at: new Date()
      })
      .returning('*');
    
    return ${entity.toLowerCase()};
  }

  static async delete(id) {
    return await knex(this.tableName).where('id', id).del();
  }

  static async count({ where = {} }) {
    let query = knex(this.tableName);
    
    Object.keys(where).forEach(key => {
      if (where[key] !== undefined) {
        query = query.where(key, where[key]);
      }
    });
    
    const result = await query.count('* as count').first();
    return parseInt(result.count);
  }
}

module.exports = ${entity}Model;
```

#### Validation Schema (src/utils/schemas/${entity}Schema.js)
```javascript
const Joi = require('joi');

const ${entity}Schema = {
  create: Joi.object({
    // Add specific fields for your entity
    name: Joi.string().required().min(1).max(255),
    description: Joi.string().optional().max(1000),
    status: Joi.string().valid('active', 'inactive').default('active'),
    // Add more fields as needed
  }),

  update: Joi.object({
    name: Joi.string().optional().min(1).max(255),
    description: Joi.string().optional().max(1000),
    status: Joi.string().valid('active', 'inactive').optional(),
    // Add more fields as needed
  }),

  query: Joi.object({
    page: Joi.number().integer().min(1).default(1),
    limit: Joi.number().integer().min(1).max(100).default(10),
    status: Joi.string().valid('active', 'inactive').optional(),
    // Add more query filters as needed
  })
};

module.exports = { ${entity}Schema };
```

#### Routes (src/routes/${entity}Routes.js)
```javascript
const express = require('express');
const ${entity}Controller = require('../controllers/${entity}Controller');
const { authenticate } = require('../middleware/auth');
const { authorize } = require('../middleware/authorization');
const { validateQuery } = require('../middleware/validation');
const { ${entity}Schema } = require('../utils/schemas/${entity}Schema');

const router = express.Router();

// Public routes (if any)
// router.get('/public', ${entity}Controller.getPublic);

// Protected routes
router.use(authenticate); // All routes below require authentication

// GET /api/v1/${entity}s - List all ${entity}s
router.get('/', 
  validateQuery(${entity}Schema.query),
  authorize(['admin', 'user']),
  ${entity}Controller.getAll
);

// GET /api/v1/${entity}s/:id - Get specific ${entity}
router.get('/:id', 
  authorize(['admin', 'user']),
  ${entity}Controller.getById
);

// POST /api/v1/${entity}s - Create new ${entity}
router.post('/', 
  authorize(['admin', 'user']),
  ${entity}Controller.create
);

// PUT /api/v1/${entity}s/:id - Update ${entity}
router.put('/:id', 
  authorize(['admin', 'owner']),
  ${entity}Controller.update
);

// DELETE /api/v1/${entity}s/:id - Delete ${entity}
router.delete('/:id', 
  authorize(['admin']),
  ${entity}Controller.delete
);

module.exports = router;
```

### 3. Tests

#### Unit Tests (tests/unit/${entity}Controller.test.js)
```javascript
const request = require('supertest');
const app = require('../../src/app');
const ${entity}Service = require('../../src/services/${entity}Service');

// Mock the service
jest.mock('../../src/services/${entity}Service');

describe('${entity}Controller', () => {
  let authToken;

  beforeAll(async () => {
    // Setup auth token for tests
    authToken = 'mock-jwt-token';
  });

  beforeEach(() => {
    jest.clearAllMocks();
  });

  describe('GET /api/v1/${entity.toLowerCase()}s', () => {
    it('should return paginated ${entity.toLowerCase()}s', async () => {
      const mockData = {
        data: [{ id: 1, name: 'Test ${entity}' }],
        total: 1,
        page: 1,
        limit: 10
      };

      ${entity}Service.getAll.mockResolvedValue(mockData);

      const response = await request(app)
        .get('/api/v1/${entity.toLowerCase()}s')
        .set('Authorization', `Bearer ${authToken}`)
        .expect(200);

      expect(response.body.success).toBe(true);
      expect(response.body.data).toEqual(mockData.data);
      expect(response.body.pagination.total).toBe(1);
    });

    it('should handle service errors', async () => {
      ${entity}Service.getAll.mockRejectedValue(new Error('Service error'));

      await request(app)
        .get('/api/v1/${entity.toLowerCase()}s')
        .set('Authorization', `Bearer ${authToken}`)
        .expect(500);
    });
  });

  describe('POST /api/v1/${entity.toLowerCase()}s', () => {
    it('should create a new ${entity.toLowerCase()}', async () => {
      const newData = { name: 'New ${entity}' };
      const createdData = { id: 1, ...newData };

      ${entity}Service.create.mockResolvedValue(createdData);

      const response = await request(app)
        .post('/api/v1/${entity.toLowerCase()}s')
        .set('Authorization', `Bearer ${authToken}`)
        .send(newData)
        .expect(201);

      expect(response.body.success).toBe(true);
      expect(response.body.data).toEqual(createdData);
    });

    it('should validate input data', async () => {
      await request(app)
        .post('/api/v1/${entity.toLowerCase()}s')
        .set('Authorization', `Bearer ${authToken}`)
        .send({}) // Empty data
        .expect(400);
    });
  });
});
```

### 4. Documentation

#### OpenAPI Specification
```yaml
# Add to your openapi.yml
paths:
  /api/v1/${entity.toLowerCase()}s:
    get:
      summary: List ${entity}s
      tags:
        - ${entity}s
      security:
        - bearerAuth: []
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            minimum: 1
            default: 1
        - name: limit
          in: query
          schema:
            type: integer
            minimum: 1
            maximum: 100
            default: 10
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  success:
                    type: boolean
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/${entity}'
                  pagination:
                    $ref: '#/components/schemas/Pagination'

    post:
      summary: Create ${entity}
      tags:
        - ${entity}s
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/${entity}Create'
      responses:
        '201':
          description: Created successfully
          content:
            application/json:
              schema:
                type: object
                properties:
                  success:
                    type: boolean
                  data:
                    $ref: '#/components/schemas/${entity}'

components:
  schemas:
    ${entity}:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        description:
          type: string
        status:
          type: string
          enum: [active, inactive]
        createdAt:
          type: string
          format: date-time
        updatedAt:
          type: string
          format: date-time
```

## ✅ Checklist Post-Creación

- [ ] Controller implementado con todos los métodos CRUD
- [ ] Service layer con lógica de negocio
- [ ] Model con queries optimizadas
- [ ] Validation schemas definidos
- [ ] Routes configuradas con permisos
- [ ] Tests unitarios escritos
- [ ] Documentación OpenAPI actualizada
- [ ] Error handling implementado
- [ ] Logging agregado
- [ ] Security middleware aplicado

## 🚀 Pasos Adicionales

1. **Registrar rutas** en `src/app.js`:
```javascript
app.use('/api/v1/${entity.toLowerCase()}s', require('./routes/${entity.toLowerCase()}Routes'));
```

2. **Crear migración** de base de datos (si es necesario)

3. **Actualizar** tests de integración

4. **Verificar** que todos los tests pasan

---

**Tiempo estimado**: 15-20 minutos por endpoint completo 🚀