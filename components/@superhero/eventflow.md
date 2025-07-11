# @superhero/eventflow

###### [Component Documentation](/5-components.md)

The Eventflow component is designed as a message queue, process and event manager. To describe a high level overview of the Eventflow solution; it's composed by one part hub - the event hub, and one part spoke - the client, where each client is used by a service that publishes events that other services, or replications of the same service, reacts to.

The Eventflow ecosystem is composed by a set of direct sub components:

- [@superhero/eventflow-certificates](#superheroeventflow-certificates)
- [@superhero/eventflow-db](#superheroeventflow-db)
- [@superhero/eventflow-hub](#superheroeventflow-hub)
- [@superhero/eventflow-spoke](#superheroeventflow-spoke)

---

### [@superhero/eventflow-certificates](https://github.com/superhero/eventflow-certificates)

Handles TLS certificate generation and renewal for use with the Eventflow network. This supports secure authentication between hubs and spokes through automated certificate provisioning.

---

### [@superhero/eventflow-db](https://github.com/superhero/eventflow-db)

Implements the durable event storage backend for the Eventflow system using SQL (e.g. MySQL). It maintains the canonical log of domain events, supporting versioning, replay, and projection recovery.

---

### [@superhero/eventflow-hub](https://github.com/superhero/eventflow-hub)

Acts as the central message broker in the Eventflow system. It receives events from spokes, schedules them, and persists them into the eventflow-db. Supports secure multi-spoke connectivity.

---

### [@superhero/eventflow-spoke](https://github.com/superhero/eventflow-spoke)

A client component that connects to an Eventflow Hub over TLS, allowing services to publish, consume, and subscribe to events in a decoupled fashion. Represents a service's "spoke" in the hub-and-spoke topology.
