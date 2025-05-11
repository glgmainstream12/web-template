# Middlewares Directory

This directory contains Express middleware functions that handle cross-cutting concerns like authentication, logging, error handling, and request processing.

## Purpose

- Handle cross-cutting concerns
- Process requests before they reach routes
- Add functionality to the request/response cycle
- Implement security measures
- Handle errors globally

## Best Practices

1. **Single Responsibility**
   - Each middleware should do one thing
   - Keep middleware functions focused and small

2. **Error Handling**
   - Implement proper error handling
   - Use next() function correctly
   - Handle async errors properly

3. **Security**
   - Implement security best practices
   - Validate and sanitize input
   - Handle authentication and authorization

4. **Performance**
   - Keep middleware lightweight
   - Avoid blocking operations
   - Use caching when appropriate

## Example Middleware

Here's an example of an authentication middleware:

```typescript
// authMiddleware.ts
import { Request, Response, NextFunction } from 'express';
import jwt from 'jsonwebtoken';
import { UnauthorizedError } from '../utils/errors';

interface AuthRequest extends Request {
  user?: {
    id: string;
    email: string;
    role: string;
  };
}

export const authMiddleware = async (
  req: AuthRequest,
  res: Response,
  next: NextFunction
): Promise<void> => {
  try {
    // Get token from header
    const authHeader = req.headers.authorization;
    if (!authHeader?.startsWith('Bearer ')) {
      throw new UnauthorizedError('No token provided');
    }

    const token = authHeader.split(' ')[1];

    // Verify token
    const decoded = jwt.verify(token, process.env.JWT_SECRET!) as {
      id: string;
      email: string;
      role: string;
    };

    // Add user to request
    req.user = {
      id: decoded.id,
      email: decoded.email,
      role: decoded.role
    };

    next();
  } catch (error) {
    if (error instanceof jwt.JsonWebTokenError) {
      next(new UnauthorizedError('Invalid token'));
    } else {
      next(error);
    }
  }
};

// Role-based authorization middleware
export const authorize = (roles: string[]) => {
  return (req: AuthRequest, res: Response, next: NextFunction): void => {
    if (!req.user) {
      next(new UnauthorizedError('User not authenticated'));
      return;
    }

    if (!roles.includes(req.user.role)) {
      next(new UnauthorizedError('User not authorized'));
      return;
    }

    next();
  };
};
``` 