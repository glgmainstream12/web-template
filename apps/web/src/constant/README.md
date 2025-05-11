# Constant Directory

This directory contains application-wide constants, configuration values, and static data that don't change frequently.

## Purpose

- Store application constants
- Define configuration values
- Share static data
- Centralize common values
- Maintain consistency

## Best Practices

1. **Organization**
   - Group related constants
   - Use meaningful names
   - Implement proper typing
   - Document values

2. **Type Safety**
   - Use TypeScript types
   - Define proper interfaces
   - Use enums when appropriate
   - Implement type guards

3. **Maintenance**
   - Keep constants up to date
   - Document changes
   - Review periodically
   - Remove unused constants

4. **Usage**
   - Import only what's needed
   - Use proper naming conventions
   - Follow DRY principle
   - Maintain consistency

## Example Constants

Here's an example of application constants:

```typescript
// api.ts
export const API_BASE_URL = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:3000/api';

export const API_ENDPOINTS = {
  AUTH: {
    LOGIN: '/auth/login',
    REGISTER: '/auth/register',
    LOGOUT: '/auth/logout',
    REFRESH: '/auth/refresh',
    VERIFY: '/auth/verify'
  },
  USER: {
    PROFILE: '/user/profile',
    UPDATE: '/user/update',
    DELETE: '/user/delete'
  }
} as const;

// routes.ts
export const ROUTES = {
  HOME: '/',
  AUTH: {
    LOGIN: '/auth/login',
    REGISTER: '/auth/register',
    FORGOT_PASSWORD: '/auth/forgot-password',
    RESET_PASSWORD: '/auth/reset-password'
  },
  DASHBOARD: {
    ROOT: '/dashboard',
    PROFILE: '/dashboard/profile',
    SETTINGS: '/dashboard/settings'
  }
} as const;

// theme.ts
export const THEME = {
  COLORS: {
    PRIMARY: {
      MAIN: '#007AFF',
      LIGHT: '#4DA3FF',
      DARK: '#0055B3'
    },
    SECONDARY: {
      MAIN: '#5856D6',
      LIGHT: '#7A79E0',
      DARK: '#3E3D96'
    },
    SUCCESS: {
      MAIN: '#34C759',
      LIGHT: '#5CD679',
      DARK: '#248A3D'
    },
    ERROR: {
      MAIN: '#FF3B30',
      LIGHT: '#FF6961',
      DARK: '#B32921'
    },
    WARNING: {
      MAIN: '#FF9500',
      LIGHT: '#FFAA33',
      DARK: '#B36800'
    },
    INFO: {
      MAIN: '#5856D6',
      LIGHT: '#7A79E0',
      DARK: '#3E3D96'
    },
    GREY: {
      50: '#F9FAFB',
      100: '#F3F4F6',
      200: '#E5E7EB',
      300: '#D1D5DB',
      400: '#9CA3AF',
      500: '#6B7280',
      600: '#4B5563',
      700: '#374151',
      800: '#1F2937',
      900: '#111827'
    }
  },
  TYPOGRAPHY: {
    FONT_FAMILY: {
      PRIMARY: 'Inter, sans-serif',
      SECONDARY: 'Roboto, sans-serif'
    },
    FONT_SIZE: {
      XS: '0.75rem',
      SM: '0.875rem',
      BASE: '1rem',
      LG: '1.125rem',
      XL: '1.25rem',
      '2XL': '1.5rem',
      '3XL': '1.875rem',
      '4XL': '2.25rem'
    },
    FONT_WEIGHT: {
      LIGHT: 300,
      REGULAR: 400,
      MEDIUM: 500,
      SEMIBOLD: 600,
      BOLD: 700
    },
    LINE_HEIGHT: {
      NONE: 1,
      TIGHT: 1.25,
      SNUG: 1.375,
      NORMAL: 1.5,
      RELAXED: 1.625,
      LOOSE: 2
    }
  },
  SPACING: {
    XS: '0.25rem',
    SM: '0.5rem',
    MD: '1rem',
    LG: '1.5rem',
    XL: '2rem',
    '2XL': '3rem',
    '3XL': '4rem'
  },
  BREAKPOINTS: {
    XS: '320px',
    SM: '640px',
    MD: '768px',
    LG: '1024px',
    XL: '1280px',
    '2XL': '1536px'
  },
  BORDER_RADIUS: {
    NONE: '0',
    SM: '0.125rem',
    BASE: '0.25rem',
    MD: '0.375rem',
    LG: '0.5rem',
    XL: '0.75rem',
    '2XL': '1rem',
    FULL: '9999px'
  },
  SHADOWS: {
    SM: '0 1px 2px 0 rgba(0, 0, 0, 0.05)',
    BASE: '0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06)',
    MD: '0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06)',
    LG: '0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05)',
    XL: '0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04)'
  }
} as const;

// validation.ts
export const VALIDATION = {
  PASSWORD: {
    MIN_LENGTH: 8,
    MAX_LENGTH: 32,
    PATTERN: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d]{8,}$/
  },
  EMAIL: {
    PATTERN: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  },
  USERNAME: {
    MIN_LENGTH: 3,
    MAX_LENGTH: 20,
    PATTERN: /^[a-zA-Z0-9_-]{3,20}$/
  }
} as const;
``` 