# Superhero Tool Chain

- [Purpose of the Tool Chain](#purpose-of-the-tool-chain)
- [Architectural Direction](#architectural-direction)
- [Versioning Strategy](#versioning-strategy)

---

## Purpose of the Tool Chain

The **Superhero Tool Chain** is a modular, event-driven Node.js tool chain designed to build scalable and maintainable systems. Released version 4 introduces a refined stack focused on component isolation, offering a robust backbone solution that can be used as an event hub or message queue in a choreographed **Eventflow** system.

- The tool chain consists of packages under the `@superhero/*` namespace.
- A strict, documented structure promotes clarity, repeatability, and automation.
- Communication between systems is handled using secure and distributed event-driven solutions.

The tool chain is specifically focused on enabling **load distribution** through an **eventual consistency** model well-suited for backend services in a clustered microservice orchestration.

- [Component Documentation](/5-components.md)

---

## Architectural Direction

The **Superhero Tool Chain** is designed to align with defined standards inspired by **Domain-Driven Design** and other well-defined architectural principles.

The intent is to provide a spectrum of small and large library components, frameworks, and standards that help developers and teams implement an observable process design using a seamless built-in **load distribution** model.

The architecture promoted by the standards on this page is centered around component isolation, logical and leveled boundaries with an expected dependency direction.

- [Core Architecture Principles](/2-core-architecture-principles.md)

---

## Versioning Strategy

Each package in the tool chain follows **semantic versioning** (`MAJOR.MINOR.PATCH`). Version 4 introduces `MAJOR` breaking changes from earlier versions and establishes a new stable baseline:

- All v4 packages are `>= 4.0.0`.
- Deprecated v3 components (e.g., Eventsource, Redis integration) have been replaced or removed.
- Each v4 package includes a `README.md` and a test suite for reference and verification.

Packages are updated independently but respect structural contracts and architectural boundaries. Internal dependencies are minimized; each component is designed according to a **composition over abstraction** policy.

### Stability and Update Expectations

Breaking changes or new functionality are introduced in `MINOR` version bumps.
`PATCH` versions are expected to remain fully backward-compatible.
