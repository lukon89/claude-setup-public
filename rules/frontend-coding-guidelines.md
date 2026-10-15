# Frontend Coding Guidelines

A living collection of frontend coding patterns. Add new entries as patterns emerge.

---

## Use named helper functions over inline computations

When a named helper already exists for a computation, use it — don't reimplement the same logic inline.

**Wrong:**
```tsx
case 'NEXT': {
  const order = computeStepOrder(state.enabledScopes)
  return {
    ...state,
    step: order[order.indexOf(state.step) + 1] ?? state.step,
  }
}
```

**Right:**
```tsx
case 'NEXT':
  return {
    ...state,
    step: computeNextStep(state.step, state.enabledScopes) ?? state.step,
  }
```

**Why:** Inline reimplementations diverge from the helper silently when the helper is updated.

---

## Place JSDoc comments next to the default value, not the type declaration

**Wrong:**
```tsx
interface Props {
  /** Defaults to FULL_SCOPES so callers that haven't been migrated behave as before. */
  enabledScopes?: Scope[]
}
```

**Right:**
```tsx
interface Props {
  enabledScopes?: Scope[]
}
const MyComponent = ({
  /** Defaults to FULL_SCOPES so callers that haven't been migrated behave as before. */
  enabledScopes = [...FULL_SCOPES],
}: Props) => { ... }
```

**Why:** The default value is where the behaviour lives — the comment belongs next to the code it describes.

---

## Explicit type annotations on generic wrappers

Keep explicit type annotations on generic wrappers even when the compiler infers the type.

**Wrong:**
```ts
const query = useQuery({ queryFn: () => fetchAccounts() })
```

**Right:**
```ts
const query = useQuery<GetAccountsResponse>({ queryFn: () => fetchAccounts() })
```

**Why:** The annotation names the expected shape at the call site for human readers.

---

## Named constants over inline magic values

Extract any inline array or string literal that has a meaningful domain name into a module-level constant.

```ts
// module level
export const FULL_SCOPES: readonly Scope[] = ['bank_feeds', 'finance_automation']
```

---

## Test files: use accessible role-based selectors, not testID

Prefer `getByRole`, `getByLabelText`, `getByText` over `getByTestId`. Use `userEvent` over `fireEvent`.

**Why:** Role-based selectors test from the user's perspective and survive implementation refactors.
