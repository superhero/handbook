# Core Architecture Principles

- [Philosophy](#philosophy)
- [Layered Architecture](#layered-architecture)
- [Standard Operating Procedures](#standard-operating-procedures)

## Philosophy

The architectural approach described here is inspired by, and aligns with, other already well-defined architectural principles where **core business** logic is isolated at the center of the solution. Contextual boundaries are protected by relying on adapted implementations of contracted interfaces.

- **Logic should not have concrete dependencies:** Dependency Inversion Principle.
- **Logic should not depend on what it does not use:** Interface Segregation Principle.
- **Logic should be adaptable to different solutions:** Inversion of control improves agility.

---

### Component Isolation & Dependency Direction

These standards advocate a **Component Isolation Principle**, meaning a component must remain isolated from the problem spaces of other components, and should not address problems beyond its contextual scope.

- Components benefit from defined boundaries that contextualizes responsibilities.
- Components benefit from an isolated scope to remain stable by depending on adapters where changes in coupled components are addressed.

---

![Component Isolation](/diagrams/component-isolation.svg)

---

The adapter-pattern described above ensures **component isolation**, reducing **tight coupling** by abstracting the dependency, using a contracted interface that is implemented by one or more adapters of different solutions to the contracted problem.

The adapter-pattern described implements the **Dependency Inversion Principle** described by Robert C. Martin - _High-level modules should not depend on low-level modules. Both should depend on abstractions._ A principle that confronts **tight coupling**.

**Tight coupling** occurs when components are strongly dependent on each other's internal structure or behavior. A change in one component forces changes in others - causing instability and a maintenance overhead.

## Layered Architecture

A layered architecture organizes a system into hierarchical boundaries, each with distinct roles and responsibilities.

- **Separation of concerns:** Each layer is a boundary that addresses specific responsibilities.
- **Direction of dependencies:** Higher layers depend only on lower layers, never vice versa.
- **Dependency inversion:** Lower layers define interfaces, implemented by the higher layers.

---

- [Model-View-Controller (MVC)](#model-view-controller-mvc)
- [Onion Architecture](#onion-architecture)
- [Hexagonal Architecture (Ports & Adapters)](#hexagonal-architecture-ports--adapters)
- [Domain-Driven Design (DDD)](#domain-driven-design-ddd)
- [Command Query Responsibility Segregation (CQRS)](#command-query-responsibility-segregation-cqrs)

---

### Model-View-Controller (MVC)

> Separates responsibilities between business logic, user interface, and input management!

The **MVC** pattern structures three distinct component: **Model**, **View**, and **Controller**. This separation defines a basic understanding of boundaries between different layers. It's a basic pattern that further aligns well with the deeper architectual designs referenced on this page.

- **Model:** Manages business logic and data state.
- **View:** Presents data and interfaces to the user.
- **Controller:** Processes user input and coordinates changes between the Model and View.

#### Learn More

- [**Martin Fowler:** Model View Controller](https://martinfowler.com/eaaCatalog/modelViewController.html)
- [**Wikipedia:** Model-view-controller](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)

---

### Onion Architecture

> Onion Architecture defines a layered system with core domain logic in the center!

In **Onion Architecture**, the core domain model is at the center, surrounded by layers such as **Infrastructure**, **API**, and **View**, creating clear boundaries and a defined direction for dependencies; outer layers may depend inward, but never outward.

- Decouples business logic from infrastructure specifics.
- Each layer evolves independently.
- Changes in external systems has less impact on core behavior.

#### Learn More

- [**Jeffrey Palermo:** The Onion Architecture : Part 1](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/)
- [**Jeffrey Palermo:** The Onion Architecture : Part 2](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-2/)
- [**Jeffrey Palermo:** The Onion Architecture : Part 3](https://jeffreypalermo.com/2008/08/the-onion-architecture-part-3/)
- [**Jeffrey Palermo:** The Onion Architecture : Part 4 - After Four Years](https://jeffreypalermo.com/2013/08/onion-architecture-part-4-after-four-years/)
- [**YouTube:** Onion Architecture | Programming with Palermo (Clear Measure)](https://www.youtube.com/watch?v=nFU-L7QdS78)

---

### Hexagonal Architecture (Ports & Adapters)

> Improves the ability to adapt and test the solution in isolation from the deployment context!

Inspired by **Hexagonal Architecture**, the system uses **adapters** for external interactions at upstream (input) and downstream (output) boundaries. The application layer defines **ports** (interfaces), and external infrastructural interactions are implemented via adapters (**plugs**).

Adapters commonly implement:

- Gateways for APIs or protocols.
- Repositories interacting with data layers.
- Controllers and middleware.

#### Learn More

- [**Alistair Cockburn:** Hexagonal architecture the original 2005 article](https://alistair.cockburn.us/hexagonal-architecture)
- [**Alistair Cockburn:** Hexagonal Architecture (Ports & Adapters) The 2023 version (PDF)](https://alistaircockburn.com/Hexagonal%20Budapest%2023-05-18.pdf)
- [**Alistair Cockburn:** How the Ports & Adapters architecture simplifies your life (PDF)](https://alistaircockburn.com/hexarch%20v1.1b%20DIFFS%2020250420-1012%20paper+epub.docx.pdf)
- [**Wikipedia:** Hexagonal architecture (software)](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software))
- [**YouTube:** Hexagonal Architecture (Alistair Cockburn) (Tech Excellence)](https://www.youtube.com/watch?v=k0ykTxw7s0Y)
- [**YouTube:** Hexagonal Architecture with Alistair Cockburn (Miami Java User Group)](https://www.youtube.com/watch?v=V63A-xjAeoo)

---

### Domain-Driven Design (DDD)

> Solutions should reflect the language and needs of stakeholders to the solution!

The architectural principles described originate from **Domain-Driven Design**, which organizes software around business concepts, using a **ubiquitous-language** developed with **domain experts** to ensure clarity and reduce miscommunication.

- The **domain experts** are one or many, with practical experience, preferably a stakeholder in the solution.
- The **ubiquitous-language** is derived from the domain and the **domain experts**, not invented by engineers.

#### Learn More

- [**Martin Fowler:** Bounded Context](https://martinfowler.com/bliki/BoundedContext.html)
- [**Martin Fowler:** DDD Aggregate](https://martinfowler.com/bliki/DDD_Aggregate.html)
- [**Martin Fowler:** Domain-driven design](https://martinfowler.com/bliki/DomainDrivenDesign.html)
- [**Martin Fowler:** Evans Classification](https://martinfowler.com/bliki/EvansClassification.html)
- [**Martin Fowler:** Value Object](https://martinfowler.com/bliki/ValueObject.html)
- [**Wikipedia:** Domain-driven design](https://en.wikipedia.org/wiki/Domain-driven_design)
- [**YouTube:** Bounded Contexts - Eric Evans - DDD Europe 2020 (Domain-Driven Design Europe)](https://www.youtube.com/watch?v=am-HXycfalo)
- [**YouTube:** Eric Evans - Tackling Complexity in the Heart of Software (Domain-Driven Design Europe)](https://www.youtube.com/watch?v=dnUFEg68ESM)
- [**YouTube:** What is DDD - Eric Evans - DDD Europe 2019 (Domain-Driven Design Europe)](https://www.youtube.com/watch?v=pMuiVlnGqjk)

---

#### Core, Supporting, & Generic Domains

A domain addresses a specific business problem and can be decomposed into more focused **subdomains** that contextualize problems that are more or less relevant to what is important. Described by the **Domain-Driven Design** doctrine, a domain space can be differentiated as follow:

- **Core Domain:** The business-critical part of the system. High-value logic important to the business.
- **Supporting Subdomain:** Enables core functionality, _e.g., logging, auditing, etc_.
- **Generic Subdomain:** Replaceable third-party functionality, _e.g., services, libraries, etc_.

Clearly defined boundaries (**bounded-contexts**) clarify the separation of concerns and guide component design.

---

#### Domain Boundary & Microservices

A **microservice** in this context hosts a specific domain responsibility, named semantically to reflect the domain solution it provides.

---

#### Bounded-Context

A **bounded-context** encapsulates a consistent domain model, language, and behavior, typically centered around an **aggregate root**, but may also align with business capabilities or departments.

---

#### Aggregate

An **aggregate** organizes domain **entities** and **value objects** hierarchically around an **aggregate root**, enforcing domain policies and consistency to preserve integrity.

The **aggregate root** is an entity that defines the **aggregate** context that external layers always should use when altering the state of the data strcuture.

- **Controls access to modification:** Changes to the state must go through the aggregate.
- **Ensures that all modifications respect business policies:** Enforces domain rules to data mutation.
- **Defines the boundaries for persistence and concurrency:** Loaded and persisted as a consistent, transactional unit - all or nothing.

_**OBS!** An aggregate should not know of, or reference another aggregate. Modifying two aggregates at once violates the **Single Transactional Boundary Rule**. Aggregates are meant to be modified independently._

---

#### Entity

An **Entity** is an object that has a distinct identity. **Example:** a user or sales-order in a system is identified by a unique ID and can be identified even if their attributes changes.

---

#### Value Object

A **Value Object** represents a descriptive aspect of the domain with no conceptual identity; it is defined only by its imutable value.

---

#### Repository

A **Repository** abstracts the **data layer**, acting as the **Anti-Corruption Layer** between the **domain** model and **downstream infrastructure**.

- **Read model:** Returns a fully instantiated **Aggregate**, not standalone **Entities** or **Value Objects**.
- **Write model:** Accepts an **Aggregate** instance and is responsible for persisting it atomically as a consistent transaction.

---

### Command Query Responsibility Segregation (CQRS)

> Separate the read and write models!

Inspired by a "command query separation" principle, this page promotes a model design that separation between read and write:

- **Commands:** Trigger state changes and emit events, _e.g., the write model_.
- **Queries:** Optimized projections for reading, _e.g., the read model_.

#### Learn More

- [**Martin Fowler:** Command Query Separation](https://www.martinfowler.com/bliki/CommandQuerySeparation.html)
- [**Martin Fowler:** CQRS](https://martinfowler.com/bliki/CQRS.html)
- [**YouTube:** Greg Young - A Decade of DDD, CQRS, Event Sourcing (Domain-Driven Design Europe)](https://www.youtube.com/watch?v=LDW0QWie21s)

## Standard Operating Procedures

These **Standard Operating Procedures (SOP)** are detailed, documented instructions that define how to implement the specified architecture. They serve as a blueprint for processes, workflows and strategies.

- [Contextual Boundaries](#contextual-boundaries)
- [Anti-Corruption Layer & Context Mapping](#anti-corruption-layer--context-mapping)
- [Source Code File Structure](#source-code-file-structure)
- [Event-Log & Projection](#event-log--projection)

---

### Contextual Boundaries

Software boundaries separate domain responsibilities into bounded-contexts, each structured into layers: **infrastructure** (I/O implementations), **application** (orchestration), and **domain** (business logic).

---

![Contextual Boundaries](/diagrams/contextual-boundaries.png)

---

_Interaction lines in the architecture express dependency and implementation strategies that show how, and in what direction - boundaries are expected to be broken._

- **Infrastructure:** Layer where gateways, controllers and repositories are implemented.
- **Application:** Layer responsible for coordinates of different infrastructural implementations.
- **Domain:** Layer where the domain-specific business logic and data structures are implemented.
- **Command:** Implements the write model that is responsible for persistence modifications in the data layer.
- **Query:** Implements the read model that is responsible for projections of the data layer.
- **Gateway:** A technical port that expresses a pluggable cross boundary interaction.
- **Controller:** Upstream adapter implementations responsible for facading inbound gateways.
- **Repository:** Downstream data layer facade responsible for outbound implementations that persists and reads aggregated data structures.
- **Adapter:** Adapts infrastructural components to domain-specific expectations.
- **Interactor:** Transactional application service that coordinates application behaviour.
- **Process Manager:** Application service responsible for process coordination.
- **Contract:** Interfaces that express what implementations the domain requires to be able to fulfill the domain-specific business logic.
- **Aggregate:** Implements the domain-specific atomic business logic that guarantees business policies within the contextual boundary.
- **Schema:** Useful to help ensure data integrity of the data representations it specify.
- **Entity:** A persistent domain object with a unique identity, governed by its aggregate root.
- **Value Object:** A domain object without identity, defined by the value of its attribute(s), often typically used to represent attributes of an Entity.

---

### Anti-Corruption Layer & Context Mapping

Interactions between contexts use an **Anti-Corruption Layer** implementing context mappers to ensure data integrity, translating external representations into domain-specific structures, _e.g., `Entities` and `Value Objects`_.

---

![Anti-Corruption Layer & Context Mapping](/diagrams/anti-corruption-layer-context-mapping.png)

---

- **Aggregate:** Responsible for domain speific policies to preserve the **Entities** consistency.
- **Anti-Corruption:** Layer that asserts adapted data integrity.
- **Context-Mapping:** Translates data structures, implements guards against data corruption, such as external domain inconsistencies, or unintended data formats, and converts inconsistencies into domain expected representations, by returning or throwing the translated data representation.
- **DTO:** Stands for **"Data Transfer Object"**, it's a naming convention of an unspecific data structure in transit; unknown to the domain.
- **Gateway:** The implemented client of the transit protocol used to communicate with the subdomain - the external bounded-context.
- **Guard:** Used in this context to describe the validation process of the downstream response in the write model. It ensures the result aligns with the domain’s expectations before allowing the process to proceed, protecting against unexpected states.
- **Query:** A structured domain model used to express a read operation; independent of the underlying data-layer implementation.
- **Upstream/Downstream:** Describes the interaction flow in relation to the domain - upstream flows into the domain, downstream flows away from the domain.

---

### Source Code File Structure

These standards describe a file structure and naming convention of the source code. The standard described focuses on the domain language of the solution, rather than the technical terminologies, e.g,. layers, component types, design patterns, etc.

By using an approach that implements the domain language to structure the directories and file names of the source code, the aim is to reduce, or entirely remove - the technical terminologies from this scope.

This model aligns with the **ubiquitous-language** of the domain, expressing the semantic design of the architecture.

- These standards defines a **bounded-context** directory to organize the source code of a **subdomain** in. The **bounded-context** is expected to be listed next to other subdomains of the domain scope.
- The name of the **bounded-context** directory should align with the name of the **subdomain**, which typically share name with the **aggregate root** of the **bounded-context**, or else is expected to follow a **noun-driven** naming convention, _e.g., billing, catalog, identity, shipping, etc_.
- The files expected to be listed directly in the **bounded-context** are the components of the **domain layer**, _e.g., Aggregates, Entities, Value Objects, etc_.
- The direct subfolders in the **bounded-context** directory is expected to list the different **operations** of the domain, following a **verb-driven** naming convention that semantically expresses the **use cases** of the **operations**, _e.g., create-invoice, disable-user, get-order-history, etc_.
- Files in the **use cases** directories are the componets of both the **application** and **infrastructure layers**, _e.g., Adapters, Application Services, Contracts, etc_.
- Names of both **directories** and **folders** are expected to use **lower-case** and **hyphen-separated** formats.

#### Use-Case Examples

- [**Ping-Pong: File Structure**](/use-cases/1-ping-pong.md#file-structure)
- [**Message Board: File Structure**](/use-cases/2-message-board.md#file-structure)

---

### Event-Log

An **event-log** is a log stream, it belongs to a **bounded-context** and is a vector of events that is grouped by a process identity, ordered by when in time the events where persisted.

---

```mermaid
---

config:
    layout: elk
    look: classic
    theme: base
    themeVariables:
        fontFamily: "monospace"
        primaryColor: "#504945"
        primaryTextColor: "#EBDBB2"
        cScaleInv0: '#D65D0E'

---

timeline
    section Event-Log
        Process : Event : Event : Event
        Process : Event : Event : Event
        Process : Event : Event : Event
        Process : Event : Event : Event
        Process : Event : Event : Event
```

---

The benefits of working with a solution that implements an **event-log** of each commanded process:

- **Auditability:** Every change is recorded as an immutable event, providing historical traceability.
- **Regulatory Compliance:** Often a legal and financial requirement to keep an immutable transaction history.
- **Projections:** Provides a consistent version state of the processed schema evolution that can be projected.
- **Simulation:** Can be used to simulate the future of a process, or how a similar process would evolve.
- **Decoupling:** Downstream implementations can react to events decoupled from the broadcaster.

#### Use-Case Example

- [**Sale Order: Event-Log**](/use-cases/3-sale-order.md#example-event-log)

---

### Projection of the Event-Log State

An **event-log** offers a **time-dimensional** view of the evolved process schema state. To create a **flat stateful** data representation of the **time-dimensional** data representation, the event bodies of the **event-log** are merged together.

---

```mermaid
---

config:
    layout: elk
    look: classic
    theme: base
    themeVariables:
        fontFamily: "monospace"
        primaryColor: "#504945"
        primaryTextColor: "#EBDBB2"
        cScaleInv0: '#D65D0E'

---

timeline
    section Event-Log
        Process 1 : Event<br>──<br>foo = 123 
        Process 2 : Event<br>──<br>foo = 123 : Event<br>──<br>bar = 234 
        Process 3 : Event<br>──<br>foo = 123 : Event<br>──<br>bar = 234 : Event<br>──<br>foo = 987
        Process 4 : Event<br>──<br>foo = 123 : Event<br>──<br>bar = 234 : Event<br>──<br>foo = 987<br>baz = 345 : Event<br>──<br>foo = 876
        Process 5 : Event<br>──<br>foo = 123 : Event<br>──<br>bar = 234<br>baz = 345<br>qux = 456 : Event<br>──<br>baz = 987<br>qux = 876 : Event<br>──<br>baz = 765
```

---

The above example shows a set of **time-dimensional event-log representations** ranging between `Process 1` and `Process 5`. Translating them into a set of **flat stateful representations** of the same **event-logs**, then the event data is merged into the following projections:

---

```mermaid
---

config:
    layout: elk
    look: classic
    theme: base
    themeVariables:
        fontFamily: "monospace"
        primaryColor: "#504945"
        primaryTextColor: "#EBDBB2"
        cScaleInv0: '#D65D0E'

---

timeline
    section Projected Event-Log State
        Process 1 : State<br>──<br>foo = 123 
        Process 2 : State<br>──<br>foo = 123<br>bar = 234 
        Process 3 : State<br>──<br>foo = 987<br>bar = 234
        Process 4 : State<br>──<br>foo = 876<br>bar = 234<br>baz = 345
        Process 5 : State<br>──<br>foo = 123<br>bar = 234<br>baz = 765<br>qux = 876
``` 

---

### Composite Event-Log

The **event-log** belongs to a **bounded-context**, a **composite event-log** is a unified log stream from multiple **bounded-context** sources. The **composite event-log** represents a cohesive narrative across phases of a **saga** or a process. 

The term **composite event-log** emphasizes that the log is not from a single source, but constructed from several sources - possibly filtered or correlated.

#### Use-Case Example

- [**Sale Order: Event-Log Composite**](/use-cases/3-sale-order.md#example-event-log-composite)

---

### Saga & Process Choreography

A **saga** is a series of significant events that unfolds the story of how the resulting narrative evolves. It is a reactive, long-running transaction spanning multiple contexts, triggered by events and describes compensating logic for handling edge cases and failures.

The solution design outlined here expects a write model that implements **process managers** in different **bounded-contexts** that orchestrate a choreographed **reactional event model** to a domain **saga**.

The **saga** is expected to be documented using a sequence diagram and implemented in code using one, or multiple, **process managers**.

#### Use-Case Example

- [**Sale Order: Saga**](/use-cases/3-sale-order.md#saga)