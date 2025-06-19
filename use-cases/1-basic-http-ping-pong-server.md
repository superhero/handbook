# Basic HTTP Ping-Pong Server

###### Use Case

This example provides a foundational starting point for understanding how to build services using the Superhero Tool-Chain. It focuses on implementing a minimal HTTP server that exposes a basic endpoint, returning a simple response. The intent is to introduce the structure, conventions, and module integration within the Tool-Chain in the most lightweight form possible.

The server is built using the `@superhero/http-server` component, configured to handle incoming HTTP requests. A simple controller is implemented to respond to a `ping` request with a simple `pong` message, illustrating the separation of concerns and modular design encouraged throughout the Tool-Chain. To ensure correctness and provide a reference for test-driven development, a minimal test suite verifies the request-response cycle end-to-end.

---

## Implementation

_The overwelming file structure used for this simple `ping-pong` service is to serve as a simple example that can be scaled in a context where a strong adaption model is more important._

---

### File Structure

```yaml
ping-pong
├ src
│ ├ http-server
│ │ ├ listen
│ │ │ ├ start-server.js
│ │ │ └ server-is-listening.test.js
│ │ └ config.json
│ ├ ping
│ │ ├ response
│ │ │ ├ access.js
│ │ │ ├ access-using-http.js
│ │ │ ├ ping-responds-with-pong.test.js
│ │ │ ├ respond.js
│ │ │ ├ respond-with-pong.js
│ │ │ ├ respond-with-pong-using-http.js
│ │ │ └ route.json
│ │ └ config.js
│ └ run.js
└ package.json
```

The directory of the package is named in accordence to the **domain** name `ping-pong`. The package directory has a `src` sub-directory where all source code of the package is located. In this simplified example, we only have two **sub domains**; the **core domain** called `ping`, and the **supportive domain** called `http-server`.

The `ping` **core domain** is a **bounded context** that defines the **use case** `response` as a directory where the implementation of that **use case** is defined.

- **access.js:** Upstream contract that describes the implementation interface of the input plug.
- **access-using-http.js:** Upstream HTTP-controllor adapter of the contracted access interface.
- **ping-responds-with-pong.test.js:** Unit-tests of the use case following a semantical naming convention principle with a `.test.js` suffix to differentiate it from the source code of the solution.
- **respond-with-pong.js:** Contract that describes the pluggable interface of the downstream implementation.
- **respond-with-pong-using-http.js:** A simple downstream implementation representing the data layer.
- **route.json:** Specification using the OpenAPI standards to describe the http route of the endpoint.

The `http-server` defines a different **bounded context** with the `listen` **use case** that dosn't have any defined **plugs**, only the **application service** that starts the server, and a **test case** that the server is listening.

Each **bounded-context** is expected to have a `config` file that configure the integration of all **use cases** to the **bounded-contexts**. 

Beyond the **bounded-contextual** sub-directories of the `src` folder, a `run` script is expected to define the initiation process of the application that use the `config` files of the **bounded-context** to integrate them together.

Finally, in the root directory, there is a `package.json` file defined to specify package configurations, e.g., module name, dependencies, etc.

---

### Run Script

**/ping-pong/src/run.js:** Uses the Core component of the Superhero Tool-Chain to initiate the application.

```js
// The Superhero Tool-Chain has the application component called "Core".
import Core from '@superhero/core'

// The instance of the Core is used to as the application space.
const core = new Core()

// Integrate different contexts with the application space by adding them to core instance.
// The core integrates the different contexts by merging the config files specified at the component path.
await core.add('@superhero/oas')  // ... generic domain
await core.add('./ping')          // ... core domain
await core.add('./http-server')   // ... supportive domain

// Bootstrapping the core initiates the configured application space.
await core.bootstrap()
```

---

### Context Configurations

**`/ping-pong/src/http-server/config.json`:** Using a JSON config format.

```json
{
  "bootstrap":
  {
    "ping-pong/http-server/listen/start-server" : true,
  },
  "locator":
  {
    "ping-pong/http-server/listen/start-server" : "./listen/start-server.js"
  },
  "http-server":
  {
    "log":
    {
      "muteInfo": false
    }
  }
}
```

**`/ping-pong/src/ping/config.js`:** Using a JavaScript exported configuration.

```js
export default
{
  'bootstrap':
  {
    'ping-pong/ping/response/respond-with-pong-using-http' : true,
  },
  'locator':
  {
    "ping-pong/ping/response/respond-with-pong-using-http" : "./response/respond-with-pong-using-http.js"
  }
}
```

_The `http-server` and `ping` **sub-domains** are implemented using different config formats to provide the different examples of the different format abilities._

Both `config` files define a **bootstrap** directive to run the **use cases** orchestrating **application services** during the **bootstrap phase** during the **application initiation**. 

The bootstrap directive is using a **service name** mapped to a boolean `true` value. The true value indicates that the directive should be honored. It's possible to write over the configuration from a different context to supress the directive with a `false` value. _**Example:** could be useful when performing some types of test cases, or when a generic context added to the application does more than what's required._

The **service names** are configured by defining the `locator` map. As the example shows, it's possible to use relative path mapping.

---

### Application Services

**`/ping-pong/http-server/listen/start-server.js`:** Solves the domain problem of starting an HTTP server.

```js
/**
 * @memberof PingPong.ping.httpServer~StartServer
 */
export function locate(locator)
{
  const httpServerUsingOas = locator.locate('@superhero/http-server-using-oas')
  return new StartServer(httpServerUsingOas.server)
}

/**
 * @memberof PingPong.ping.httpServer
 */
export default class StartServer
{
  /**
   * @param {@superhero/http-server} httpServer - generic domain solution
   */
  constructor(httpServer)
  {
    this.httpServer = httpServer
  }

  /**
   * Implements the bootstrap function to be able to plug in to 
   * the application bootstrap phase.
   */
  async bootstrap()
  {
    this.httpServer.listen(process.env.HTTP_PORT ?? 80)
  }
}
```

**`/ping-pong/ping/response/respond-with-pong-using-http.js`:** Solves the domain problem of responding to a ping request with an injected response model.

```js
import AccessUsingHttp from './access-using-http.js'
import RespondWithPong from './respond-with-pong.js'

/**
 * @memberof PingPong.ping.response~RespondWithPongUsingHttp
 */
export function locate(locator)
{
  const 
    respondWithPong     = new RespondWithPong(),
    accessUsingHttp     = new AccessUsingHttp(respondWithPong),
    httpServerUsingOas  = locator.locate('@superhero/http-server-using-oas')

  return new RespondWithPongUsingHttp(accessUsingHttp, httpServerUsingOas)
}

/**
 * @memberof PingPong.ping.response
 */
export default class RespondWithPongUsingHttp
{
  constructor(specification, dispatcher, router)
  {
    this.dispatcher = dispatcher
    this.router     = router
  }

  /**
   * Implements the bootstrap function to be able to plug in to 
   * the application bootstrap phase.
   */
  bootstrap()
  {
    const
      path    = '/ping',
      method  = 'get'

    this.router.setOasRoute(path, method, this.dispatcher)
  }
}
```

Both **application services** exports a **locate** function that the **locator** in the **core** component can use to resolve the configured name and service path to a service instance.

---

### Contracts

**`/ping-pong/ping/response/access.js`:** Upstream controller contract.

```js
/**
 * @typedef {Object} Access
 * @memberof PingPong.ping.response
 * @property {Respond} respond
 */
```

**`/ping-pong/ping/response/respond.js`:** Downstream repository contract.

```js
/**
 * @typedef {number} sleep
 * @memberof PingPong.ping.response
 * @see PingPong.config.oas.components.schemas.sleep
 */

/**
 * @typedef {string} response
 * @memberof PingPong.ping.response
 * @see PingPong.config.oas.components.schemas.response
 */

/**
 * Resolver function that takes a sleep argument and returns a resolved response.
 * @callback Resolver
 * @memberof PingPong.ping.response~Respond
 * @param {sleep} sleep - Time in ms to sleep before resolving
 * @returns {Promise<response>} response
 */

/**
 * Downstream repository that can resolve a ping query.
 * @typedef {Object} Respond
 * @memberof PingPong.ping.response
 * @property {Resolver} resolve
 */
```

The contracts are defined using JSDoc annotations, but could alternatively be defined using markdown or some other format convention. The value of defining the contracts in JavaScript is documentational, as the language doesn't support to implement contracted interfaces.

---

### Route Schema

**`/ping-pong/ping/response/route.json`:** Specify the route using an OpenAPI specification.

```json
{
  "components":
  {
    "schemas":
    {
      "sleep":
      {
        "description" : "Time period in milliseconds before the request is resolved.",
        "type"        : "integer",
        "minimum"     : 0,
        "maximum"     : 10000,
        "default"     : 0,
      },
      "response":
      {
        "description" : "The ping-pong response.",
        "type"        : "string"
      }
    }
  },
  "paths":
  {
    "/ping":
    {
      "get":
      {
        "description" : "Responds to a ping request with a pong message.",
        "properties"  :
        {
          "sleep" :
          {
            "description" : "Use the optional query variable to define a delayed response.",
            "in"          : "query",
            "schema"      : { "$ref" : "#/components/schemas/sleep" }
          }
        },
        "responses":
        {
          "200":
          {
            "description" : "Status 200 represents a successfull ping-pong response.",
            "content"     :
            {
              "application/json":
              {
                "schema": { "$ref" : "#/components/schemas/response" }
              }
            }
          }
        }
      }
    }
  }
}
```

---

### Controller

**`/ping-pong/ping/response/access-using-http.js`**

```js
/**
 * HTTP-adapter implementation of the upstream controller port to access 
 * the `ping-response` **use case**.
 * @memberof PingPong.ping.response
 */
export default class AccessUsingHttp
{
  /**
   * @param {Respond} respond 
   */
  constructor(respond)
  {
    this.respond = respond
  }

  /**
   * Implements the dispatch interface to be able to plug in to the router 
   * of the `@superhero/http-server` component.
   */
  async dispatch(request, session)
  {
    session.view.body = await this.respond.resolve(request.query.sleep)
  }
}
```

---

### Repository

**`/ping-pong/ping/response/respond-with-pong.js`**

```js
/**
 * Adapted repository of the {@link Respond} port to plug in to the 
 * contracted data-layer by.
 * @memberof PingPong.ping.response
 */
export default class RespondWithPong
{
  /**
   * Implements the resolve method of the {@link Respond} contract.
   * @param {sleep} sleep - ms to delay the response with.
   */
  async resolve(sleep)
  {
    await new Promise(resolve => setTimeout(resolve, sleep))
    return 'pong'
  }
}
```

---

### Npm Package

**`/ping-pong/package.json`:** Defines the package dependencies, scripts, and other configurations.

```json
{
  "name": "ping-pong",
  "version": "1.0.0",
  "description": "A simple ping-pong service written as a use case demo for the Superhero Tool-Chain",
  "keywords": [
    "ping",
    "pong",
  ],
  "license": "MIT",
  "type": "module",
  "scripts": {
    "start": "node src/run.js",
    "test": "node --test --test-reporter=@superhero/audit/reporter --experimental-test-coverage"
  },
  "dependencies": {
    "@superhero/core": "^4.1.7",
    "@superhero/http-server-using-oas": "^4.0.0"
  },
  "devDependencies": {
    "@superhero/audit": "^4.0.3"
  }
}
```

---

## Summary of the Package

This example is using the defined OpenAPI specification to implement the **Anti-Corruption Layer** of the upstream controller by taking advantage of the functionality offered by the **generic sub-domain** `@superhero/http-server-using-oas`.

**Install the dependencies of the application:**

```bash
npm install
```

**Test the application:**

```bash
npm test
```

**Start the application:**

```bash
npm start
```
