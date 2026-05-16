---
applyTo: '**/components/**,**/*.tsx,**/pages/**,**/app/**,**/hooks/**'
---

# Frontend Development Standards

These rules apply when editing React components, pages, or custom hooks.

## Component Structure

Keep this order inside every component file:

1. Types / interfaces
2. Component function
3. Subcomponents (if small enough to co-locate)
4. Default export

## Composition Over Everything

Build UIs from small, focused pieces — not monolithic components:

```typescript
// WRONG — one component does layout + data + logic + rendering
export function UserDashboard() {
  // 200 lines of everything mixed together
}

// CORRECT — composed from focused pieces
export function UserDashboard() {
  return (
    <DashboardLayout>
      <UserStats userId={currentUser.id} />
      <RecentOrders userId={currentUser.id} />
    </DashboardLayout>
  )
}
```

## State Rules

- `useState` — local UI state only (open/closed, form field values)
- `useReducer` — complex state with multiple sub-values or transitions
- Derived state must NOT be stored in state — compute it:

```typescript
// WRONG — storing derived value
const [items, setItems] = useState([]);
const [count, setCount] = useState(0); // derived! always = items.length

// CORRECT — compute it
const [items, setItems] = useState([]);
const count = items.length; // derived, no state needed
```

- Never mutate state directly:

```typescript
// WRONG
items.push(newItem);
setItems(items);

// CORRECT
setItems([...items, newItem]);
```

## Data Fetching

- Prefer React Query or SWR over raw `useEffect` for data fetching
- Never fetch data in `useEffect` if you can use a library
- If you must use `useEffect`, always handle loading, error, and cleanup:

```typescript
useEffect(() => {
  let cancelled = false;
  async function load() {
    try {
      const data = await fetchData();
      if (!cancelled) setData(data);
    } catch (error) {
      if (!cancelled) setError(error);
    }
  }
  load();
  return () => {
    cancelled = true;
  };
}, [id]);
```

## Performance Rules

- Wrap expensive components with `React.memo` when they receive stable props
- Wrap expensive calculations with `useMemo`
- Wrap callbacks passed as props with `useCallback` to avoid child re-renders
- Never create objects or arrays as default prop values inline:

```typescript
// WRONG — new array on every render causes re-renders
<Component items={[]} />

// CORRECT — stable reference
const EMPTY_ITEMS: Item[] = []
<Component items={EMPTY_ITEMS} />
```

- Lists with > 100 items must use virtualization (react-window, @tanstack/virtual)

## Custom Hooks

Extract reusable stateful logic into custom hooks:

```typescript
// WRONG — logic mixed into component
export function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  useEffect(() => {
    /* fetch user */
  }, [userId]);
  // ...
}

// CORRECT — logic in a hook
function useUser(userId: string) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  useEffect(() => {
    /* fetch user */
  }, [userId]);
  return { user, loading };
}

export function UserProfile({ userId }) {
  const { user, loading } = useUser(userId);
  // component is now just rendering
}
```

## Forms

- Use a form library (React Hook Form, Formik) for non-trivial forms
- Validate with Zod schemas
- Never submit without client-side validation

## Accessibility Baseline

- Buttons must have accessible text (`aria-label` if icon-only)
- Images must have `alt` text
- Forms must have `<label>` elements linked to inputs
- Focus must be visible — never `outline: none` without a replacement
