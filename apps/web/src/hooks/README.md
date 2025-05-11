# Hooks Directory

This directory contains custom React hooks that encapsulate reusable stateful logic and side effects.

## Purpose

- Share stateful logic
- Handle side effects
- Manage form state
- Implement data fetching
- Handle authentication
- Manage UI state

## Best Practices

1. **Hook Design**
   - Keep hooks focused
   - Use proper naming
   - Handle errors properly
   - Implement proper cleanup

2. **State Management**
   - Use appropriate state
   - Handle loading states
   - Manage error states
   - Implement proper caching

3. **Performance**
   - Use proper dependencies
   - Implement memoization
   - Avoid unnecessary re-renders
   - Handle cleanup properly

4. **Testing**
   - Write unit tests
   - Test edge cases
   - Mock dependencies
   - Test error handling

## Example Hooks

Here's an example of custom hooks:

```typescript
// useAuth.ts
import { useState, useEffect, useCallback } from 'react';
import { useRouter } from 'next/router';
import { API_ENDPOINTS } from '../constant/api';
import { ROUTES } from '../constant/routes';

interface User {
  id: string;
  email: string;
  firstName: string;
  lastName: string;
}

interface AuthState {
  user: User | null;
  isLoading: boolean;
  error: string | null;
}

export const useAuth = () => {
  const router = useRouter();
  const [state, setState] = useState<AuthState>({
    user: null,
    isLoading: true,
    error: null
  });

  const login = useCallback(async (email: string, password: string) => {
    try {
      setState(prev => ({ ...prev, isLoading: true, error: null }));
      
      const response = await fetch(API_ENDPOINTS.AUTH.LOGIN, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email, password })
      });

      if (!response.ok) {
        throw new Error('Invalid credentials');
      }

      const data = await response.json();
      setState({ user: data.user, isLoading: false, error: null });
      router.push(ROUTES.DASHBOARD.ROOT);
    } catch (error) {
      setState(prev => ({
        ...prev,
        isLoading: false,
        error: error instanceof Error ? error.message : 'An error occurred'
      }));
    }
  }, [router]);

  const logout = useCallback(async () => {
    try {
      setState(prev => ({ ...prev, isLoading: true, error: null }));
      
      await fetch(API_ENDPOINTS.AUTH.LOGOUT, {
        method: 'POST',
        credentials: 'include'
      });

      setState({ user: null, isLoading: false, error: null });
      router.push(ROUTES.AUTH.LOGIN);
    } catch (error) {
      setState(prev => ({
        ...prev,
        isLoading: false,
        error: error instanceof Error ? error.message : 'An error occurred'
      }));
    }
  }, [router]);

  const checkAuth = useCallback(async () => {
    try {
      setState(prev => ({ ...prev, isLoading: true, error: null }));
      
      const response = await fetch(API_ENDPOINTS.AUTH.VERIFY, {
        credentials: 'include'
      });

      if (!response.ok) {
        throw new Error('Not authenticated');
      }

      const data = await response.json();
      setState({ user: data.user, isLoading: false, error: null });
    } catch (error) {
      setState({ user: null, isLoading: false, error: null });
    }
  }, []);

  useEffect(() => {
    checkAuth();
  }, [checkAuth]);

  return {
    user: state.user,
    isLoading: state.isLoading,
    error: state.error,
    login,
    logout
  };
};

// useForm.ts
import { useState, useCallback } from 'react';

interface ValidationRules {
  required?: boolean;
  minLength?: number;
  maxLength?: number;
  pattern?: RegExp;
  validate?: (value: any) => boolean | string;
}

interface ValidationErrors {
  [key: string]: string;
}

export const useForm = <T extends Record<string, any>>(
  initialValues: T,
  validationRules?: Partial<Record<keyof T, ValidationRules>>
) => {
  const [values, setValues] = useState<T>(initialValues);
  const [errors, setErrors] = useState<ValidationErrors>({});
  const [touched, setTouched] = useState<Record<string, boolean>>({});

  const validateField = useCallback(
    (name: keyof T, value: any) => {
      if (!validationRules?.[name]) return '';

      const rules = validationRules[name]!;
      if (rules.required && !value) {
        return 'This field is required';
      }
      if (rules.minLength && value.length < rules.minLength) {
        return `Minimum length is ${rules.minLength}`;
      }
      if (rules.maxLength && value.length > rules.maxLength) {
        return `Maximum length is ${rules.maxLength}`;
      }
      if (rules.pattern && !rules.pattern.test(value)) {
        return 'Invalid format';
      }
      if (rules.validate) {
        const result = rules.validate(value);
        if (typeof result === 'string') return result;
        if (!result) return 'Invalid value';
      }
      return '';
    },
    [validationRules]
  );

  const handleChange = useCallback(
    (name: keyof T, value: any) => {
      setValues(prev => ({ ...prev, [name]: value }));
      if (validationRules?.[name]) {
        const error = validateField(name, value);
        setErrors(prev => ({ ...prev, [name]: error }));
      }
    },
    [validateField, validationRules]
  );

  const handleBlur = useCallback((name: keyof T) => {
    setTouched(prev => ({ ...prev, [name]: true }));
    if (validationRules?.[name]) {
      const error = validateField(name, values[name]);
      setErrors(prev => ({ ...prev, [name]: error }));
    }
  }, [validateField, validationRules, values]);

  const handleSubmit = useCallback(
    (onSubmit: (values: T) => void) => (e: React.FormEvent) => {
      e.preventDefault();
      const newErrors: ValidationErrors = {};
      let hasErrors = false;

      if (validationRules) {
        Object.keys(validationRules).forEach(key => {
          const error = validateField(key as keyof T, values[key as keyof T]);
          if (error) {
            newErrors[key] = error;
            hasErrors = true;
          }
        });
      }

      setErrors(newErrors);
      if (!hasErrors) {
        onSubmit(values);
      }
    },
    [validateField, validationRules, values]
  );

  const reset = useCallback(() => {
    setValues(initialValues);
    setErrors({});
    setTouched({});
  }, [initialValues]);

  return {
    values,
    errors,
    touched,
    handleChange,
    handleBlur,
    handleSubmit,
    reset
  };
};
``` 