# Message Board

###### Use Case

---

## Implementation

This example represents a simple message board solution. The implementation represents a smaller and simplified domain model, focused on serving as an example.

---

### File Structure

Defined below is an example of a file structure that implements the [**Source Code File Structure SOP**](/2-core-architecture-principles.md#source-code-file-structure).

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

### Work in Progress

... to be continued