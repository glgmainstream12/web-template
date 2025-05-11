# Tests Directory

This directory contains test files for the application, including unit tests, integration tests, and end-to-end tests.

## Purpose

- Test application functionality
- Ensure code quality
- Prevent regressions
- Document expected behavior
- Validate business logic

## Best Practices

1. **Test Organization**
   - Group tests by feature/component
   - Use descriptive test names
   - Follow AAA pattern (Arrange, Act, Assert)
   - Keep tests independent

2. **Test Coverage**
   - Aim for high test coverage
   - Test edge cases
   - Test error scenarios
   - Test business logic thoroughly

3. **Test Types**
   - Unit tests for individual components
   - Integration tests for component interactions
   - End-to-end tests for complete flows
   - Performance tests when needed

4. **Test Data**
   - Use test fixtures
   - Clean up test data
   - Use meaningful test data
   - Mock external dependencies

## Example Test

Here's an example of a user service test:

```typescript
// userService.test.ts
import { UserService } from '../services/userService';
import { UserRepository } from '../repositories/userRepository';
import { ValidationError } from '../utils/errors';

// Mock the UserRepository
jest.mock('../repositories/userRepository');

describe('UserService', () => {
  let userService: UserService;
  let mockUserRepository: jest.Mocked<UserRepository>;

  beforeEach(() => {
    // Clear all mocks before each test
    jest.clearAllMocks();
    
    // Create a new instance of UserService with mocked repository
    mockUserRepository = new UserRepository() as jest.Mocked<UserRepository>;
    userService = new UserService(mockUserRepository);
  });

  describe('createUser', () => {
    const validUserData = {
      email: 'test@example.com',
      password: 'password123',
      firstName: 'John',
      lastName: 'Doe'
    };

    it('should create a new user successfully', async () => {
      // Arrange
      const expectedUser = {
        id: '1',
        ...validUserData,
        role: 'user',
        isActive: true,
        createdAt: new Date(),
        updatedAt: new Date()
      };
      mockUserRepository.findByEmail.mockResolvedValue(null);
      mockUserRepository.create.mockResolvedValue(expectedUser);

      // Act
      const result = await userService.createUser(validUserData);

      // Assert
      expect(result).toEqual(expectedUser);
      expect(mockUserRepository.findByEmail).toHaveBeenCalledWith(validUserData.email);
      expect(mockUserRepository.create).toHaveBeenCalledWith(validUserData);
    });

    it('should throw ValidationError if user already exists', async () => {
      // Arrange
      const existingUser = {
        id: '1',
        ...validUserData,
        role: 'user',
        isActive: true,
        createdAt: new Date(),
        updatedAt: new Date()
      };
      mockUserRepository.findByEmail.mockResolvedValue(existingUser);

      // Act & Assert
      await expect(userService.createUser(validUserData)).rejects.toThrow(
        ValidationError
      );
      expect(mockUserRepository.findByEmail).toHaveBeenCalledWith(validUserData.email);
      expect(mockUserRepository.create).not.toHaveBeenCalled();
    });

    it('should throw ValidationError if required fields are missing', async () => {
      // Arrange
      const invalidUserData = {
        email: 'test@example.com'
        // Missing password, firstName, lastName
      };

      // Act & Assert
      await expect(userService.createUser(invalidUserData as any)).rejects.toThrow(
        ValidationError
      );
      expect(mockUserRepository.findByEmail).not.toHaveBeenCalled();
      expect(mockUserRepository.create).not.toHaveBeenCalled();
    });
  });
});
``` 