# ADR 002: Use Domain-Driven Design for Application Domain

**Status**: Accepted  
**Date**: 2025-01-28  
**Tags**: architecture, domain-modeling, backend

## Context

Budgettr is a personal finance application that deals with complex domain concepts like budgets, expenses, income, transactions, categories, and financial calculations. As the application grows, we need a structured approach to:
- Model the personal finance domain accurately
- Maintain a clear separation between business logic and technical concerns
- Create a ubiquitous language shared between developers and domain experts
- Ensure the codebase remains maintainable as complexity increases
- Support future features like budgeting rules, forecasting, and reports

## Decision

We will adopt **Domain-Driven Design (DDD)** principles to model and structure the personal finance domain in Budgettr.

## Rationale

### Why DDD?

1. **Rich Domain Model**: Personal finance has clear domain concepts (Budget, Expense, Income, Transaction, Category, Account) that benefit from explicit modeling.

2. **Complex Business Rules**: Financial calculations, budget constraints, categorization rules, and date-based logic are better expressed as domain logic rather than scattered technical code.

3. **Ubiquitous Language**: DDD encourages creating a shared vocabulary between developers and users, making the codebase more understandable and aligned with user mental models.

4. **Strategic Design**: Bounded contexts help us isolate different aspects of finance management (budgeting, tracking, reporting) as the application grows.

5. **Rust's Type System**: Rust's strong type system and algebraic data types align well with DDD's emphasis on making invalid states unrepresentable.

6. **Tauri Architecture**: The clear frontend/backend separation in Tauri naturally supports DDD's layered architecture.

### DDD Concepts to Apply

**Tactical Patterns**:
- **Entities**: Objects with unique identity (Budget, Account, Transaction)
- **Value Objects**: Immutable objects defined by their attributes (Money, DateRange, CategoryType)
- **Aggregates**: Consistency boundaries (Budget as aggregate root with BudgetItems)
- **Domain Services**: Operations that don't naturally belong to an entity
- **Repositories**: Abstract data access for aggregates
- **Domain Events**: Capture significant domain occurrences (BudgetExceeded, TransactionCreated)

**Strategic Patterns**:
- **Bounded Contexts**: Separate contexts for budgeting, tracking, reporting
- **Context Mapping**: Define relationships between contexts as the app grows

### Alternatives Considered

- **Anemic Domain Model**: Simple data structures with logic in services - easier initially but harder to maintain as complexity grows
- **Transaction Script**: Procedural approach - simpler but loses domain expressiveness
- **CRUD-based**: Direct mapping to database - insufficient for complex business rules

## Consequences

### Positive
- Clear domain model that reflects real-world personal finance concepts
- Business logic centralized in domain layer, easier to test and maintain
- Strong type safety prevents invalid financial states
- Easier to reason about complex budget rules and calculations
- Better separation of concerns between UI, domain, and infrastructure
- Domain model can evolve independently of Tauri/React technical concerns
- Natural fit with Rust's type system (enums for state machines, Result for operations)

### Negative
- Steeper learning curve for developers unfamiliar with DDD
- More upfront design effort required
- Can lead to over-engineering for simple CRUD operations
- Need to avoid over-abstraction in early stages

### Neutral
- Domain logic will primarily live in Rust (src-tauri/src/domain/)
- Frontend (React) will work with DTOs and call domain operations via Tauri commands
- Need to establish patterns for serializing domain objects across Tauri IPC boundary
- May introduce domain events that need coordination with frontend state

## Implementation Notes

### Project Structure
```
src-tauri/src/
├── domain/           # Domain layer (entities, value objects, services)
│   ├── budget/
│   ├── transaction/
│   ├── account/
│   └── shared/       # Shared value objects (Money, DateRange)
├── app/      # Application services (use cases)
├── infra/   # Technical concerns (persistence, external APIs)
    └── commands/         # Tauri command handlers (thin layer)
```

### Key Guidelines
- Keep domain logic pure Rust with no Tauri dependencies
- Use rich domain types (e.g., `Money` instead of `f64`, `BudgetStatus` enum)
- Make invalid states unrepresentable through types
- Domain entities should validate their own invariants
- Use Result types for operations that can fail with domain errors
- DTOs for crossing Tauri IPC boundary (serialize domain models when needed)

### Example Domain Concepts
- `Money`: Value object with amount and currency
- `Budget`: Aggregate root with identity, name, period, and budget items
- `Transaction`: Entity representing a financial transaction
- `Category`: Value object or entity for expense/income categorization
- `BudgetPeriod`: Value object for date ranges (monthly, yearly, custom)

## References

- [Domain-Driven Design by Eric Evans](https://www.domainlanguage.com/ddd/)
- [Implementing Domain-Driven Design by Vaughn Vernon](https://vaughnvernon.com/)
- [Domain Modeling Made Functional (F# but applicable to Rust)](https://pragprog.com/titles/swdddf/domain-modeling-made-functional/)
- [Rust DDD Example](https://github.com/vadinamo/ddd-rust-example)
