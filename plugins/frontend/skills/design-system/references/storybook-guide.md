# Storybook Guide

All Design System components must be documented in Storybook for visual testing, documentation, and discoverability.

## Creating a Story

Stories live alongside components in `packages/ui/src/lib/components/`.

### Basic Story Template

```tsx
// PMButton.stories.tsx
import type { Meta, StoryObj } from '@storybook/react';
import { PMButton } from './PMButton';

const meta: Meta<typeof PMButton> = {
  title: 'Form/PMButton',
  component: PMButton,
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'secondary', 'tertiary', 'outline', 'ghost'],
    },
    disabled: {
      control: 'boolean',
    },
  },
};

export default meta;
type Story = StoryObj<typeof PMButton>;

export const Primary: Story = {
  args: {
    children: 'Primary Button',
    variant: 'primary',
  },
};

export const Secondary: Story = {
  args: {
    children: 'Secondary Button',
    variant: 'secondary',
  },
};

export const Disabled: Story = {
  args: {
    children: 'Disabled Button',
    variant: 'primary',
    disabled: true,
  },
};
```

## Story Organization

Group stories by category:

- `Form/` - Form components (PMButton, PMInput, PMSelect)
- `Typography/` - Text components (PMHeading, PMText, PMLink)
- `Layout/` - Layout components (PMBox, PMFlex, PMCard)
- `Content/` - Content components (PMTable, PMList)
- `Feedback/` - Feedback components (PMAlert, PMToast)
- `Navigation/` - Navigation components (PMTabs, PMBreadcrumb)

## Documentation

Use JSDoc comments to generate automatic documentation:

```tsx
/**
 * Primary UI button component with multiple variants
 *
 * @param variant - Visual style variant
 * @param children - Button content
 * @param disabled - Disable button interaction
 */
export const PMButton = forwardRef<HTMLButtonElement, IPMButtonProps>(
  (props, ref) => {
    // ...
  }
);
```

## Interactive Controls

Provide meaningful controls for all props:

```tsx
argTypes: {
  variant: {
    control: 'select',
    options: ['primary', 'secondary', 'tertiary'],
    description: 'Visual style of the button',
  },
  size: {
    control: 'radio',
    options: ['sm', 'md', 'lg'],
    description: 'Size of the button',
  },
  loading: {
    control: 'boolean',
    description: 'Show loading state',
  },
}
```

## Testing States

Create stories for all component states:

- Default state
- Hover state (via play function)
- Active/focused state
- Disabled state
- Loading state
- Error state
- Empty state

Example:

```tsx
export const AllStates: Story = {
  render: () => (
    <div style={{ display: 'flex', gap: '1rem', flexDirection: 'column' }}>
      <PMButton variant="primary">Default</PMButton>
      <PMButton variant="primary" disabled>Disabled</PMButton>
      <PMButton variant="primary" loading>Loading</PMButton>
    </div>
  ),
};
```

## Accessibility Testing

Include accessibility notes in story descriptions:

```tsx
export const AccessibleButton: Story = {
  args: {
    'aria-label': 'Submit form',
    children: 'Submit',
  },
  parameters: {
    docs: {
      description: {
        story: 'Example showing proper aria-label usage for accessible buttons',
      },
    },
    a11y: {
      config: {
        rules: [
          {
            id: 'color-contrast',
            enabled: true,
          },
        ],
      },
    },
  },
};
```

## Running Storybook

```bash
# Start Storybook dev server
npm run storybook

# Build static Storybook
npm run build-storybook
```

## Visual Regression Testing

Storybook stories are used for snapshot testing:

```tsx
// PMButton.spec.tsx
import { composeStories } from '@storybook/react';
import { render } from '@testing-library/react';
import * as stories from './PMButton.stories';

const { Primary, Secondary } = composeStories(stories);

describe('PMButton', () => {
  it('matches snapshot for primary variant', () => {
    const { container } = render(<Primary />);
    expect(container.firstChild).toMatchSnapshot();
  });

  it('matches snapshot for secondary variant', () => {
    const { container } = render(<Secondary />);
    expect(container.firstChild).toMatchSnapshot();
  });
});
```

## Best Practices

1. **One story per variant** - Make each variant easily discoverable
2. **Use argTypes** - Provide interactive controls for all configurable props
3. **Document edge cases** - Show error states, empty states, loading states
4. **Add descriptions** - Use JSDoc and story parameters for documentation
5. **Test accessibility** - Include a11y addon and test keyboard navigation
6. **Keep stories simple** - Focus on showcasing the component, not complex logic
7. **Use play functions** - Demonstrate interactions programmatically

## Story Checklist

When creating a new component story:

- [ ] Created `.stories.tsx` file alongside component
- [ ] Defined meta with proper title and category
- [ ] Added `autodocs` tag
- [ ] Created story for each variant
- [ ] Added argTypes with controls
- [ ] Included JSDoc documentation
- [ ] Tested accessibility with a11y addon
- [ ] Created snapshot tests using composeStories
- [ ] Documented edge cases and states
- [ ] Verified story in Storybook dev server
