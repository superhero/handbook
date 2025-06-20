# Core Architecture Principles

- [Philosophy](#philosophy)
- [Layered Architecture](#layered-architecture)
- [Standard Operating Procedures](#standard-operating-procedures)

## Philosophy

The architectural approach described here is inspired by, and aligns with, well-defined architectural principles in **Domain-Driven Design**, **Onion Architecture**, and **Hexagonal Architecture**, where **core business** logic is isolated at the center of the solution. Contextual boundaries are protected by relying on adapted implementations of clearly defined interfaces (**ports**).

- **Core logic has no concrete dependencies:** Adapter-patterns enable simpler unit testing.
- **Core logic remains stable despite infrastructural changes:** Ensures domain stability.
- **Adapters can be swapped independently:** Simplifies refactoring.

---

### Component Isolation & Dependency Direction

These standards advocate a **component isolation policy**, meaning a component must remain isolated from the problem spaces of other components, and should not address problems beyond its contextual scope.

- Components benefit from clearly defined boundaries and responsibilities.
- Isolated components remain stable when coupled components change.
- Isolated components are simpler to test individually.

---

![Component Isolation](/diagrams/component-isolation.png)

---

The adapter-pattern described above ensures **component isolation**, reducing coupling by abstracting the dependency, using a contracted interface implemented by one or more adapters of different solutions to the contracted problem.

The adapter-pattern described implements the **Dependency Inversion Principle** described by Robert C. Martin - _High-level modules should not depend on low-level modules. Both should depend on abstractions._ A principle that confronts **tight coupling**.

**Tight coupling** occurs when components, modules, or classes are strongly dependent on each other's internal structure or behavior. A change in one component forces changes in others - defining instability, causes a maintenance overhead.

## Layered Architecture

A Layered Architecture organizes a system into hierarchical layers, each with distinct roles and responsibilities.

- **Separation of concerns:** Each layer addresses specific responsibilities.
- **Direction of dependencies:** Higher layers depend only on lower layers, never vice versa.
- **Dependency inversion:** Domain and application layers define interfaces, implemented by the infrastructure layer.

---

- [Onion Architecture](#onion-architecture)
- [Hexagonal Architecture (Ports & Adapters)](#hexagonal-architecture-ports--adapters)
- [Domain-Driven Design (DDD)](#domain-driven-design-ddd)
- [CQRS](#cqrs)

---

### Onion Architecture

> Onion Architecture defines a layered system with core domain logic in the center!

In **Onion Architecture**, the core domain model is at the center, surrounded by layers such as **Infrastructure**, **API**, and **View**, creating clear boundaries and a defined direction for dependencies; outer layers may depend inward, but never outward.

- Decouples business logic from infrastructure specifics.
- Each layer evolves independently.
- Changes in external systems has less impact on core behavior.

---

### Hexagonal Architecture (Ports & Adapters)

> Improves the ability to adapt and test the solution in isolation from the deployment context!

Inspired by **Hexagonal Architecture**, the system uses **adapters** for external interactions at upstream (input) and downstream (output) boundaries. The application layer defines **ports** (interfaces), and external infrastructural interactions are implemented via adapters (**plugs**).

Adapters commonly implement:

- Gateways for APIs or protocols.
- Repositories interacting with data layers.
- Controllers and middleware.

---

### Domain-Driven Design (DDD)

> Solutions should reflect the language and needs of stakeholders to the solution!

The architectural principles described originate from **Domain-Driven Design**, which organizes software around business concepts, using a **ubiquitous-language** developed with **domain experts** to ensure clarity and reduce miscommunication.

- The **domain experts** are one or many, with practical experience, preferably a stakeholder in the solution.
- The **ubiquitous-language** is derived from the domain and the **domain experts**, not invented by engineers.

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

### CQRS

> Command Query Responsibility Segregation!

Inspired by Greg Young’s principles, the stack promotes a clear separation between read and write models:

- **Commands:** Trigger state changes and emit events, _e.g., the write model_.
- **Queries:** Optimized projections for reading, _e.g., the read model_.

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

- **Infrastructure Layer:** Where upstream controllers and downstream repositories are implemented.
- **Application Layer:** Responsible for orchestration between infrastructural implementations and the Domain layer.
- **Domain Layer:** Implements the domain-specific business logic.
- **Command Layer:** Write model responsible for persistence modifications in the data layer.
- **Query Layer:** Read model responsible for projections of the data layer.
- **Controller:** Upstream layer responsible for input adapters.
- **Repository:** Downstream layer responsible for output implementations of cross boundary interactions.
- **Adapter:** Adapts infrastructural components to domain-specific expectations.
- **Process Manager:** Command Layer for implementations responsible for process orchestration.
- **Contract:** Interfaces that express what implementations the domain requires to be able to fulfill the domain-specific business logic.
- **Saga:** Process managers that orchestrates domain-specific reactions to domain events.
- **Aggregate:** Implements the domain-specific atomic business logic that guarantees business policies within the contextual boundary.
- **Schema:** Responsible for ensuring data integrity of the data representations it specify.
- **Entity:** A persistent domain object with a unique identity, governed by its aggregate root.
- **Value Object:** A domain object without identity, defined by the value of its attribute(s), often typically used to represent attributes of an Entity.

---

### Anti-Corruption Layer & Context Mapping

Interactions between contexts use an **Anti-Corruption Layer** implementing context mappers to ensure data integrity, translating external representations into domain-specific structures, _e.g., `Entities` and `Value Objects`_.

---

![Anti-Corruption Layer & Context Mapping](/diagrams/anti-corruption-layer-context-mapping.png)

---

- **Anti-Corruption Layer:** Asserts adapted data integrity.
- **Atomic:** All operations in a transaction succeed or none do. If any part fails, the entire transaction is rolled back, leaving the system unchanged.
- **Context-Mapper:** Translates data structures, implements guards against data corruption, such as external domain inconsistencies, or unintended data formats, and converts inconsistencies into domain expected representations, by returning or throwing the translated data representation.
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

---

#### Example Use Case: Message Board

Defined below is an example of a file structure that implements the **Source Code File Structure SOP** described above. The example represents a simple message board solution. The file structure represents a smaller and simplified domain model, focused on serving as an example.

---

```yaml
message-board                             # Domain name.
└ src                                     # Source code directory.
  ├ database                              # Supporting database domain responsible for abstracting the database connection.
  │ ├ gateway.js                          # Database gateway contract.
  │ ├ gateway-mock.js                     # Mocked implementation of the database gateway contract.
  │ └ gateway-mysql.js                    # Mysql client implementation of the database gateway contract.
  ├ post                                  # Core Domain.
  │ ├ create                              # Use case of a domain operation.
  │ │ ├ access.js                         # Upstream contract for the implementation of the controller adapter.
  │ │ ├ access-using-http.js              # The adapter of the upstream controller that implements the "access" contract.
  │ │ ├ command.json                      # Schema of the command model.
  │ │ ├ persist.js                        # Downstream contract for the implementation of the repository adapter.
  │ │ ├ persist-in-database.js            # Adapter of the downstream repository that implements the "persist" contract.
  │ │ ├ persist-in-database.sql           # The SQL query used to persist the post in the database.
  │ │ ├ persist-in-databas-using-http.js  # Application service responsible for the orchestration of the interaction.
  │ │ └ route.json                        # Schema specifying the http route policy.
  │ ├ view                                # ...use case...
  │ │ ├ access.js                         # ...upstream contract...
  │ │ ├ access-using-http.js              # ...upstream adapter...
  │ │ ├ find.js                           # ...downstream contract...
  │ │ ├ find-in-database.js               # ...downstream adapter...
  │ │ ├ find-in-database.sql              # ...SQL query...
  │ │ ├ find-in-database-using-http.js    # ...orchestrator...
  │ │ ├ query.json                        # Schema of the query model.
  │ │ └ route.json                        # ...route schema...
  │ ├ post.js                             # Aggregator of the bounded-context.
  │ └ post.json                           # Entity schema of the aggregate root.
  └ user                                  # Supporting user domain responsible for authentication and authorization.
    ├ find                                # Common use case to find a user.
    │ ├ find.js                           # ...downstream contract...
    │ ├ find-in-database.js               # ...downstream adapter...
    │ └ find-in-database.sql              # ...SQL query...
    ├ hash-password                       # Common use case to hash a password.
    │ ├ hash-password.js                  # ...downstream contract...
    │ └ hash-password-using-bcrypt.js     # ...downstream adapter...
    ├ login                               # ...
    │ ├ access.js
    │ ├ access-using-http.js
    │ ├ command.json
    │ ├ create-session.js
    │ ├ create-session-in-database.js
    │ ├ create-session-in-database.sql
    │ ├ create-session-in-database-using-http.js
    │ └ route.json
    ├ logout
    │ ├ access.js
    │ ├ access-using-http.js
    │ ├ command.json
    │ ├ terminate-session.js
    │ ├ terminate-session-in-database.js
    │ ├ terminate-session-in-database.sql
    │ ├ terminate-session-in-database-using-http.js
    │ └ route.json
    ├ session.json
    ├ user.js
    └ user.json
```

---

### Event-Log & Projection

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

---

#### Projection of the Event-Log State

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

#### Composite Event-Log

The **event-log** belongs to a **bounded-context**, a **composite event-log** is a unified log stream from multiple **bounded-context** sources. The **composite event-log** represents a cohesive narrative across phases of a **saga** or a process. 

The term **composite event-log** emphasizes that the log is not from a single source, but constructed from several sources - possibly filtered or correlated.

---

#### Saga & Process Choreography

A **saga** is a series of significant events that unfolds the story of how the resulting narrative evolves. It is a reactive, long-running transaction spanning multiple contexts, triggered by events and describes compensating logic for handling edge cases and failures.

The solution design outlined here expects a write model that implements **process managers** in different **bounded-contexts** that orchestrate a choreographed **reactional event model** to a domain **saga**.

---

#### Example Use Case: Sale Order Saga

This example uses a **saga** that governs an **Sale Order Fulfillment Process** where each phase (ordering, payment, shipment, notifications) involves event-driven choreography, compensations, retries, and consistent customer communication.

---

[![Sale Order Saga](/diagrams/saga-sale-order.svg)](/diagrams/saga-sale-order.png)

> _View diagram on [mermaid.live](https://mermaid.live/edit#pako:eNq1WFtvozgU_isW-7IrJRUYQhIeKgWYSiPtzlab7jyM8uKAk1oFOwumSrbqfx-bqwmYpittqkaq-53v3M8xvBkRi7HhGfP5fEd3NGL0QI7ejgLxSdCFFdwDOHmpDxh78UCUoDwnUXXEn3GKPbBHOVYOvqOMoH2C85pJfvYoejlmrKCxB3bGL1YITWjtjA5wYJQ_oJQkFwlIGWX5CUVYhZwykqLsErCEZRqWGvKEz7yDffFD34cS1gFRxFnmsyzGFQau5I9KVSL6RMvAfXCdAeh3QvGUUTk5UpR0iNBdhOaXIUJndYNK0B4nPjv7L8cpdS2s9E5Ru1yvR5B9rUPbRNpPV5gBkQgDeUWcMHqtdGieglX9GJLm-J8C0wh_K9K9SmmWnyqddeE20JCgY4bSuhYJTzD4U9oDtuiIqlNUcEZLxqYc9uwMsuP-V8ecger3NxBkGPFaWKkulHESkROiHGwsgPKa_TERlRprcGaHC5CwMdEioUQGsgezFMfgMWNxEYnzV0REpkhC-EUjafclt0g0H9gIHyo8prHe1Ud0STHl48x-6WUNAX_JMOdc54Bvq-BvTApEmLxq8Y4ODzYHLuJli4OUUBG6XMOwHDGvEdJphX2ZSQsXKrYNsAbs9olPiMTgiYGgyDlLmzqazMX2mZz0yQjKZDQYsI2ecVxoyymAPXSIxUDXYu0e9isFTxmiOdEZ4lxTixBmWvJFD_0gqrmBTgZDFAQ5kKgcFePMoQkkdQUUVdNEGrDDTR0XWnr5pvv-pqjqvwRrSKCepGvEG3jsCZ6rLAINhXMDxUfJDRe32TGV8tC9gaMqBJ0rSz1D3VpNp2kIVhO5vZ5nOo71LRzVhNJVmDnpxmXQEkMGq8ewxVGRiU0gGb7SV5QokagHlNIxbYNV3xtrfn-_gV6DnNgx9bZMlCoR0vOzlG6k5MjeDKu61BJaXmX0ZRwOrvAb06u79XrrbkxJZ7Z0agyb_lZlcJKrxkBJbncujy7HMW-h9FYIftTEpYYQtvYpoIGyzqL_y11bSPsi9s0y7K13xVe_TNKqS1JdQ1uOeJEDru6t9v5_6hT59nx-74vwjO975b6nBrWWFCYuvbp1pjrzSnA55VsPFa5vcqyR6cewZHGkroX36etJKbyYzPB_yXKvmdU0QpkG6GkuND6s3WgbvgnZSMVLrrMEfzxXWm4xnlofPjmaejEvQxYIruZWc3UXUlwOpMuBcHn8bhOUZtmdVe19SVvZgazlwPb0SzKQNRs6nyJ1JKnjaZdmIEssXHyKcyE5F974bSqQMQzd2wi7GV-K-a5XLaSBzl6JSAPOElsvb0E5LjUcGFeqeHYZkx6gw6Wmk8f6ol9Sblmf5s2TQPAYM-OYkdjweFbgmSH-lSL5p_EmITujfL-xM-RzaIyyF_kQ-i5kxKb-wVjaiGWsOD4b3gEJe2ZGcYrFs2T9aNpCMC2fkwvKDc9ZOiWH4b0ZZ8ObO5Z9t1pDaK6XENoutGbGRRxb5urOWTsL11mtLMeyLPg-M_4t9cI7y7VtyxWP0EJo5cCZgWPCWfZH9YKnfM_z_hMKKYNd)_

---

**Create Order:**

- Begins when the sale order is placed (`Order Placed`).
- The product availability is confirmed; failure results in cancellation and customer notification.
- If available, the sales area is confirmed; failure also leads to cancellation.
- Success in both leads to the payment phase.

**Payment:**

- Payment is requested and the customer is notified.
- If no payment is received, reminders are issued. After three failed attempts, the order is canceled.
- If payment is received, it must be verified.
- Invalid payment confirmations trigger alerts to the security context.

**Shipment:**

- Upon payment confirmation, a shipment is scheduled.
- The process handles real-world conditions: delays, transit, delivery, or failure.
- In case of failure, the customer is refunded; retry logic is applied if repayment fails.

**Notification:**

- At each decision point, the customer is kept informed.
- Notifications reflect product availability, payment reminders, shipment status, cancellations, and repayment status.
- Security is notified in case of suspicious activity, _e.g., invalid payment_.

---

Examples below are different event-log representations of sale orders processed by the **saga** described above.

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
    section Create Order
        Order 1 : Order Placed : Confirmed Product Availability : Confirmed Sales Area
        Order 2 : Order Placed : Order Canceled
        Order 3 : Order Placed : Confirmed Product Availability : Order Canceled
        Order 4 : Order Placed : Confirmed Product Availability : Confirmed Sales Area
        Order 5 : Order Placed : Confirmed Product Availability : Confirmed Sales Area
```

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
    section Payment
        Order 1 : Payment Requested : Payment Received : Payment Confirmed
        Order 2 : -
        Order 3 : -
        Order 4 : Payment Requested : Payment Request Reminded : Payment Request Reminded : Payment Request Reminded : Order Canceled
        Order 5 : Payment Requested : Payment Received : Payment Confirmed
```

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
    section Shipment
        Order 1 : Shipment Scheduled : Shipment Delivered
        Order 2 : -
        Order 3 : -
        Order 4 : -
        Order 5 : Shipment Scheduled : Shipment Failed
```

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
    section Notification
        Order 1 : Notified Customer of Payment Requested : Notified Customer of Shipment Delivered
        Order 2 : Notified Customer of Product Unavailable : Notified Customer of Order Canceled
        Order 3 : Notified Customer of Sales Area Unavailable : Notified Customer of Order Canceled
        Order 4 : Notified Customer of Payment Requested : Notified Customer of Payment Reminder : Notified Customer of Payment Reminder : Notified Customer of Payment Reminder : Notified Customer of Order Canceled
        Order 5 : Notified Customer of Payment Requested : Notified Customer of Shipment Failed : Notified Customer of Repayment Failed : Notified Customer of Repaid Payment
```

---

Below example shows a **composite event-log** representation of the same event-log examples listed above.

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
    section Composite Event-Log of the Sale Order Saga
        Order 1 : Order Placed : Confirmed Product Availability : Confirmed Sales Area : Payment Requested : Notified Customer of Payment Requested : Payment Received : Payment Confirmed : Shipment Scheduled : Shipment Delivered : Notified Customer of Shipment Delivered
        Order 2 : Order Placed : Order Canceled : Notified Customer of Product Unavailable : Notified Customer of Order Canceled
        Order 3 : Order Placed : Confirmed Product Availability : Order Canceled : Notified Customer of Sales Area Unavailable : Notified Customer of Order Canceled
        Order 4 : Order Placed : Confirmed Product Availability : Confirmed Sales Area : Payment Requested : Notified Customer of Payment Requested : Payment Request Reminded : Notified Customer of Payment Reminder : Payment Request Reminded : Notified Customer of Payment Reminder : Payment Request Reminded : Notified Customer of Payment Reminder : Order Canceled : Notified Customer of Order Canceled
        Order 5 : Order Placed : Confirmed Product Availability : Confirmed Sales Area : Payment Requested : Notified Customer of Payment Requested : Payment Received : Payment Confirmed : Shipment Scheduled : Shipment Failed : Notified Customer of Shipment Failed : Payment Repaid To Customer : Notified Customer of Repayment Failed : Payment Repaid To Customer : Notified Customer of Repaid Payment
```
