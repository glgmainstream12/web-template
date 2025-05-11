# Services Directory

This directory contains the business logic layer of the application. Services handle complex operations, external service integrations, and data transformations.

## Purpose

- Encapsulate business logic
- Handle complex operations
- Manage external service integrations
- Transform data between layers
- Implement reusable business rules

## Best Practices

1. **Single Responsibility**
   - Each service should have one primary responsibility
   - Keep services focused and cohesive

2. **Dependency Injection**
   - Use dependency injection for better testability
   - Avoid direct instantiation of dependencies

3. **Error Handling**
   - Implement proper error handling
   - Use custom error types
   - Provide meaningful error messages

4. **Type Safety**
   - Use TypeScript interfaces for input/output
   - Document complex data structures

## Example Service

Here's an example of a user service:

```typescript
// userService.ts
import { User, CreateUserDTO, UpdateUserDTO } from '../types/user';
import { UserRepository } from '../repositories/userRepository';
import { ValidationError } from '../utils/errors';

export class UserService {
  constructor(private userRepository: UserRepository) {}

  async createUser(userData: CreateUserDTO): Promise<User> {
    // Validate user data
    if (!userData.email || !userData.password) {
      throw new ValidationError('Email and password are required');
    }

    // Check if user exists
    const existingUser = await this.userRepository.findByEmail(userData.email);
    if (existingUser) {
      throw new ValidationError('User already exists');
    }

    // Create user
    const user = await this.userRepository.create(userData);
    return user;
  }

  async updateUser(id: string, userData: UpdateUserDTO): Promise<User> {
    // Validate user exists
    const user = await this.userRepository.findById(id);
    if (!user) {
      throw new ValidationError('User not found');
    }

    // Update user
    const updatedUser = await this.userRepository.update(id, userData);
    return updatedUser;
  }

  async deleteUser(id: string): Promise<void> {
    const user = await this.userRepository.findById(id);
    if (!user) {
      throw new ValidationError('User not found');
    }

    await this.userRepository.delete(id);
  }
} 