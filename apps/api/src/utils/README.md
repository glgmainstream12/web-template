# Utils Directory

This directory contains utility functions, helpers, and common tools used throughout the application.

## Purpose

- Provide reusable utility functions
- Handle common operations
- Implement helper methods
- Define custom error classes
- Share common constants

## Best Practices

1. **Function Organization**
   - Group related functions together
   - Keep functions pure when possible
   - Use meaningful function names
   - Document function parameters and return types

2. **Error Handling**
   - Define custom error classes
   - Implement proper error messages
   - Use appropriate error types
   - Handle edge cases

3. **Type Safety**
   - Use TypeScript types/interfaces
   - Implement proper type guards
   - Use generics when appropriate
   - Document complex types

4. **Testing**
   - Write unit tests for utilities
   - Test edge cases
   - Ensure proper error handling
   - Mock external dependencies

## Example Utils

Here's an example of utility functions and error classes:

```typescript
// errors.ts
export class AppError extends Error {
  constructor(
    public message: string,
    public statusCode: number = 500,
    public isOperational = true
  ) {
    super(message);
    Object.setPrototypeOf(this, AppError.prototype);
  }
}

export class ValidationError extends AppError {
  constructor(message: string) {
    super(message, 400);
  }
}

export class UnauthorizedError extends AppError {
  constructor(message: string = 'Unauthorized') {
    super(message, 401);
  }
}

export class NotFoundError extends AppError {
  constructor(message: string = 'Resource not found') {
    super(message, 404);
  }
}

// validation.ts
export const isValidEmail = (email: string): boolean => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
};

export const isValidPassword = (password: string): boolean => {
  // At least 8 characters, 1 uppercase, 1 lowercase, 1 number
  const passwordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d]{8,}$/;
  return passwordRegex.test(password);
};

// dateUtils.ts
export const formatDate = (date: Date): string => {
  return date.toISOString();
};

export const parseDate = (dateString: string): Date => {
  const date = new Date(dateString);
  if (isNaN(date.getTime())) {
    throw new ValidationError('Invalid date format');
  }
  return date;
};

// stringUtils.ts
export const capitalize = (str: string): string => {
  if (!str) return str;
  return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase();
};

export const slugify = (str: string): string => {
  return str
    .toLowerCase()
    .trim()
    .replace(/[^\w\s-]/g, '')
    .replace(/[\s_-]+/g, '-')
    .replace(/^-+|-+$/g, '');
};

// asyncUtils.ts
export const sleep = (ms: number): Promise<void> => {
  return new Promise(resolve => setTimeout(resolve, ms));
};

export const retry = async <T>(
  fn: () => Promise<T>,
  retries: number = 3,
  delay: number = 1000
): Promise<T> => {
  try {
    return await fn();
  } catch (error) {
    if (retries === 0) throw error;
    await sleep(delay);
    return retry(fn, retries - 1, delay);
  }
};
``` 