---
name: 'design-system'
description: 'Guide for working with the Chakra UI-based Design System. This skill should be used when users need to create or modify UI components, work with theme tokens, implement design patterns, or ensure accessibility and visual consistency across the application. Triggers on requests like "create a component", "add a button", "style this element", "use the design system", or any UI-related task.'
---

# Design System

Build consistent, accessible, and maintainable UIs using the Chakra UI-based Design System with PM-prefixed components and theme tokens.

## Core Principles

1. **Token-based design**: All styling uses theme tokens (colors, spacing, typography) - never hard-coded values
2. **Component wrapping**: Wrap Chakra UI primitives with custom PM-prefixed components
3. **Accessibility first**: Leverage Chakra's built-in ARIA support and keyboard navigation
4. **Centralized exports**: Import PM components from a central index to prevent direct Chakra UI usage
5. **Design-code sync**: Theme tokens align with Figma design tokens

## Available Components

All PM components are exported from `@packmind/ui`:

### Typography
- `PMHeading` - Headings (h1-h6)
- `PMText` - Body text and paragraphs
- `PMLink` - Links and navigation

### Form Components
- `PMButton` - Buttons with variants (primary, secondary, tertiary, outline, ghost)
- `PMInput` - Text inputs
- `PMTextArea` - Multi-line text inputs
- `PMLabel` - Form field labels
- `PMFormContainer` - Form layout wrapper
- `PMNativeSelect` - Dropdown select
- `PMMenu` - Menu and dropdown components
- `PMDialog` - Modal dialogs

### Layout Components
Exported from `lib/components/layout`

### Content Components
Exported from `lib/components/content` (includes `PMTable`)

### Feedback Components
Exported from `lib/components/feedback`

### Navigation Components
Exported from `lib/components/navigation` (includes `PMTabs`)

## Creating Components

### Workflow

1. **Never import Chakra UI directly** in feature code - always wrap with PM components
2. **Define the PM component** in `packages/ui/src/lib/components/`
3. **Use forwardRef** to support ref forwarding
4. **Extend Chakra props** to inherit ARIA, event handlers, and style props
5. **Use theme tokens** via curly brace syntax: `{colors.background.primary}`
6. **Export from index** at `packages/ui/src/index.ts`

### Component Template

```tsx
import { Button, ButtonProps } from '@chakra-ui/react';
import { forwardRef } from 'react';

interface IPMButtonProps extends ButtonProps {
  variant?: 'primary' | 'secondary' | 'tertiary' | 'outline' | 'ghost';
  children: React.ReactNode;
}

export const PMButton = forwardRef<HTMLButtonElement, IPMButtonProps>(
  (props, ref) => {
    const { children, variant = 'primary', ...buttonProps } = props;

    return (
      <Button ref={ref} {...buttonProps} variant={variant}>
        {children}
      </Button>
    );
  }
);
```

### Key Patterns

**DO:**
- ✅ Wrap Chakra primitives (Box, Flex, Text, Button)
- ✅ Use theme tokens: `{colors.blue.500}`, `{spacing.4}`
- ✅ Extend Chakra component props interfaces
- ✅ Use forwardRef for ref forwarding
- ✅ Export from central index file
- ✅ Include ARIA attributes automatically via Chakra

**DON'T:**
- ❌ Hard-code colors: `#FF5733`, `rgb(255, 87, 51)`
- ❌ Hard-code spacing: `padding: 16px`, `margin: 1rem`
- ❌ Create raw HTML elements: `<div style={{...}}>`
- ❌ Import Chakra UI directly in app code
- ❌ Use inline styles

## Theme Configuration

Theme is defined in `packages/ui/src/lib/theme/theme.ts` using `defineConfig`:

```typescript
import { defineConfig } from '@chakra-ui/react';

export const packmindTheme = defineConfig({
  globalCss: {
    'html, body': {
      backgroundColor: '{colors.background.secondary}',
      color: '{colors.text.primary}',
    },
  },
  cssVarsPrefix: 'pm',
  theme: {
    tokens: {
      colors: {
        beige: { /* color scale */ },
        blue: { /* color scale */ },
        orange: { /* color scale */ },
        // Semantic tokens
        background: {
          primary: { value: '{colors.beige.0}' },
          secondary: { value: '{colors.beige.50}' },
        },
        text: {
          primary: { value: '{colors.beige.1000}' },
        },
      },
      spacing: { /* spacing scale */ },
      fonts: { /* font definitions */ },
    },
    recipes: {
      button: pmButtonRecipe,
      heading: pmHeadingRecipe,
      // Component-specific styling
    },
  },
});
```

## Application Setup

Wrap the app root with `UIProvider`:

```tsx
import { UIProvider } from '@packmind/ui';

function App() {
  return (
    <UIProvider>
      {/* Your app content */}
    </UIProvider>
  );
}
```

The `UIProvider` wraps `ChakraProvider` and configures the theme system.

## Extending the Theme

### Adding New Colors

```typescript
// In theme.ts
theme: {
  tokens: {
    colors: {
      // Add new brand color
      purple: {
        500: { value: '#8B5CF6' },
        600: { value: '#7C3AED' },
      },
      // Add semantic token
      background: {
        tertiary: { value: '{colors.purple.500}' },
      },
    },
  },
}
```

### Adding Component Recipes

Create a `.recipe.ts` file for component-specific styling:

```typescript
// PMButton.recipe.ts
import { defineRecipe } from '@chakra-ui/react';

export const pmButtonRecipe = defineRecipe({
  base: {
    borderRadius: 'md',
    fontWeight: 'medium',
  },
  variants: {
    variant: {
      primary: {
        bg: '{colors.blue.500}',
        color: 'white',
        _hover: { bg: '{colors.blue.600}' },
      },
      secondary: {
        bg: '{colors.orange.500}',
        color: 'white',
        _hover: { bg: '{colors.orange.600}' },
      },
    },
  },
});
```

## Common Patterns

### Creating Layout Components

```tsx
import { Box, Flex } from '@chakra-ui/react';
import { forwardRef } from 'react';

interface IPMCardProps {
  children: React.ReactNode;
  padding?: string;
}

export const PMCard = forwardRef<HTMLDivElement, IPMCardProps>(
  ({ children, padding = '4' }, ref) => {
    return (
      <Box
        ref={ref}
        bg="{colors.background.primary}"
        borderRadius="md"
        padding={padding}
        boxShadow="sm"
      >
        {children}
      </Box>
    );
  }
);
```

### Using Responsive Tokens

```tsx
<PMBox
  padding={{ base: '2', md: '4', lg: '6' }}
  fontSize={{ base: 'sm', md: 'md' }}
>
  Responsive content
</PMBox>
```

### Composing Components

```tsx
import { PMButton, PMText } from '@packmind/ui';

export const MyFeature = () => {
  return (
    <div>
      <PMText variant="body">Click the button</PMText>
      <PMButton variant="primary" onClick={() => {}}>
        Submit
      </PMButton>
    </div>
  );
};
```

## Storybook Documentation

Every new component must have a Storybook story. See [references/storybook-guide.md](references/storybook-guide.md) for details.

## Common Issues

### Hard-coded Values

**Problem:**
```tsx
<div style={{ color: '#FF5733', padding: '16px' }}>
```

**Solution:**
```tsx
import { Box } from '@chakra-ui/react';
<Box color="{colors.orange.500}" padding="4">
```

### Direct Chakra Imports

**Problem:**
```tsx
import { Button } from '@chakra-ui/react';
```

**Solution:**
```tsx
import { PMButton } from '@packmind/ui';
```

### Missing forwardRef

**Problem:**
```tsx
export const PMButton = (props) => <Button {...props} />;
```

**Solution:**
```tsx
export const PMButton = forwardRef<HTMLButtonElement, IPMButtonProps>(
  (props, ref) => <Button ref={ref} {...props} />
);
```

## Resources

- [Chakra UI Documentation](https://www.chakra-ui.com/)
- [Storybook Guide](references/storybook-guide.md)
- [Component Examples](references/component-examples.md)
- Full standard: [React Components with Chakra UI Design System](../../../.packmind/standards/react-components-with-chakra-ui-design-system.md)