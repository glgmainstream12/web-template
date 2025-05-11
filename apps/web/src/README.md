# Web Frontend Source Directory

This directory contains the source code for the Next.js frontend application.

## Directory Structure

```
src/
├── components/     # Reusable React components
├── constant/       # Constants and configuration values
├── hooks/          # Custom React hooks
├── lib/           # Utility functions and third-party integrations
├── pages/         # Next.js pages and API routes
├── styles/        # Global styles and CSS modules
└── types/         # TypeScript type definitions
```

## Key Components

### Components
Reusable React components that can be used across different pages. Components should be organized by feature or type.

### Constant
Application-wide constants, configuration values, and static data that don't change frequently.

### Hooks
Custom React hooks that encapsulate reusable stateful logic and side effects.

### Lib
Utility functions, API clients, and third-party service integrations.

### Pages
Next.js pages and API routes. Each file in this directory automatically becomes a route in your application.

### Styles
Global styles, CSS modules, and styling utilities. This includes:
- Global CSS
- CSS Modules
- Styled Components
- Theme configurations

### Types
TypeScript type definitions and interfaces used throughout the application.

## Development Guidelines

1. Follow the component-based architecture
2. Keep components small and focused
3. Use TypeScript for type safety
4. Implement proper error boundaries
5. Follow Next.js best practices for routing and data fetching
6. Use CSS modules or styled-components for styling
7. Write tests for components and hooks
8. Document components using JSDoc or similar tools

## Best Practices

1. **Component Organization**
   - Group related components together
   - Use index files for clean exports
   - Follow atomic design principles when appropriate

2. **State Management**
   - Use React hooks for local state
   - Consider context for global state
   - Use appropriate data fetching methods (getStaticProps, getServerSideProps)

3. **Performance**
   - Implement proper code splitting
   - Use Next.js image optimization
   - Optimize bundle size
   - Implement proper caching strategies 