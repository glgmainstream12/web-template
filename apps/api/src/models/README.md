# Models Directory

This directory contains the database models and schemas that define the structure of your data and handle database interactions.

## Purpose

- Define data structures
- Handle database operations
- Implement data validation
- Manage relationships between entities
- Provide type safety for database operations

## Best Practices

1. **Schema Definition**
   - Define clear and consistent schemas
   - Use appropriate data types
   - Include validation rules
   - Document fields and relationships

2. **Type Safety**
   - Use TypeScript interfaces
   - Define proper types for all fields
   - Include proper return types for methods

3. **Validation**
   - Implement schema validation
   - Use middleware for validation
   - Handle validation errors properly

4. **Relationships**
   - Define clear relationships between models
   - Use appropriate relationship types
   - Handle cascading operations properly

## Example Model

Here's an example of a user model using Mongoose:

```typescript
// userModel.ts
import mongoose, { Document, Schema } from 'mongoose';
import bcrypt from 'bcryptjs';

// Interface for User document
export interface IUser extends Document {
  email: string;
  password: string;
  firstName: string;
  lastName: string;
  role: 'user' | 'admin';
  isActive: boolean;
  createdAt: Date;
  updatedAt: Date;
  comparePassword(candidatePassword: string): Promise<boolean>;
}

// User schema
const userSchema = new Schema<IUser>(
  {
    email: {
      type: String,
      required: [true, 'Email is required'],
      unique: true,
      lowercase: true,
      trim: true,
      match: [/^\S+@\S+\.\S+$/, 'Please use a valid email address']
    },
    password: {
      type: String,
      required: [true, 'Password is required'],
      minlength: [8, 'Password must be at least 8 characters long']
    },
    firstName: {
      type: String,
      required: [true, 'First name is required'],
      trim: true
    },
    lastName: {
      type: String,
      required: [true, 'Last name is required'],
      trim: true
    },
    role: {
      type: String,
      enum: ['user', 'admin'],
      default: 'user'
    },
    isActive: {
      type: Boolean,
      default: true
    }
  },
  {
    timestamps: true
  }
);

// Hash password before saving
userSchema.pre('save', async function(next) {
  if (!this.isModified('password')) return next();

  try {
    const salt = await bcrypt.genSalt(10);
    this.password = await bcrypt.hash(this.password, salt);
    next();
  } catch (error) {
    next(error as Error);
  }
});

// Compare password method
userSchema.methods.comparePassword = async function(
  candidatePassword: string
): Promise<boolean> {
  return bcrypt.compare(candidatePassword, this.password);
};

// Create and export the model
export const User = mongoose.model<IUser>('User', userSchema);
``` 