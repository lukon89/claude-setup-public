# Coding Guidelines

A living collection of coding guidelines. Add new entries as patterns emerge and are confirmed twice (see rule-factory.md).

---

## Named constants over inline magic values

Extract any inline list, string, or numeric literal that has a meaningful domain name into a module-level named constant — even when the value is used in only one place.

**Wrong:**
```python
scope_parts = ["openid", "profile", "email", "offline_access"]
if bank_feeds_enabled:
    scope_parts.append("bankfeeds")
```

**Right:**
```python
_COMMON_SCOPES = ["openid", "profile", "email", "offline_access"]
_BANK_FEEDS_SCOPES = ["bankfeeds"]

scope_parts = list(_COMMON_SCOPES)
if bank_feeds_enabled:
    scope_parts.extend(_BANK_FEEDS_SCOPES)
```

**Why:** Inline literals hide intent, scatter the definition, and make future changes error-prone. A named constant makes the group's purpose explicit and gives a single place to update.

**How to apply:** Any time a list or string literal appears inline in a method body and represents a named concept (scope group, status set, error code list, allowed values) — extract it to a module-level constant before committing.
