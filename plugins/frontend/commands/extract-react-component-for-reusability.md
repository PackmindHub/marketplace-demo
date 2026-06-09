Identify duplicated or complex JSX blocks in React components and extract them into standalone, typed, reusable components with proper props interfaces to improve maintainability and reduce code duplication.

## When to Use

- The same JSX structure appears in multiple components with minor variations
- A component contains complex nested JSX blocks (50+ lines) that handle a specific concern
- Logic for rendering a UI section is tightly coupled with data transformation
- Component readability suffers from mixing multiple concerns in the render method
- You need to unit test a specific UI section independently from its parent component

## Context Validation Checkpoints

* [ ] What is the responsibility of the code block to be extracted (what does it render and why)?
* [ ] What data does the extracted component need from its parent (identify minimal props)?
* [ ] Are there similar patterns in other files that should use this extracted component?
* [ ] Where should the extracted component live (same domain, shared components, feature-specific)?
* [ ] What prefix or naming convention should be applied (PM-, Feature-, Section-, etc.)?

## Command Steps

### Step 1: Identify the Code Block to Extract

Locate the JSX block that is duplicated or adds complexity. Look for repeated patterns across components, self-contained rendering logic (conditional rendering, mapping, formatting), or blocks that handle a single UI concern (status display, list rendering, form sections).

```tsx
// Example: Complex status rendering in parent component
{outdatedDeployments.length > 0 && (
  <PMBox mb={4}>
    <DeploymentItem title="❌ Outdated">
      {outdatedDeployments.map((deployment) => (
        <DeploymentEntry key={deployment.id} {...deployment} />
      ))}
    </DeploymentItem>
  </PMBox>
)}
```

### Step 2: Define TypeScript Props Interface

Create an interface capturing the minimum data needed from the parent. Use descriptive names and type imports from domain packages.

```tsx
import { StandardDeploymentStatus } from '@packmind/deployments';

interface StandardStatusSectionProps {
  standardDeployment: StandardDeploymentStatus;
}
```

### Step 3: Create Component File with Proper Structure

Create a new folder with the component name and an index.ts for exports. Use PascalCase with appropriate prefix.

```bash
# Structure
ComponentName/
  ComponentName.tsx
  index.ts
```

### Step 4: Extract and Encapsulate Logic

Move the JSX and any related filtering/transformation logic into the new component. Keep all data manipulation internal to maintain component encapsulation.

```tsx
export const StandardStatusSection: React.FC<StandardStatusSectionProps> = ({
  standardDeployment,
}) => {
  const outdatedDeployments = standardDeployment.deployments.filter(
    (d) => !d.isUpToDate,
  );

  return (
    <>
      {outdatedDeployments.length > 0 && (
        <PMBox mb={4}>
          <DeploymentItem title="❌ Outdated">
            {/* ... */}
          </DeploymentItem>
        </PMBox>
      )}
    </>
  );
};
```

### Step 5: Update Parent Component Imports and Usage

Replace the extracted JSX with the new component. Import from the component's index file for clean imports.

```tsx
import { StandardStatusSection } from '../StandardStatusSection';

// In parent component render:
<StandardStatusSection standardDeployment={standardDeployment} />
```

### Step 6: Add Export to Component Index File

Ensure proper module exports for clean imports across the application.

```tsx
export * from './StandardStatusSection';
```

### Step 7: Verify and Update Similar Usage Patterns

Search the codebase for similar patterns and update them to use the new shared component for consistency and to maximize the refactoring benefits.
