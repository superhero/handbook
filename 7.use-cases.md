## Use Cases

> TODO! to show use cases with code examples.

---

### 1. Basic HTTP Ping-Pong Server

- **Overview**: Demonstrates setting up a basic HTTP server that responds to requests with a simple message.
- **Problem Spec**: Implement a lightweight HTTP server with minimal functionality as an entry point for learning the Tool Chain.
- **Solution**:

- Use @superhero/http-server to set up and start the server.
- Implement the ping-pong response logic in the controller.
- Include a testing suite demonstrating end-to-end request-response handling.

---

### 2. HTTP Eventual Consistency and Process Management

- **Overview**: Expands the HTTP server to manage processes asynchronously, ensuring eventual consistency.
- **Problem Spec**: Handle a domain-specific process triggered by an incoming request, including asynchronous upstream calls and event-logging.
- **Solution**:

- Use `@superhero/core` to organize the solution and manage processes across CPU clusters.
- Call multiple upstream services to validate their state.
- Log the results in `@superhero/eventflow` for persistence.
- Aggregate results and return the final state to the requesting client.

---

### 3. Bootstrap Subscription to Docker Monitoring Stream

- **Overview**: Demonstrates setting up a subscription to a Docker state monitoring API during the bootstrap phase.
- **Problem Spec**: Show how to initialize subscriptions dynamically during application startup.
- **Solution**:

- Use `@superhero/bootstrap` to manage initialization logic.
- Set up a persistent subscription to the Docker monitoring stream.
- Handle and log incoming stream updates with @superhero/eventflow.

---

### 4. Authorizing User Requests with Session and Signature

- **Overview**: Implements a user authentication flow using session IDs and signatures.
- **Problem Spec**: Validate user sessions securely and log user activities during requests.
- **Solution**:

- Use `@superhero/core` to manage request handlers and middleware.
- Verify session IDs from headers and signatures from authentication processes.
- Log user activities in the @superhero/eventflow middleware.

---

### 5. Subscribing to Changes in the Eventlog

- **Overview**: Demonstrates a real-time subscription to changes in the eventlog.
- **Problem Spec**: Allow clients to receive updates whenever a new event is logged.
- **Solution**:

- Use a client eventsource stream to establish a server connection.
- Subscribe to the eventlog and push updates.
- Ensure scalability and manage multiple client connections efficiently.