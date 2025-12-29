# Software Design Patterns

**Scope**: Code-level design patterns, principles, and best practices for writing maintainable, testable code.

**Purpose**: Use this when designing classes, modules, and code structure. For system-level architectural patterns, see [Software Architecture](architecture.md).

## Table of Contents

- [1. Design Principles](#1-design-principles)
- [2. Programming Paradigms](#2-programming-paradigms)
- [3. Domain-Driven Design (Tactical)](#3-domain-driven-design-tactical)
- [4. Design Patterns](#4-design-patterns)
- [5. Testing Strategies](#5-testing-strategies)

---

## 1. Design Principles

### SOLID Principles

- **Single Responsibility**: Each class/module has one reason to change
- **Open/Closed**: Open for extension, closed for modification
- **Liskov Substitution**: Subtypes must be substitutable for base types
- **Interface Segregation**: Many specific interfaces > one general interface
- **Dependency Inversion**: Depend on abstractions, not concretions

### Design Principles

- **Separation of Concerns**: Business logic, data access, presentation separate
- **DRY (Don't Repeat Yourself)**: Single source of truth for logic/data
- **KISS (Keep It Simple, Stupid)**: Prefer simple solutions over complex
- **YAGNI (You Aren't Gonna Need It)**: Don't build for hypothetical future needs
- **Fail Fast**: Detect errors early, fail immediately with clear messages

---

## 2. Programming Paradigms

### Object-Oriented Programming (OOP)

- **Core Principles**:
  - **Encapsulation**: Bundle data and methods together; hide internal state
  - **Inheritance**: Reuse code through class hierarchies
  - **Polymorphism**: Same interface, different implementations
  - **Abstraction**: Hide complexity, expose essential features
- **Example**: `class Order { private items; addItem(); calculateTotal(); }`
- **When to use**: Complex domain models, stateful systems, GUI applications
- **Languages**: Java, C#, Python, TypeScript (class-based)

### Functional Programming (FP)

- **Core Principles**:
  - **Immutability**: Data doesn't change; create new data structures
  - **Pure Functions**: No side effects; same input → same output
  - **First-Class Functions**: Functions as values (pass, return, assign)
  - **Higher-Order Functions**: Functions that take/return functions (map, filter, reduce)
- **Example**: `const total = items.map(x => x.price).reduce((a, b) => a + b, 0)`
- **When to use**: Data transformations, concurrent programming, mathematical computations
- **Languages**: Haskell, Clojure, F#, Elixir (pure FP); JavaScript, Python, Scala (support FP)

### Hybrid Approaches

- **OOP + FP**: Use OOP for structure, FP for transformations
  - **Example**: Classes for domain models, functional methods for data processing
  - **Languages**: Scala, Kotlin, Python, TypeScript, C# (modern features)
- **Best of Both**: Encapsulation + immutability, classes + higher-order functions
- **When to use**: Modern applications benefit from both paradigms

### Language Characteristics

- **Compiled vs Interpreted**:
  - **Compiled** (C, C++, Go, Rust): Faster execution, type checking at compile time, harder to debug
  - **Interpreted** (Python, JavaScript, Ruby): Easier development, slower execution, dynamic typing
  - **Hybrid** (Java, C#, TypeScript): Compile to bytecode/JS, then interpret/JIT compile
- **Level of Abstraction**:
  - **Low-level** (C, Rust): Direct memory management, hardware control, high performance
  - **High-level** (Python, JavaScript): Built-in data structures, garbage collection, rapid development
  - **Mid-level** (Java, C#, Go): Balance of control and productivity
- **Type Systems**:
  - **Static typing** (Java, C#, TypeScript): Type checking at compile time, fewer runtime errors
  - **Dynamic typing** (Python, JavaScript, Ruby): Flexibility, faster prototyping
  - **Gradual typing** (TypeScript, Python type hints): Optional static typing

### Choosing a Paradigm/Language

- **Domain complexity**: OOP for rich domain models, FP for data pipelines
- **Performance requirements**: Compiled languages for performance-critical code
- **Team expertise**: Choose languages team knows; consider learning curve
- **Ecosystem**: Libraries, frameworks, tooling available
- **Type safety**: Static typing for large codebases, dynamic for rapid prototyping

---

## 3. Domain-Driven Design (Tactical)

- **Entities**: Objects with identity (User, Order)
- **Value Objects**: Immutable objects defined by attributes (Money, Address)
- **Aggregates**: Cluster of entities treated as single unit (Order + OrderItems)
- **Aggregate Root**: Single entry point to aggregate (Order)
- **Domain Events**: Something that happened in the domain (OrderPlaced, PaymentReceived)
- **Repositories**: Abstraction for aggregate persistence
- **Domain Services**: Operations that don't belong to single entity
- **Factories**: Encapsulate complex object creation

> **Note**: For DDD strategic design (bounded contexts, context mapping), see [Software Architecture](architecture.md).

---

## 4. Design Patterns

### Creational Patterns

- **Factory Pattern**: Centralize object creation logic
- **Builder Pattern**: Construct complex objects step by step
- **Singleton Pattern**: Single instance (use sparingly, prefer dependency injection)

### Structural Patterns

- **Adapter Pattern**: Convert interface of one class to another
- **Decorator Pattern**: Add behavior to objects dynamically
- **Facade Pattern**: Simplified interface to complex subsystem

### Behavioral Patterns

- **Strategy Pattern**: Encapsulate algorithms, make them interchangeable
- **Observer Pattern**: Notify dependents of state changes
- **Command Pattern**: Encapsulate requests as objects
- **Specification Pattern**: Encapsulate business rules as reusable predicates

---

## 5. Testing Strategies

### Testing Pyramid

- **Unit Tests**: Fast, isolated, test single functions/classes
- **Integration Tests**: Test interactions between components
- **Contract Tests**: Verify API contracts between services
- **End-to-End Tests**: Test complete user workflows (few, slow)

### Testing Patterns

- **Test-Driven Development (TDD)**: Write tests before code
- **Behavior-Driven Development (BDD)**: Tests in natural language
- **Mocking**: Replace dependencies with test doubles
- **Test Fixtures**: Reusable test data setup
- **Property-Based Testing**: Generate test cases automatically

### Test Types

- **Unit Tests**: Fast, test business logic in isolation
- **Integration Tests**: Test database, external APIs, message queues
- **Contract Tests**: Verify API contracts (Pact, Spring Cloud Contract)
- **Chaos Engineering**: Test system resilience to failures

---

> **Note**: For system-level architectural patterns, see [Software Architecture](architecture.md). For cloud-specific implementations, see [Cloud Architecture](../cloud/architecture.md).

