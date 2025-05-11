# Controllers Directory

This directory contains the request handlers that process incoming HTTP requests and send responses. Controllers act as the interface between the HTTP layer and the business logic layer.

## Purpose

- Handle HTTP requests and responses
- Validate request data
- Call appropriate services
- Format response data
- Handle errors and send appropriate status codes

## Best Practices

1. **Thin Controllers**
   - Keep controllers thin
   - Move business logic to services
   - Focus on request/response handling

2. **Error Handling**
   - Use try-catch blocks
   - Implement proper error responses
   - Use HTTP status codes correctly

3. **Request Validation**
   - Validate request data before processing
   - Use validation middleware when possible
   - Return clear validation error messages

4. **Response Formatting**
   - Maintain consistent response format
   - Use proper HTTP status codes
   - Include relevant metadata

## Example Controller

Here's an example of a user controller:

```typescript
// userController.ts
import { Request, Response, NextFunction } from 'express';
import { UserService } from '../services/userService';
import { ValidationError } from '../utils/errors';

export class UserController {
  constructor(private userService: UserService) {}

  async createUser = async (
    req: Request,
    res: Response,
    next: NextFunction
  ): Promise<void> => {
    try {
      const user = await this.userService.createUser(req.body);
      res.status(201).json({
        success: true,
        data: user,
        message: 'User created successfully'
      });
    } catch (error) {
      if (error instanceof ValidationError) {
        res.status(400).json({
          success: false,
          error: error.message
        });
      } else {
        next(error);
      }
    }
  };

  async updateUser = async (
    req: Request,
    res: Response,
    next: NextFunction
  ): Promise<void> => {
    try {
      const { id } = req.params;
      const user = await this.userService.updateUser(id, req.body);
      res.json({
        success: true,
        data: user,
        message: 'User updated successfully'
      });
    } catch (error) {
      if (error instanceof ValidationError) {
        res.status(400).json({
          success: false,
          error: error.message
        });
      } else {
        next(error);
      }
    }
  };

  async deleteUser = async (
    req: Request,
    res: Response,
    next: NextFunction
  ): Promise<void> => {
    try {
      const { id } = req.params;
      await this.userService.deleteUser(id);
      res.json({
        success: true,
        message: 'User deleted successfully'
      });
    } catch (error) {
      if (error instanceof ValidationError) {
        res.status(400).json({
          success: false,
          error: error.message
        });
      } else {
        next(error);
      }
    }
  };
} 