# What is JEC?

JavaScript Enterprise Container _(JEC)_, is a set of specifications for enterprise features such as distributed computing and web services.

JEC is defined by its specification.  The specification defines APIs and their interactions.

### Web specifications

- **Jslet**: defines how to manage HTTP requests, in an asynchronous way. It is low level and other JEC specifications rely on it
- **JWS**: the JavaScript API for WebSocket specification defines a set of APIs to service WebSocket connections

### Web service specifications

- **JARS**: the JavaScript API for RESTful Services provides support in creating web services according to the Representational State Transfer _(REST)_ architectural pattern

### Enterprise specifications

- **JDI**: the JavaScript depencency Injection API is a specification to provide a depencency injection container
- [**JUTA**](./docs/reference/jec-apis/juta/jec-juta): the JavaScript Unit Testing API provides support in creating annotation-based test suites 

## Non Blocking Architecture

The main idea behind JEC is to provide developers an easy way to serve non blocking and asynchronous applications.

JEC specifies that apps default configuration is stateless. When combined with non blocking architectures, it serves RESTful-based microservices that are much more fast than for stateful apps.

## TypeScript Abstraction

By default, JEC serves JavaScript applications built on top of the [Node.js](https://nodejs.org/) runtime. But, since it is entirely written in TypeScript, you can use the JEC specification do deploy applications over other languages that support [AOP](https://en.wikipedia.org/wiki/Aspect-oriented_programming), such as JAVA.