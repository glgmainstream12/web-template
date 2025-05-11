# Routers Directory

This directory contains the API route definitions that map HTTP endpoints to their corresponding controller functions.

## Purpose

- Define API routes
- Map routes to controllers
- Apply middleware
- Handle route parameters
- Organize API endpoints

## Best Practices

1. **Route Organization**
   - Group related routes together
   - Use meaningful route names
   - Follow RESTful conventions
   - Version your APIs

2. **Middleware Usage**
   - Apply appropriate middleware
   - Order middleware correctly
   - Use route-specific middleware

3. **Error Handling**
   - Implement proper error handling
   - Use error middleware
   - Handle async errors

4. **Documentation**
   - Document routes clearly
   - Include request/response examples
   - Document parameters and body

## Example Router

Here's an example of a user router:

```typescript
// userRouter.ts
import { Router } from 'express';
import { UserController } from '../controllers/userController';
import { authMiddleware, authorize } from '../middlewares/authMiddleware';
import { validateRequest } from '../middlewares/validationMiddleware';
import { createUserSchema, updateUserSchema } from '../validators/userSchema';

const router = Router();
const userController = new UserController();

// Public routes
router.post(
  '/register',
  validateRequest(createUserSchema),
  userController.createUser
);

router.post('/login', userController.login);

// Protected routes
router.use(authMiddleware);

// User routes
router.get('/profile', userController.getProfile);
router.put(
  '/profile',
  validateRequest(updateUserSchema),
  userController.updateProfile
);

// Admin routes
router.use(authorize(['admin']));
router.get('/', userController.getAllUsers);
router.get('/:id', userController.getUserById);
router.put(
  '/:id',
  validateRequest(updateUserSchema),
  userController.updateUser
);
router.delete('/:id', userController.deleteUser);

export default router; 