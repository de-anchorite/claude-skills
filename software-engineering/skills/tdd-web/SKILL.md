---
name: tdd-web
description: "Use when building React components, hooks, pages, or features; working with Vite, Tailwind, DaisyUI, TanStack Query, Storybook, or MSW; writing frontend tests with Vitest; or asked to build a web app, UI, or frontend feature."
---

# Web App Design (React/TypeScript)

Preferred patterns and conventions for building React web applications. Apply proactively to all frontend work.

## Trigger conditions

Apply this skill when:
- Creating components or setting up routing
- Fetching data or managing state
- Adding authentication
- Building forms, charts, or dashboards
- Writing Storybook stories
- Styling with Tailwind or CSS

## Stack

| Concern | Choice |
|---|---|
| Framework | React + Vite |
| Styling | Tailwind CSS + DaisyUI; modern CSS — no SASS |
| Data fetching | TanStack Query |
| Routing | React Router |
| Auth | Auth0 Lock |
| Charts | Chart.js |
| Component development | Storybook + MSW |
| Tests | Vitest (logic only) |

## Project structure

```
src/
├── components/   # Shared reusable UI
├── features/     # Self-contained slices (component + hook + story)
├── hooks/        # Shared custom hooks
├── providers/    # Context providers
├── pages/        # Thin route-level components
└── api/          # API client functions
```

## Components

Self-contained — fetch their own data, avoid prop drilling. No Redux.

```tsx
const UserCard = ({ userId }: { userId: string }) => {
  const { data: user, isLoading } = useUser(userId);
  if (isLoading) return <UserCardSkeleton />;
  return <div className="card">{user?.name}</div>;
};
```

Use TanStack Query for server state, `useState`/`useReducer` for local state, Context for genuinely shared state. Keep providers focused — no mega-provider.

## Styling

Tailwind for layout. DaisyUI for UI elements. CSS files for complex or repetitive Tailwind. No SASS. Never pass presentation values as props.

```tsx
// Prefer
<button className="btn btn-primary">Save</button>
// Avoid
<Button color="primary" size="lg" rounded />
```

## Data fetching

TanStack Query for all server state. Custom hooks per feature. Native `fetch` — not Axios. Always show a skeleton while loading — never blank space or a spinner mid-content.

```ts
export const useUsers = (accountId: string) =>
  useQuery({
    queryKey: ['users', accountId],
    queryFn: () => api.users.getByAccount(accountId),
  });
```

## Routing

React Router. Pages are thin — read params, delegate to feature components.

## Authentication

Auth0 Lock. `AuthProvider` + `useAuth`. Keep auth out of feature components — pass user ID or account ID in, don't reach into auth directly.

## Storybook

Primary way to develop and review UI. Every component gets a story. Use MSW to mock API calls. Cover: loading, loaded, empty, error. Stories live alongside the component.

```tsx
export const Loaded: Story = {
  parameters: {
    msw: { handlers: [http.get('/api/users', () => HttpResponse.json(mockUsers))] }
  }
};

export const Loading: Story = {
  parameters: {
    msw: { handlers: [http.get('/api/users', async () => { await delay('infinite'); })] }
  }
};
```

## Tests

Logic only — utility functions, data transforms, custom hook logic, validation. Not for UI rendering. Use Vitest.

## Working style

Keep components small. Split when complex — don't add props. Prefer composition over configuration.

## Planning and workflow

**REQUIRED:** Follow the `development-workflow` skill for planning, per-task review, and git commits. Use the `web-` prefix for plan files (e.g. `.claude/plans/web-<feature-name>.md`). Each deliverable unit (component, hook, page) is one unit of work.
