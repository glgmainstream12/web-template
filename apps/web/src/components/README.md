# Components Directory

This directory contains reusable React components used throughout the application.

## Purpose

- Store reusable UI components
- Implement component logic
- Handle component state
- Manage component styling
- Share common UI elements

## Best Practices

1. **Component Organization**
   - Group related components
   - Use atomic design principles
   - Implement proper folder structure
   - Share common components

2. **Component Design**
   - Keep components small and focused
   - Use proper prop types
   - Implement proper error handling
   - Follow React best practices

3. **State Management**
   - Use appropriate state management
   - Implement proper data flow
   - Handle side effects correctly
   - Use React hooks effectively

4. **Styling**
   - Use consistent styling approach
   - Implement responsive design
   - Follow accessibility guidelines
   - Use CSS modules or styled-components

## Example Component

Here's an example of a reusable button component:

```typescript
// Button.tsx
import React from 'react';
import styles from './Button.module.css';

export type ButtonVariant = 'primary' | 'secondary' | 'outline' | 'text';
export type ButtonSize = 'small' | 'medium' | 'large';

export interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: ButtonVariant;
  size?: ButtonSize;
  isLoading?: boolean;
  leftIcon?: React.ReactNode;
  rightIcon?: React.ReactNode;
  fullWidth?: boolean;
}

export const Button: React.FC<ButtonProps> = ({
  children,
  variant = 'primary',
  size = 'medium',
  isLoading = false,
  leftIcon,
  rightIcon,
  fullWidth = false,
  className,
  disabled,
  ...props
}) => {
  const buttonClasses = [
    styles.button,
    styles[variant],
    styles[size],
    fullWidth && styles.fullWidth,
    isLoading && styles.loading,
    className
  ]
    .filter(Boolean)
    .join(' ');

  return (
    <button
      className={buttonClasses}
      disabled={disabled || isLoading}
      {...props}
    >
      {isLoading && (
        <span className={styles.spinner} aria-hidden="true">
          <svg
            className={styles.spinnerIcon}
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <circle
              className={styles.spinnerCircle}
              cx="12"
              cy="12"
              r="10"
              stroke="currentColor"
              strokeWidth="4"
            />
          </svg>
        </span>
      )}
      {!isLoading && leftIcon && (
        <span className={styles.leftIcon}>{leftIcon}</span>
      )}
      <span className={styles.content}>{children}</span>
      {!isLoading && rightIcon && (
        <span className={styles.rightIcon}>{rightIcon}</span>
      )}
    </button>
  );
};

// Button.module.css
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
  border: none;
  outline: none;
}

.button:disabled {
  cursor: not-allowed;
  opacity: 0.6;
}

/* Variants */
.primary {
  background-color: var(--primary-color);
  color: white;
}

.primary:hover:not(:disabled) {
  background-color: var(--primary-color-dark);
}

.secondary {
  background-color: var(--secondary-color);
  color: white;
}

.secondary:hover:not(:disabled) {
  background-color: var(--secondary-color-dark);
}

.outline {
  background-color: transparent;
  border: 1px solid var(--primary-color);
  color: var(--primary-color);
}

.outline:hover:not(:disabled) {
  background-color: var(--primary-color-light);
}

.text {
  background-color: transparent;
  color: var(--primary-color);
  padding: 0;
}

.text:hover:not(:disabled) {
  background-color: var(--primary-color-light);
}

/* Sizes */
.small {
  padding: 8px 16px;
  font-size: 14px;
}

.medium {
  padding: 12px 24px;
  font-size: 16px;
}

.large {
  padding: 16px 32px;
  font-size: 18px;
}

/* Full width */
.fullWidth {
  width: 100%;
}

/* Loading state */
.loading {
  position: relative;
  color: transparent;
}

.spinner {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.spinnerIcon {
  width: 20px;
  height: 20px;
  animation: spin 1s linear infinite;
}

.spinnerCircle {
  stroke-dasharray: 60;
  stroke-dashoffset: 50;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

/* Icons */
.leftIcon {
  margin-right: 8px;
}

.rightIcon {
  margin-left: 8px;
}
``` 