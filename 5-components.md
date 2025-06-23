# Component Documentation

Each component has a separate documentation located together with the source code of the package on github.com, listed and linked below.

---

### [@superhero/array](https://github.com/superhero/array)

The module exports a class that extends the JavaScript `Array` object, implementing additonal methods relative to collection logic. Provides different array manipulation tools.

---

### [@superhero/audit](https://github.com/superhero/audit)

A test-suite tool-kit, including a reporter and a contextual assertion module that extends the `node:assert` module with additional functionality to help test code and understand failed test cases.

---

### [@superhero/bootstrap](https://github.com/superhero/bootstrap)

The `@superhero/bootstrap` component is used to orchestrate initialization logic and runtime setup. It helps coordinate async pre-launch behavior like subscriptions or module wiring.

---

### [@superhero/config](https://github.com/superhero/config)

A lightweight configuration loader that supports environment-based overrides. It enables a service to load context-aware settings via files, environment variables, or in-memory injections, aligning with infrastructure abstraction best practices.

---

### [@superhero/core](https://github.com/superhero/core)

The application framework and contextual lifecycle container. It provides service discovery, lifecycle orchestration, clustering, dependency hierarchy and graceful service termination. It's mainly a composition layer of other lower level components in the tool chain.

---

### [@superhero/date](https://github.com/superhero/date)

Provides utility functions for working with JavaScript `Date` objects in a safer and more predictable way. Emphasizes clarity around time zones, formatting, and interval math for process and infrastructure layers.

---

### [@superhero/deep](https://github.com/superhero/deep)

Includes deep manipulation functions for nested objects—such as merging, cloning, and diffing—used in event payload normalization, schema validation, or internal transformation layers.

---

### [Eventflow](/components/@superhero/eventflow.md)

A message queue solution, designed to manage events and event-logs, and to help developers support event source behaviour in the solution they implement.

This component is seperated into sub-packages, where the two main packages are:

- [**Client:** @superhero/eventflow-spoke](/components/@superhero/eventflow.md/#superheroeventflow-spoke)
- [**Server:** @superhero/eventflow-hub](/components/@superhero/eventflow.md/#superheroeventflow-hub)

---

### [@superhero/http-request](https://github.com/superhero/http-request)

Provides a standardized wrapper for making outbound HTTP(S.md) requests. Offers timeout, retry, and structured response handling consistent with the stack’s observable and testable design philosophy.

---

### [@superhero/http-server](https://github.com/superhero/http-server)

Exposes a component-based HTTP server with middleware support. Designed for running isolated services and exposing APIs, it includes hooks for attaching API layers and route-based observers.

---

### [@superhero/http-server-using-oas](https://github.com/superhero/http-server-using-oas)

Adapts the `@superhero/oas` to the `@superhero/http-server` to provide the ability to route using the **OpenAPI Specification**. The adaption uses the specification to conform the request body and asserts the response of the dispatcher to match the expected result.

---

### [@superhero/id-name-generator](https://github.com/superhero/id-name-generator)

Generates unique, human-readable identifiers—suitable for naming deployments, event streams, or test environments. Uses pluggable dictionaries of adjectives and nouns to produce randomized names.

---

### [@superhero/locator](https://github.com/superhero/locator)

A dependency resolution and service discovery module, used to link modules and wiring configurations at runtime. It ensures that services can be resolved by name and injected into one another explicitly.

---

### [@superhero/log](https://github.com/superhero/log)

Structured, leveled logger that outputs consistent logs for auditing, debugging, and observability. Supports labeled categories and timestamp precision to support distributed tracing across components.

---

### [@superhero/oas](https://github.com/superhero/oas)

A layer on top of the `@superhero/http-server` and `@superhero/router` to validate and route endpoints/operations using OpenAPI specifications.

---

### [@superhero/openssl](https://github.com/superhero/openssl)

A Node.js wrapper for essential OpenSSL functions, used internally by certificate and identity-related components. Enables programmatic key/certificate creation, inspection, and signing.

---

### [@superhero/path-resolver](https://github.com/superhero/path-resolver)

A common module used by other core components, used to resolves a contextual or relative paths at runtime. Simplifies location-based references for files and folders, supporting configurations of a specific base context relative to the intended path to resolve.

---

### [@superhero/router](https://github.com/superhero/router)

Modular router for APIs and message dispatching. Supports versioned routes, async middleware chains, and message observers. Connects the API layer to process and domain logic in a predictable fashion.

---

### [@superhero/tcp-record-channel](https://github.com/superhero/tcp-record-channel)

A low level TCP socket channel module that handles persistent connections with record-framed payloads. Used by the Eventflow solution, and could be used in other service-to-service communications, to ensure secure and ordered framed message delivery.

---

### [@superhero/wild-trie](https://github.com/superhero/wild-trie)

A Trie (DAG - Directed Acyclic Graph) data-structure implementation with wildcard support for graph path pattern matching. A lower level solution that can be used to model and navigate different tree structures.
