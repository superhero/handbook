# Use Cases

---

### 1. [Ping-Pong: Basic HTTP Server](/use-cases/1-ping-pong.md)

- **Overview**: Demonstrates setting up a basic HTTP server that responds to requests with a simple message.
- **Problem Spec**: Implement a lightweight HTTP server with minimal functionality as an entry point for learning the Tool-Chain.
- **Solution**:
  - Uses `@superhero/http-server` to set up and start the server.
  - Implement the ping-pong response logic in the controller.
  - Include a testing suite demonstrating end-to-end request-response handling.

---

### 2. [Message Board: Eventual Consistency and Process Management](/use-cases/2-message-board.md)

- **Overview**: HTTP server that hosts a smaller and simplified eventual consistency solution.
- **Problem Spec**: Handles domain-specific processes triggered by API requests; including asynchronous controllers and downstream event-logging.
- **Solution**:
  - Uses `@superhero/core` to organize the solution.
  - Uses `@superhero/eventflow` for persistence.
  - Aggregate results and returns data state to the requesting client.

---

### 3. [Sale Order: Eventual Consistency and Process Management](/use-cases/3-sale-order.md)

- **Overview**: Demonstrates setting up a sale order fulfillment process.
- **Problem Spec**: Shows a comprehensive event-driven choreography.
- **Solution**:
  - Uses `@superhero/core` to organize the solution, and to manage processes across a CPU cluster.
  - Uses `@superhero/eventflow` for persistence.
  - Uses `@superhero/bootstrap` to manage initialization logic.

---

### TODO ...

---

### 4. TODO: Authorizing User Requests with Session and Signature

- **Overview**: Implements a user authentication flow using session IDs and signatures.
- **Problem Spec**: Validate user sessions securely and log user activities during requests.
- **Solution**:
  - Use `@superhero/core` to manage request handlers and middleware.
  - Verify session IDs from headers and signatures from authentication processes.
  - Log user activities in the @superhero/eventflow middleware.

---

### 5. TODO: Subscribing to Changes in the Eventlog

- **Overview**: Demonstrates a real-time subscription to changes in the eventlog.
- **Problem Spec**: Allow clients to receive updates whenever a new event is logged.
- **Solution**:
  - Use a client eventsource stream to establish a server connection.
  - Subscribe to the eventlog and push updates.
  - Ensure scalability and manage multiple client connections efficiently.