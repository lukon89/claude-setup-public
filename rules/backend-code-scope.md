# Backend Code Scope

Backend service code, inline comments, and service-level technical docs must describe behaviour and intent in domain terms. Keep frontend implementation details out.

## What does not belong in backend code/docs

- UI component names, file paths, or labels (e.g. `CounterpartsStep.tsx`, the "Submit" button)
- Explanations of how a screen renders or what the operator physically sees in the UI
- Frontend-specific implementation choices (React state, CSS, routing)

## What is fine

- One-line intent statements that reference an operator decision: "operator rejected this contact", "operator clicked Unlink" — these describe the domain event, not the UI mechanics
- Linking to a ticket where the UI behaviour is specced

## Why

Embedding the exact React component path and a frontend label in the backend doc causes both to rot immediately when the UI changes, and adds no value to backend engineers reading the code.

## How to apply

When writing or reviewing backend code or service-level docs: if a sentence names a UI component, a screen label, or a frontend file — remove it or replace it with a one-sentence domain-intent statement. If the UI detail is genuinely load-bearing, it belongs in a comment inside the frontend component, not in the backend.
