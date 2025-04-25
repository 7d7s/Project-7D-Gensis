# NextGen MedTech Platform - Theme SDK Guide

## Theme Architecture Overview

### Core Concepts
- Theme inheritance
- Component styling
- Dynamic theming
- Responsive design
- Accessibility compliance

## Theme Structure

```
/theme-project
├── src/
│   ├── components/
│   ├── styles/
│   │   ├── variables.scss
│   │   ├── mixins.scss
│   │   └── global.scss
│   ├── assets/
│   └── index.ts
├── theme.config.js
└── package.json
```

## Theme Development

### 1. Initialize Theme
```bash
nextgen-theme init my-custom-theme
cd my-custom-theme
npm install
```

### 2. Theme Configuration
```javascript
// theme.config.js
module.exports = {
  name: 'my-custom-theme',
  version: '1.0.0',
  extends: '@nextgen-medtech/base-theme',
  variables: {
    colors: {
      primary: '#007AFF',
      secondary: '#5856D6',
      success: '#34C759',
      warning: '#FF9500',
      danger: '#FF3B30',
      info: '#5AC8FA',
      light: '#F2F2F7',
      dark: '#1C1C1E'
    },
    typography: {
      fontFamily: '"SF Pro Display", -apple-system, BlinkMacSystemFont, sans-serif',
      fontSize: '16px',
      lineHeight: 1.5
    },
    spacing: {
      base: '8px',
      small: '4px',
      medium: '16px',
      large: '24px',
      xlarge: '32px'
    },
    breakpoints: {
      sm: '576px',
      md: '768px',
      lg: '992px',
      xl: '1200px'
    }
  }
};
```

### 3. Component Styling

#### Base Styles
```scss
// src/styles/variables.scss
@import '@nextgen-medtech/theme-sdk/variables';

$primary-color: var(--primary-color);
$font-family: var(--font-family);
$spacing-base: var(--spacing-base);

// src/styles/mixins.scss
@mixin flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}

@mixin responsive($breakpoint) {
  @media (min-width: map-get($breakpoints, $breakpoint)) {
    @content;
  }
}
```

#### Component Example
```typescript
// src/components/Button/Button.tsx
import React from 'react';
import styles from './Button.module.scss';

export interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'outline';
  size?: 'small' | 'medium' | 'large';
  children: React.ReactNode;
  onClick?: () => void;
}

export const Button: React.FC<ButtonProps> = ({
  variant = 'primary',
  size = 'medium',
  children,
  onClick
}) => (
  <button
    className={`${styles.button} ${styles[variant]} ${styles[size]}`}
    onClick={onClick}
  >
    {children}
  </button>
);
```

```scss
// src/components/Button/Button.module.scss
@import '../../styles/variables';
@import '../../styles/mixins';

.button {
  border: none;
  border-radius: 8px;
  font-family: $font-family;
  cursor: pointer;
  transition: all 0.2s ease;

  &:hover {
    transform: translateY(-1px);
  }

  &:active {
    transform: translateY(1px);
  }
}

.primary {
  background-color: $primary-color;
  color: white;
}

.secondary {
  background-color: $secondary-color;
  color: white;
}

.outline {
  background-color: transparent;
  border: 2px solid $primary-color;
  color: $primary-color;
}

.small {
  padding: $spacing-small $spacing-base;
  font-size: 14px;
}

.medium {
  padding: $spacing-base $spacing-medium;
  font-size: 16px;
}

.large {
  padding: $spacing-medium $spacing-large;
  font-size: 18px;
}
```

## Theme API Reference

### Core APIs

#### Theme Registration
```typescript
import { registerTheme } from '@nextgen-medtech/theme-sdk';

registerTheme({
  name: 'custom-theme',
  styles: themeStyles,
  components: themeComponents
});
```

#### Dynamic Theming
```typescript
import { useTheme, ThemeProvider } from '@nextgen-medtech/theme-sdk';

// Hook usage
const { theme, setTheme } = useTheme();

// Provider usage
<ThemeProvider theme={customTheme}>
  <App />
</ThemeProvider>
```

### Utility Functions

```typescript
import { createTheme, mergeThemes } from '@nextgen-medtech/theme-sdk';

// Create new theme
const customTheme = createTheme({
  name: 'custom',
  variables: { /* ... */ }
});

// Merge themes
const mergedTheme = mergeThemes(baseTheme, customTheme);
```

## Responsive Design

### Breakpoint Utilities
```typescript
import { useBreakpoint } from '@nextgen-medtech/theme-sdk';

const Component = () => {
  const isMobile = useBreakpoint('sm');
  return (
    <div className={isMobile ? 'mobile-layout' : 'desktop-layout'}>
      {/* Content */}
    </div>
  );
};
```

### Media Queries
```scss
@include responsive(sm) {
  .container {
    padding: $spacing-small;
  }
}

@include responsive(md) {
  .container {
    padding: $spacing-medium;
  }
}
```

## Accessibility

### WCAG Compliance
```scss
// High contrast mode
@media (prefers-contrast: high) {
  :root {
    --primary-color: #0056b3;
    --contrast-ratio: 7:1;
  }
}

// Reduced motion
@media (prefers-reduced-motion: reduce) {
  * {
    animation: none !important;
    transition: none !important;
  }
}
```

## Testing

### Visual Regression Testing
```typescript
import { ThemeTester } from '@nextgen-medtech/theme-sdk/testing';

describe('Theme Visual Tests', () => {
  let tester: ThemeTester;

  beforeEach(() => {
    tester = new ThemeTester();
  });

  it('should match snapshot', async () => {
    const component = await tester.render(Button, {
      variant: 'primary',
      children: 'Test Button'
    });
    expect(component).toMatchSnapshot();
  });
});
```

## Build and Deployment

### Build Theme
```bash
npm run build
```

### Package Theme
```bash
nextgen-theme package
```

### Deploy Theme
```bash
nextgen-theme deploy --env production
```

## Best Practices

### Performance
- Use CSS variables for dynamic values
- Implement code splitting
- Optimize asset loading
- Minimize CSS bundle size

### Maintainability
- Follow BEM naming convention
- Document component variants
- Create reusable mixins
- Maintain consistent spacing

### Accessibility
- Ensure sufficient color contrast
- Support keyboard navigation
- Implement ARIA attributes
- Test with screen readers

## Troubleshooting

### Common Issues
1. Theme inheritance conflicts
2. Responsive design breakpoints
3. CSS specificity problems
4. Performance bottlenecks

### Debugging
```typescript
// Enable theme debug mode
setThemeDebugMode(true);

// Log theme changes
enableThemeLogging();
```

## Support

### Resources
- Documentation: https://docs.nextgenmedtech.com/themes
- Component Library: https://components.nextgenmedtech.com
- Design System: https://design.nextgenmedtech.com

### Contact
- Email: theme-support@nextgenmedtech.com
- Discord: https://discord.gg/nextgenmedtech
- GitHub: https://github.com/nextgenmedtech/theme-sdk