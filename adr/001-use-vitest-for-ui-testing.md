# ADR 001: Use Vitest for UI Testing

**Status**: Accepted  
**Date**: 2025-01-28  
**Tags**: testing, frontend, tooling

## Context

Budgettr currently lacks a testing framework for the React frontend. As the application grows, we need a reliable way to:
- Test React components and UI interactions
- Ensure TypeScript type safety in tests
- Maintain fast test execution during development
- Integrate seamlessly with our existing Vite-based build system

## Decision

We will use **Vitest** as the primary testing framework for UI testing in the React frontend.

## Rationale

### Why Vitest?

1. **Native Vite Integration**: Vitest is built by the Vite team and shares the same configuration, transformation pipeline, and resolver. This eliminates configuration duplication and ensures consistency.

2. **TypeScript Support**: First-class TypeScript support with no additional configuration needed, working seamlessly with our strict mode setup.

3. **Performance**: 
   - Uses Vite's transformation pipeline for fast test execution
   - ESM-first with instant HMR for tests
   - Parallel test execution by default

4. **Jest-Compatible API**: Familiar API for developers coming from Jest, reducing learning curve.

5. **React Testing**: Works well with React Testing Library and other React testing tools.

6. **Modern Tooling**: Built for ESM modules and modern JavaScript, aligning with our React 19 and TypeScript 5.8 setup.

### Alternatives Considered

- **Jest**: Industry standard but requires additional configuration for Vite/ESM projects, slower transformation pipeline
- **Testing Library alone**: Not a test runner, would still need Vitest or Jest
- **Playwright Component Testing**: Better for E2E but overkill for unit/component tests

## Consequences

### Positive
- Fast test execution with Vite's transformation pipeline
- Zero additional configuration for TypeScript
- Shared configuration with Vite reduces maintenance
- Modern developer experience with watch mode and UI
- Growing ecosystem and community support
- Compatible with Material-UI component testing

### Negative
- Smaller ecosystem compared to Jest (though growing rapidly)
- Team may need to learn Vitest-specific features
- Less mature than Jest (though stable for production use)

### Neutral
- Will need to add `@testing-library/react` and `@testing-library/jest-dom` as dependencies
- May need to add `jsdom` or `happy-dom` for DOM environment
- Need to establish testing patterns and conventions for the team

## Implementation Notes

Key packages to add:
- `vitest` - Test framework
- `@testing-library/react` - React component testing utilities
- `@testing-library/jest-dom` - Custom matchers for DOM testing
- `jsdom` - DOM environment for tests

Configuration in `vitest.config.ts` will extend existing `vite.config.ts`.

## References

- [Vitest Documentation](https://vitest.dev/)
- [Testing Library React](https://testing-library.com/react)
- [Vite Testing Guide](https://vitejs.dev/guide/features.html#testing)
