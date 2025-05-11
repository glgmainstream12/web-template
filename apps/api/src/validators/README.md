# Validators Directory

This directory contains validation schemas and functions used to validate request data and ensure data integrity.

## Purpose

- Define validation schemas
- Validate request data
- Ensure data integrity
- Provide type safety
- Handle validation errors

## Best Practices

1. **Schema Definition**
   - Use clear and consistent schemas
   - Define proper validation rules
   - Include meaningful error messages
   - Document schema requirements

2. **Type Safety**
   - Use TypeScript types/interfaces
   - Define proper input/output types
   - Use type inference when possible
   - Document complex types

3. **Error Handling**
   - Provide clear error messages
   - Use appropriate error types
   - Handle edge cases
   - Implement custom validators

4. **Reusability**
   - Create reusable validation rules
   - Share common schemas
   - Use composition for complex schemas
   - Maintain consistency

## Example Validator

Here's an example of validation schemas using Joi:

```typescript
// userSchema.ts
import Joi from 'joi';
import { ValidationError } from '../utils/errors';

// Base user schema
const baseUserSchema = {
  firstName: Joi.string()
    .min(2)
    .max(50)
    .required()
    .messages({
      'string.empty': 'First name is required',
      'string.min': 'First name must be at least 2 characters long',
      'string.max': 'First name cannot exceed 50 characters'
    }),
  lastName: Joi.string()
    .min(2)
    .max(50)
    .required()
    .messages({
      'string.empty': 'Last name is required',
      'string.min': 'Last name must be at least 2 characters long',
      'string.max': 'Last name cannot exceed 50 characters'
    }),
  email: Joi.string()
    .email()
    .required()
    .messages({
      'string.empty': 'Email is required',
      'string.email': 'Please provide a valid email address'
    })
};

// Create user schema
export const createUserSchema = Joi.object({
  ...baseUserSchema,
  password: Joi.string()
    .min(8)
    .pattern(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d]{8,}$/)
    .required()
    .messages({
      'string.empty': 'Password is required',
      'string.min': 'Password must be at least 8 characters long',
      'string.pattern.base':
        'Password must contain at least one uppercase letter, one lowercase letter, and one number'
    }),
  confirmPassword: Joi.string()
    .valid(Joi.ref('password'))
    .required()
    .messages({
      'any.only': 'Passwords do not match'
    })
});

// Update user schema
export const updateUserSchema = Joi.object({
  ...baseUserSchema,
  password: Joi.string()
    .min(8)
    .pattern(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d]{8,}$/)
    .optional()
    .messages({
      'string.min': 'Password must be at least 8 characters long',
      'string.pattern.base':
        'Password must contain at least one uppercase letter, one lowercase letter, and one number'
    }),
  confirmPassword: Joi.string()
    .valid(Joi.ref('password'))
    .optional()
    .messages({
      'any.only': 'Passwords do not match'
    })
});

// Validation middleware
export const validateRequest = (schema: Joi.Schema) => {
  return (req: Request, res: Response, next: NextFunction) => {
    const { error } = schema.validate(req.body, {
      abortEarly: false,
      stripUnknown: true
    });

    if (error) {
      const errorMessage = error.details
        .map(detail => detail.message)
        .join(', ');
      throw new ValidationError(errorMessage);
    }

    next();
  };
};

// Custom validators
export const validatePassword = (password: string): boolean => {
  const passwordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d]{8,}$/;
  return passwordRegex.test(password);
};

export const validateEmail = (email: string): boolean => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
};
``` 