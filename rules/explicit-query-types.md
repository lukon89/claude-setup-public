# Explicit Type Annotations on Generic Wrappers

Keep explicit type annotations on generic wrappers — even when the compiler can infer the type from the argument.

## Why

Inference is for the machine. The explicit annotation is for the human: it names the type at the call site so a reader knows the expected shape without tracing through the call chain.

Removing the annotation is a net loss:
- The code becomes harder to read cold
- The named type disappears from IDE hover
- Future refactors have no single place to see "this returns / accepts X"

## Examples across stacks

**Frontend — React Query:**
```ts
// Good — named at the call site
const query = useQuery<GetAccountsResponse>({...})

// Bad — reader must trace queryFn → api.ts → types.ts
const query = useQuery({...})
```

**Backend — Python generics:**
```python
# Good
result: list[Account] = await repo.list(...)

# Bad — reader must check what list() returns
result = await repo.list(...)
```

The annotation must be consistent with the actual type — the compiler will error if it's wrong, so there is no risk of drift.
