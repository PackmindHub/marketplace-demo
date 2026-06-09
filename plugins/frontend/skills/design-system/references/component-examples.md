# Component Examples

Real-world examples of Design System component implementations.

## Typography Components

### PMHeading

```tsx
import { PMHeading, PMText } from '@packmind/ui';

export const PageHeader = () => {
  return (
    <header>
      <PMHeading as="h1" size="3xl" marginBottom="2">
        Welcome to Packmind
      </PMHeading>
      <PMText variant="subtitle" color="{colors.text.secondary}">
        Build consistent coding standards
      </PMText>
    </header>
  );
};
```

### PMText with Links

```tsx
import { PMText, PMLink } from '@packmind/ui';

export const Documentation = () => {
  return (
    <PMText variant="body">
      Read our{' '}
      <PMLink href="/docs" variant="inline">
        documentation
      </PMLink>{' '}
      to get started.
    </PMText>
  );
};
```

## Form Components

### Complete Form Example

```tsx
import {
  PMFormContainer,
  PMLabel,
  PMInput,
  PMTextArea,
  PMButton,
  PMText,
} from '@packmind/ui';
import { useState } from 'react';

export const ContactForm = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: '',
  });

  return (
    <PMFormContainer onSubmit={(e) => e.preventDefault()}>
      <div>
        <PMLabel htmlFor="name">Name</PMLabel>
        <PMInput
          id="name"
          value={formData.name}
          onChange={(e) => setFormData({ ...formData, name: e.target.value })}
          placeholder="Enter your name"
        />
      </div>

      <div>
        <PMLabel htmlFor="email">Email</PMLabel>
        <PMInput
          id="email"
          type="email"
          value={formData.email}
          onChange={(e) => setFormData({ ...formData, email: e.target.value })}
          placeholder="you@example.com"
        />
      </div>

      <div>
        <PMLabel htmlFor="message">Message</PMLabel>
        <PMTextArea
          id="message"
          value={formData.message}
          onChange={(e) =>
            setFormData({ ...formData, message: e.target.value })
          }
          placeholder="Your message here..."
          rows={4}
        />
      </div>

      <PMButton type="submit" variant="primary">
        Send Message
      </PMButton>
    </PMFormContainer>
  );
};
```

### Select Dropdown

```tsx
import { PMLabel, PMNativeSelect } from '@packmind/ui';

export const LanguageSelector = () => {
  return (
    <div>
      <PMLabel htmlFor="language">Programming Language</PMLabel>
      <PMNativeSelect id="language" defaultValue="typescript">
        <option value="typescript">TypeScript</option>
        <option value="javascript">JavaScript</option>
        <option value="python">Python</option>
        <option value="java">Java</option>
      </PMNativeSelect>
    </div>
  );
};
```

### Button Variants

```tsx
import { PMButton } from '@packmind/ui';

export const ButtonShowcase = () => {
  return (
    <div style={{ display: 'flex', gap: '1rem', flexWrap: 'wrap' }}>
      <PMButton variant="primary" onClick={() => console.log('Primary')}>
        Primary
      </PMButton>
      <PMButton variant="secondary" onClick={() => console.log('Secondary')}>
        Secondary
      </PMButton>
      <PMButton variant="tertiary" onClick={() => console.log('Tertiary')}>
        Tertiary
      </PMButton>
      <PMButton variant="outline" onClick={() => console.log('Outline')}>
        Outline
      </PMButton>
      <PMButton variant="ghost" onClick={() => console.log('Ghost')}>
        Ghost
      </PMButton>
    </div>
  );
};
```

## Dialog/Modal

```tsx
import { PMDialog, PMButton, PMText } from '@packmind/ui';
import { useState } from 'react';

export const ConfirmationDialog = () => {
  const [open, setOpen] = useState(false);

  return (
    <>
      <PMButton variant="primary" onClick={() => setOpen(true)}>
        Delete Item
      </PMButton>

      <PMDialog.Root open={open} onOpenChange={({ open }) => setOpen(open)}>
        <PMDialog.Content>
          <PMDialog.Title>Confirm Deletion</PMDialog.Title>
          <PMDialog.Body>
            <PMText>Are you sure you want to delete this item?</PMText>
          </PMDialog.Body>
          <PMDialog.Footer>
            <PMButton variant="ghost" onClick={() => setOpen(false)}>
              Cancel
            </PMButton>
            <PMButton
              variant="primary"
              onClick={() => {
                // Handle deletion
                setOpen(false);
              }}
            >
              Delete
            </PMButton>
          </PMDialog.Footer>
        </PMDialog.Content>
      </PMDialog.Root>
    </>
  );
};
```

## Menu/Dropdown

```tsx
import { PMMenu, PMButton } from '@packmind/ui';

export const ActionsMenu = () => {
  return (
    <PMMenu.Root>
      <PMMenu.Trigger asChild>
        <PMButton variant="outline">Actions</PMButton>
      </PMMenu.Trigger>
      <PMMenu.Content>
        <PMMenu.Item value="edit" onClick={() => console.log('Edit')}>
          Edit
        </PMMenu.Item>
        <PMMenu.Item value="duplicate" onClick={() => console.log('Duplicate')}>
          Duplicate
        </PMMenu.Item>
        <PMMenu.Separator />
        <PMMenu.Item value="delete" color="red" onClick={() => console.log('Delete')}>
          Delete
        </PMMenu.Item>
      </PMMenu.Content>
    </PMMenu.Root>
  );
};
```

## Navigation - Tabs

```tsx
import { PMTabs, PMText } from '@packmind/ui';

export const SettingsTabs = () => {
  return (
    <PMTabs.Root defaultValue="general">
      <PMTabs.List>
        <PMTabs.Trigger value="general">General</PMTabs.Trigger>
        <PMTabs.Trigger value="security">Security</PMTabs.Trigger>
        <PMTabs.Trigger value="notifications">Notifications</PMTabs.Trigger>
      </PMTabs.List>

      <PMTabs.Content value="general">
        <PMText>General settings content</PMText>
      </PMTabs.Content>

      <PMTabs.Content value="security">
        <PMText>Security settings content</PMText>
      </PMTabs.Content>

      <PMTabs.Content value="notifications">
        <PMText>Notification settings content</PMText>
      </PMTabs.Content>
    </PMTabs.Root>
  );
};
```

## Content - Table

```tsx
import { PMTable, PMText } from '@packmind/ui';

interface User {
  id: string;
  name: string;
  email: string;
  role: string;
}

export const UserTable = ({ users }: { users: User[] }) => {
  return (
    <PMTable.Root>
      <PMTable.Header>
        <PMTable.Row>
          <PMTable.ColumnHeader>Name</PMTable.ColumnHeader>
          <PMTable.ColumnHeader>Email</PMTable.ColumnHeader>
          <PMTable.ColumnHeader>Role</PMTable.ColumnHeader>
        </PMTable.Row>
      </PMTable.Header>
      <PMTable.Body>
        {users.map((user) => (
          <PMTable.Row key={user.id}>
            <PMTable.Cell>
              <PMText variant="body">{user.name}</PMText>
            </PMTable.Cell>
            <PMTable.Cell>
              <PMText variant="body">{user.email}</PMText>
            </PMTable.Cell>
            <PMTable.Cell>
              <PMText variant="body">{user.role}</PMText>
            </PMTable.Cell>
          </PMTable.Row>
        ))}
      </PMTable.Body>
    </PMTable.Root>
  );
};
```

## Layout Components with Chakra Primitives

### Card Layout

```tsx
import { Box, Flex } from '@chakra-ui/react';
import { PMHeading, PMText, PMButton } from '@packmind/ui';

export const PMCard = ({ title, description, onAction }) => {
  return (
    <Box
      bg="{colors.background.primary}"
      borderRadius="md"
      padding="6"
      boxShadow="sm"
      _hover={{ boxShadow: 'md' }}
    >
      <PMHeading as="h3" size="lg" marginBottom="2">
        {title}
      </PMHeading>
      <PMText variant="body" color="{colors.text.secondary}" marginBottom="4">
        {description}
      </PMText>
      <PMButton variant="primary" onClick={onAction}>
        Learn More
      </PMButton>
    </Box>
  );
};
```

### Responsive Grid

```tsx
import { Box } from '@chakra-ui/react';

export const ResponsiveGrid = ({ children }) => {
  return (
    <Box
      display="grid"
      gridTemplateColumns={{
        base: '1fr',
        md: 'repeat(2, 1fr)',
        lg: 'repeat(3, 1fr)',
      }}
      gap="6"
    >
      {children}
    </Box>
  );
};
```

### Flex Container

```tsx
import { Flex } from '@chakra-ui/react';
import { PMButton } from '@packmind/ui';

export const ActionBar = () => {
  return (
    <Flex
      justifyContent="space-between"
      alignItems="center"
      padding="4"
      bg="{colors.background.primary}"
      borderBottom="1px solid {colors.beige.200}"
    >
      <Flex gap="2">
        <PMButton variant="primary">Save</PMButton>
        <PMButton variant="outline">Cancel</PMButton>
      </Flex>
      <PMButton variant="ghost">Reset</PMButton>
    </Flex>
  );
};
```

## Accessibility Patterns

### Icon Button with Label

```tsx
import { PMButton } from '@packmind/ui';
import { TrashIcon } from '@heroicons/react/24/outline';

export const DeleteButton = ({ onDelete }) => {
  return (
    <PMButton
      variant="ghost"
      onClick={onDelete}
      aria-label="Delete item"
      title="Delete item"
    >
      <TrashIcon width={20} height={20} />
    </PMButton>
  );
};
```

### Form with Validation

```tsx
import { PMInput, PMLabel, PMText } from '@packmind/ui';
import { useState } from 'react';

export const EmailInput = () => {
  const [email, setEmail] = useState('');
  const [error, setError] = useState('');

  const validateEmail = (value: string) => {
    if (!value.includes('@')) {
      setError('Please enter a valid email address');
    } else {
      setError('');
    }
  };

  return (
    <div>
      <PMLabel htmlFor="email">Email</PMLabel>
      <PMInput
        id="email"
        type="email"
        value={email}
        onChange={(e) => {
          setEmail(e.target.value);
          validateEmail(e.target.value);
        }}
        aria-invalid={!!error}
        aria-describedby={error ? 'email-error' : undefined}
      />
      {error && (
        <PMText id="email-error" color="{colors.red.500}" fontSize="sm">
          {error}
        </PMText>
      )}
    </div>
  );
};
```

## Composition Patterns

### Feature Card with Multiple Components

```tsx
import { Box, Flex } from '@chakra-ui/react';
import {
  PMHeading,
  PMText,
  PMButton,
  PMMenu,
  PMDialog,
} from '@packmind/ui';
import { useState } from 'react';

export const StandardCard = ({ standard, onEdit, onDelete }) => {
  const [deleteOpen, setDeleteOpen] = useState(false);

  return (
    <Box
      bg="{colors.background.primary}"
      borderRadius="md"
      padding="6"
      boxShadow="sm"
    >
      <Flex justifyContent="space-between" alignItems="start" marginBottom="4">
        <PMHeading as="h3" size="lg">
          {standard.name}
        </PMHeading>
        <PMMenu.Root>
          <PMMenu.Trigger asChild>
            <PMButton variant="ghost" size="sm">
              •••
            </PMButton>
          </PMMenu.Trigger>
          <PMMenu.Content>
            <PMMenu.Item value="edit" onClick={onEdit}>
              Edit
            </PMMenu.Item>
            <PMMenu.Item value="delete" onClick={() => setDeleteOpen(true)}>
              Delete
            </PMMenu.Item>
          </PMMenu.Content>
        </PMMenu.Root>
      </Flex>

      <PMText variant="body" color="{colors.text.secondary}">
        {standard.description}
      </PMText>

      <PMDialog.Root
        open={deleteOpen}
        onOpenChange={({ open }) => setDeleteOpen(open)}
      >
        <PMDialog.Content>
          <PMDialog.Title>Delete Standard</PMDialog.Title>
          <PMDialog.Body>
            <PMText>Are you sure you want to delete "{standard.name}"?</PMText>
          </PMDialog.Body>
          <PMDialog.Footer>
            <PMButton variant="ghost" onClick={() => setDeleteOpen(false)}>
              Cancel
            </PMButton>
            <PMButton
              variant="primary"
              onClick={() => {
                onDelete();
                setDeleteOpen(false);
              }}
            >
              Delete
            </PMButton>
          </PMDialog.Footer>
        </PMDialog.Content>
      </PMDialog.Root>
    </Box>
  );
};
```
