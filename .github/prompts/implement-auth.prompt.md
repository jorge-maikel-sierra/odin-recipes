---
mode: 'agent'
description: 'Implementar sistema de autenticación JWT completo con mejores prácticas'
---

# Implementar Autenticación JWT

Tu objetivo es implementar un sistema de autenticación robusto y seguro usando JWT con todas las mejores prácticas de la industria.

## 📋 Información Requerida

- **Tipo de autenticación**: ${input:authType:JWT | OAuth2 | SAML}
- **Proveedor de hashing**: ${input:hasher:bcrypt | argon2}
- **Duración del token**: ${input:tokenExpiry:15m | 1h | 1d | 7d}
- **Refresh tokens**: ${input:refreshTokens:yes | no}
- **Rate limiting**: ${input:rateLimit:yes | no}

## 🔐 Implementación Completa

### 1. Authentication Service (src/services/authService.js)

```javascript
const jwt = require('jsonwebtoken');
const bcrypt = require('bcryptjs');
const crypto = require('crypto');
const UserModel = require('../models/UserModel');
const { AppError } = require('../utils/AppError');
const logger = require('../utils/logger');
const redisClient = require('../config/redis');

class AuthService {
  static async register(userData) {
    try {
      const { email, password, firstName, lastName } = userData;

      // Check if user already exists
      const existingUser = await UserModel.findByEmail(email);
      if (existingUser) {
        throw new AppError('User already exists with this email', 409);
      }

      // Hash password
      const saltRounds = 12;
      const hashedPassword = await bcrypt.hash(password, saltRounds);

      // Create user
      const user = await UserModel.create({
        email,
        password: hashedPassword,
        firstName,
        lastName,
        role: 'user',
        isEmailVerified: false,
        emailVerificationToken: crypto.randomBytes(32).toString('hex')
      });

      // Remove password from response
      const { password: _, ...userWithoutPassword } = user;

      // Generate tokens
      const tokens = await this.generateTokens(user);

      logger.info('User registered successfully', { 
        userId: user.id, 
        email: user.email 
      });

      return {
        user: userWithoutPassword,
        ...tokens
      };

    } catch (error) {
      if (error instanceof AppError) throw error;
      throw new AppError(`Registration failed: ${error.message}`, 500);
    }
  }

  static async login(email, password, ipAddress, userAgent) {
    try {
      // Find user
      const user = await UserModel.findByEmail(email);
      if (!user) {
        throw new AppError('Invalid credentials', 401);
      }

      // Check if account is locked
      if (user.isLocked && user.lockUntil > new Date()) {
        const remainingTime = Math.ceil((user.lockUntil - new Date()) / 1000 / 60);
        throw new AppError(`Account locked. Try again in ${remainingTime} minutes`, 423);
      }

      // Verify password
      const isPasswordValid = await bcrypt.compare(password, user.password);
      if (!isPasswordValid) {
        await this.handleFailedLogin(user.id);
        throw new AppError('Invalid credentials', 401);
      }

      // Check if email is verified
      if (!user.isEmailVerified) {
        throw new AppError('Please verify your email before logging in', 401);
      }

      // Reset failed login attempts
      await UserModel.resetFailedLoginAttempts(user.id);

      // Update last login
      await UserModel.updateLastLogin(user.id, ipAddress);

      // Remove password from response
      const { password: _, ...userWithoutPassword } = user;

      // Generate tokens
      const tokens = await this.generateTokens(user);

      // Log successful login
      logger.info('User logged in successfully', { 
        userId: user.id, 
        email: user.email,
        ipAddress,
        userAgent 
      });

      return {
        user: userWithoutPassword,
        ...tokens
      };

    } catch (error) {
      if (error instanceof AppError) throw error;
      throw new AppError(`Login failed: ${error.message}`, 500);
    }
  }

  static async refreshToken(refreshToken) {
    try {
      // Verify refresh token
      const decoded = jwt.verify(refreshToken, process.env.JWT_REFRESH_SECRET);
      
      // Check if token exists in Redis
      const storedToken = await redisClient.get(`refresh_token:${decoded.userId}`);
      if (!storedToken || storedToken !== refreshToken) {
        throw new AppError('Invalid refresh token', 401);
      }

      // Get user
      const user = await UserModel.findById(decoded.userId);
      if (!user) {
        throw new AppError('User not found', 404);
      }

      // Generate new tokens
      const tokens = await this.generateTokens(user);

      // Remove old refresh token
      await redisClient.del(`refresh_token:${user.id}`);

      logger.info('Token refreshed successfully', { userId: user.id });

      return tokens;

    } catch (error) {
      if (error.name === 'JsonWebTokenError' || error.name === 'TokenExpiredError') {
        throw new AppError('Invalid refresh token', 401);
      }
      if (error instanceof AppError) throw error;
      throw new AppError(`Token refresh failed: ${error.message}`, 500);
    }
  }

  static async logout(userId, refreshToken) {
    try {
      // Remove refresh token from Redis
      await redisClient.del(`refresh_token:${userId}`);
      
      // Add access token to blacklist (if needed)
      // await redisClient.sadd('blacklisted_tokens', accessToken);

      logger.info('User logged out successfully', { userId });

      return { message: 'Logged out successfully' };

    } catch (error) {
      throw new AppError(`Logout failed: ${error.message}`, 500);
    }
  }

  static async generateTokens(user) {
    const payload = {
      userId: user.id,
      email: user.email,
      role: user.role
    };

    // Generate access token
    const accessToken = jwt.sign(
      payload,
      process.env.JWT_SECRET,
      { 
        expiresIn: process.env.JWT_EXPIRES_IN || '15m',
        issuer: process.env.JWT_ISSUER || 'your-api',
        audience: process.env.JWT_AUDIENCE || 'your-app'
      }
    );

    // Generate refresh token
    const refreshToken = jwt.sign(
      { userId: user.id },
      process.env.JWT_REFRESH_SECRET,
      { 
        expiresIn: process.env.JWT_REFRESH_EXPIRES_IN || '7d',
        issuer: process.env.JWT_ISSUER || 'your-api',
        audience: process.env.JWT_AUDIENCE || 'your-app'
      }
    );

    // Store refresh token in Redis
    const refreshTokenExpiry = 7 * 24 * 60 * 60; // 7 days in seconds
    await redisClient.setex(`refresh_token:${user.id}`, refreshTokenExpiry, refreshToken);

    return {
      accessToken,
      refreshToken,
      expiresIn: process.env.JWT_EXPIRES_IN || '15m'
    };
  }

  static async handleFailedLogin(userId) {
    const user = await UserModel.findById(userId);
    const failedAttempts = (user.failedLoginAttempts || 0) + 1;
    const maxAttempts = 5;
    const lockTime = 30 * 60 * 1000; // 30 minutes

    if (failedAttempts >= maxAttempts) {
      await UserModel.lockAccount(userId, new Date(Date.now() + lockTime));
      logger.warn('Account locked due to failed login attempts', { userId, failedAttempts });
    } else {
      await UserModel.incrementFailedLoginAttempts(userId, failedAttempts);
    }
  }

  static async forgotPassword(email) {
    try {
      const user = await UserModel.findByEmail(email);
      if (!user) {
        // Don't reveal if email exists or not
        return { message: 'If email exists, password reset instructions will be sent' };
      }

      // Generate reset token
      const resetToken = crypto.randomBytes(32).toString('hex');
      const resetTokenExpiry = new Date(Date.now() + 60 * 60 * 1000); // 1 hour

      // Save reset token
      await UserModel.savePasswordResetToken(user.id, resetToken, resetTokenExpiry);

      // TODO: Send email with reset token
      // await emailService.sendPasswordResetEmail(user.email, resetToken);

      logger.info('Password reset requested', { userId: user.id, email });

      return { message: 'If email exists, password reset instructions will be sent' };

    } catch (error) {
      throw new AppError(`Password reset failed: ${error.message}`, 500);
    }
  }

  static async resetPassword(resetToken, newPassword) {
    try {
      const user = await UserModel.findByResetToken(resetToken);
      if (!user || user.passwordResetExpiry < new Date()) {
        throw new AppError('Invalid or expired reset token', 400);
      }

      // Hash new password
      const saltRounds = 12;
      const hashedPassword = await bcrypt.hash(newPassword, saltRounds);

      // Update password and clear reset token
      await UserModel.updatePassword(user.id, hashedPassword);
      await UserModel.clearPasswordResetToken(user.id);

      logger.info('Password reset successfully', { userId: user.id });

      return { message: 'Password reset successfully' };

    } catch (error) {
      if (error instanceof AppError) throw error;
      throw new AppError(`Password reset failed: ${error.message}`, 500);
    }
  }

  static async verifyEmail(token) {
    try {
      const user = await UserModel.findByEmailVerificationToken(token);
      if (!user) {
        throw new AppError('Invalid verification token', 400);
      }

      await UserModel.verifyEmail(user.id);

      logger.info('Email verified successfully', { userId: user.id });

      return { message: 'Email verified successfully' };

    } catch (error) {
      if (error instanceof AppError) throw error;
      throw new AppError(`Email verification failed: ${error.message}`, 500);
    }
  }
}

module.exports = AuthService;
```

### 2. Authentication Controller (src/controllers/authController.js)

```javascript
const AuthService = require('../services/authService');
const { validateInput } = require('../middleware/validation');
const { authSchemas } = require('../utils/schemas/authSchemas');
const logger = require('../utils/logger');

class AuthController {
  static async register(req, res, next) {
    try {
      const validatedData = await validateInput(req.body, authSchemas.register);
      
      const result = await AuthService.register(validatedData);

      // Set httpOnly cookie for refresh token
      res.cookie('refreshToken', result.refreshToken, {
        httpOnly: true,
        secure: process.env.NODE_ENV === 'production',
        sameSite: 'strict',
        maxAge: 7 * 24 * 60 * 60 * 1000 // 7 days
      });

      res.status(201).json({
        success: true,
        data: {
          user: result.user,
          accessToken: result.accessToken,
          expiresIn: result.expiresIn
        },
        message: 'User registered successfully'
      });

    } catch (error) {
      next(error);
    }
  }

  static async login(req, res, next) {
    try {
      const validatedData = await validateInput(req.body, authSchemas.login);
      const { email, password } = validatedData;
      
      const ipAddress = req.ip || req.connection.remoteAddress;
      const userAgent = req.get('User-Agent');

      const result = await AuthService.login(email, password, ipAddress, userAgent);

      // Set httpOnly cookie for refresh token
      res.cookie('refreshToken', result.refreshToken, {
        httpOnly: true,
        secure: process.env.NODE_ENV === 'production',
        sameSite: 'strict',
        maxAge: 7 * 24 * 60 * 60 * 1000 // 7 days
      });

      res.status(200).json({
        success: true,
        data: {
          user: result.user,
          accessToken: result.accessToken,
          expiresIn: result.expiresIn
        },
        message: 'Login successful'
      });

    } catch (error) {
      next(error);
    }
  }

  static async refreshToken(req, res, next) {
    try {
      const refreshToken = req.cookies.refreshToken || req.body.refreshToken;
      
      if (!refreshToken) {
        return res.status(401).json({
          success: false,
          error: {
            message: 'Refresh token required',
            code: 'REFRESH_TOKEN_REQUIRED'
          }
        });
      }

      const result = await AuthService.refreshToken(refreshToken);

      // Set new httpOnly cookie for refresh token
      res.cookie('refreshToken', result.refreshToken, {
        httpOnly: true,
        secure: process.env.NODE_ENV === 'production',
        sameSite: 'strict',
        maxAge: 7 * 24 * 60 * 60 * 1000 // 7 days
      });

      res.status(200).json({
        success: true,
        data: {
          accessToken: result.accessToken,
          expiresIn: result.expiresIn
        },
        message: 'Token refreshed successfully'
      });

    } catch (error) {
      next(error);
    }
  }

  static async logout(req, res, next) {
    try {
      const refreshToken = req.cookies.refreshToken;
      const userId = req.user.id;

      await AuthService.logout(userId, refreshToken);

      // Clear refresh token cookie
      res.clearCookie('refreshToken');

      res.status(200).json({
        success: true,
        message: 'Logged out successfully'
      });

    } catch (error) {
      next(error);
    }
  }

  static async forgotPassword(req, res, next) {
    try {
      const validatedData = await validateInput(req.body, authSchemas.forgotPassword);
      const { email } = validatedData;

      const result = await AuthService.forgotPassword(email);

      res.status(200).json({
        success: true,
        message: result.message
      });

    } catch (error) {
      next(error);
    }
  }

  static async resetPassword(req, res, next) {
    try {
      const { token } = req.params;
      const validatedData = await validateInput(req.body, authSchemas.resetPassword);
      const { password } = validatedData;

      const result = await AuthService.resetPassword(token, password);

      res.status(200).json({
        success: true,
        message: result.message
      });

    } catch (error) {
      next(error);
    }
  }

  static async verifyEmail(req, res, next) {
    try {
      const { token } = req.params;

      const result = await AuthService.verifyEmail(token);

      res.status(200).json({
        success: true,
        message: result.message
      });

    } catch (error) {
      next(error);
    }
  }

  static async getProfile(req, res, next) {
    try {
      const user = req.user;

      res.status(200).json({
        success: true,
        data: user
      });

    } catch (error) {
      next(error);
    }
  }
}

module.exports = AuthController;
```

### 3. Authentication Middleware (src/middleware/auth.js)

```javascript
const jwt = require('jsonwebtoken');
const UserModel = require('../models/UserModel');
const { AppError } = require('../utils/AppError');
const redisClient = require('../config/redis');

const authenticate = async (req, res, next) => {
  try {
    // Get token from header
    const authHeader = req.headers.authorization;
    if (!authHeader || !authHeader.startsWith('Bearer ')) {
      throw new AppError('Access token required', 401);
    }

    const token = authHeader.split(' ')[1];

    // Check if token is blacklisted (optional)
    // const isBlacklisted = await redisClient.sismember('blacklisted_tokens', token);
    // if (isBlacklisted) {
    //   throw new AppError('Token has been revoked', 401);
    // }

    // Verify token
    const decoded = jwt.verify(token, process.env.JWT_SECRET);

    // Get user from database
    const user = await UserModel.findById(decoded.userId);
    if (!user) {
      throw new AppError('User not found', 401);
    }

    // Check if user is active
    if (!user.isActive) {
      throw new AppError('Account has been deactivated', 401);
    }

    // Add user to request object
    req.user = {
      id: user.id,
      email: user.email,
      role: user.role,
      firstName: user.firstName,
      lastName: user.lastName
    };

    next();

  } catch (error) {
    if (error.name === 'JsonWebTokenError') {
      return next(new AppError('Invalid access token', 401));
    }
    if (error.name === 'TokenExpiredError') {
      return next(new AppError('Access token has expired', 401));
    }
    next(error);
  }
};

const optionalAuth = async (req, res, next) => {
  try {
    const authHeader = req.headers.authorization;
    if (!authHeader || !authHeader.startsWith('Bearer ')) {
      return next(); // Continue without authentication
    }

    // Use the same logic as authenticate but don't throw errors
    await authenticate(req, res, next);
  } catch (error) {
    // Continue without authentication if token is invalid
    next();
  }
};

module.exports = {
  authenticate,
  optionalAuth
};
```

### 4. Authorization Middleware (src/middleware/authorization.js)

```javascript
const { AppError } = require('../utils/AppError');

const authorize = (allowedRoles = []) => {
  return (req, res, next) => {
    try {
      if (!req.user) {
        throw new AppError('Authentication required', 401);
      }

      // If no roles specified, just check if user is authenticated
      if (allowedRoles.length === 0) {
        return next();
      }

      // Check if user has required role
      if (!allowedRoles.includes(req.user.role)) {
        throw new AppError('Insufficient permissions', 403);
      }

      next();

    } catch (error) {
      next(error);
    }
  };
};

const authorizeOwner = (resourceOwnerField = 'userId') => {
  return async (req, res, next) => {
    try {
      if (!req.user) {
        throw new AppError('Authentication required', 401);
      }

      // Admin can access everything
      if (req.user.role === 'admin') {
        return next();
      }

      // Check if user owns the resource
      // This assumes the resource has a field that indicates ownership
      // Implementation will depend on your specific use case
      const resourceId = req.params.id;
      
      // You'll need to implement this based on your models
      // const resource = await YourModel.findById(resourceId);
      // if (!resource || resource[resourceOwnerField] !== req.user.id) {
      //   throw new AppError('Access denied', 403);
      // }

      next();

    } catch (error) {
      next(error);
    }
  };
};

module.exports = {
  authorize,
  authorizeOwner
};
```

### 5. Validation Schemas (src/utils/schemas/authSchemas.js)

```javascript
const Joi = require('joi');

const authSchemas = {
  register: Joi.object({
    firstName: Joi.string().required().min(1).max(50).trim(),
    lastName: Joi.string().required().min(1).max(50).trim(),
    email: Joi.string().email().required().lowercase().trim(),
    password: Joi.string()
      .required()
      .min(8)
      .max(128)
      .pattern(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]/)
      .message('Password must contain at least one lowercase letter, one uppercase letter, one digit, and one special character'),
    confirmPassword: Joi.string()
      .required()
      .valid(Joi.ref('password'))
      .messages({ 'any.only': 'Passwords do not match' })
  }),

  login: Joi.object({
    email: Joi.string().email().required().lowercase().trim(),
    password: Joi.string().required()
  }),

  forgotPassword: Joi.object({
    email: Joi.string().email().required().lowercase().trim()
  }),

  resetPassword: Joi.object({
    password: Joi.string()
      .required()
      .min(8)
      .max(128)
      .pattern(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]/)
      .message('Password must contain at least one lowercase letter, one uppercase letter, one digit, and one special character'),
    confirmPassword: Joi.string()
      .required()
      .valid(Joi.ref('password'))
      .messages({ 'any.only': 'Passwords do not match' })
  }),

  refreshToken: Joi.object({
    refreshToken: Joi.string().optional()
  })
};

module.exports = { authSchemas };
```

### 6. Routes (src/routes/authRoutes.js)

```javascript
const express = require('express');
const rateLimit = require('express-rate-limit');
const AuthController = require('../controllers/authController');
const { authenticate } = require('../middleware/auth');

const router = express.Router();

// Rate limiting for auth endpoints
const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 5, // limit each IP to 5 requests per windowMs
  message: {
    success: false,
    error: {
      message: 'Too many authentication attempts, try again later',
      code: 'RATE_LIMIT_EXCEEDED'
    }
  }
});

const passwordResetLimiter = rateLimit({
  windowMs: 60 * 60 * 1000, // 1 hour
  max: 3, // limit each IP to 3 password reset requests per hour
  message: {
    success: false,
    error: {
      message: 'Too many password reset attempts, try again later',
      code: 'RATE_LIMIT_EXCEEDED'
    }
  }
});

// Public routes
router.post('/register', authLimiter, AuthController.register);
router.post('/login', authLimiter, AuthController.login);
router.post('/refresh-token', AuthController.refreshToken);
router.post('/forgot-password', passwordResetLimiter, AuthController.forgotPassword);
router.post('/reset-password/:token', AuthController.resetPassword);
router.get('/verify-email/:token', AuthController.verifyEmail);

// Protected routes
router.post('/logout', authenticate, AuthController.logout);
router.get('/profile', authenticate, AuthController.getProfile);

module.exports = router;
```

## 🔒 Variables de Entorno Requeridas

Añadir a `.env`:

```env
# JWT Configuration
JWT_SECRET=your_super_secret_jwt_key_minimum_32_characters
JWT_EXPIRES_IN=15m
JWT_REFRESH_SECRET=your_super_secret_refresh_key_different_from_above
JWT_REFRESH_EXPIRES_IN=7d
JWT_ISSUER=your-api-name
JWT_AUDIENCE=your-app-name

# Redis Configuration (for refresh tokens)
REDIS_URL=redis://localhost:6379

# Email Configuration (for password reset)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASS=your-app-password
FROM_EMAIL=noreply@yourapp.com
```

## 🧪 Tests de Autenticación

```javascript
// tests/integration/auth.test.js
const request = require('supertest');
const app = require('../../src/app');

describe('Authentication Endpoints', () => {
  describe('POST /api/v1/auth/register', () => {
    it('should register a new user', async () => {
      const userData = {
        firstName: 'John',
        lastName: 'Doe',
        email: 'john@example.com',
        password: 'Password123!',
        confirmPassword: 'Password123!'
      };

      const response = await request(app)
        .post('/api/v1/auth/register')
        .send(userData)
        .expect(201);

      expect(response.body.success).toBe(true);
      expect(response.body.data.user.email).toBe(userData.email);
      expect(response.body.data.accessToken).toBeDefined();
    });
  });

  describe('POST /api/v1/auth/login', () => {
    it('should login with valid credentials', async () => {
      const credentials = {
        email: 'john@example.com',
        password: 'Password123!'
      };

      const response = await request(app)
        .post('/api/v1/auth/login')
        .send(credentials)
        .expect(200);

      expect(response.body.success).toBe(true);
      expect(response.body.data.accessToken).toBeDefined();
    });
  });
});
```

## ✅ Checklist Post-Implementación

- [ ] AuthService implementado con todas las funcionalidades
- [ ] Middleware de autenticación y autorización
- [ ] Rate limiting configurado
- [ ] Refresh tokens con Redis
- [ ] Password hashing seguro
- [ ] Email verification implementado
- [ ] Password reset funcional
- [ ] Account locking por intentos fallidos
- [ ] Tests de integración
- [ ] Variables de entorno configuradas
- [ ] Logging de eventos de seguridad

---

**Tiempo estimado**: 2-3 horas para implementación completa 🔐