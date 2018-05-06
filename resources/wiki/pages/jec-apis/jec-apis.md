# JEC APIs

The following sections give a brief summary of the technologies required by the JEC platform and the APIs used in JEC applications.

## JavaScript Enterprise Containers Commons API

JEC provides top-level APis which are the fundamentals to guarantee the portability of server-side JavaScript projects .

### JEC Jslet Technology

JEC Jslet technology lets you define HTTP-specific jslet classes. A jslet class extends the capabilities of JavaScript servers that host applications accessed by way of a request-response programming model. Although jslets can respond to any type of request, they are commonly used to extend the applications hosted by JavaScript web servers.

### JEC Security API

The JEC Security API specification defines portable, plug-in interfaces for HTTP authentication and identity stores, and several interfaces that provides an API for programmatic security.

### JavaScript Connector API for Decorators _(JCAD)_

JavaScript Connector API for Decorators _(JCAD)_ is a SPI that turns TypeScript decorators into an abstraction layer. JCAD allows developers to choose implementations to execute routines depending on injected metadata.

## Dependency Injection for JavaScript Enterprise Containers _(JeDI)_

JavaScript Dependency Injection for _(JDI)_ defines a set of annotations and services, provided by JEC, that make it easy for developers to use enterprise beans along with Jslet technology in web applications.

## RESTful Services API for JavaScript Enterprise Containers _(JARS)_

JavaScript RESTful Services API _(JARS)_ is a JavaScript programming language API spec that provides support in creating web services according to the Representational State Transfer _(REST)_ architectural pattern. JARS uses JCAD and decorators, to simplify the development and deployment of web service clients.

## JavaScript Enterprise Containers API for WebSocket

_[Not available yet.]_

## JavaScript Unit Testing API _(JUTA)_

JavaScript Unit Testing API _(JUTA)_ is a high level specification to write unit testing for JavaScript programs.
It provides an abstraction layer for all popular JavaScript unit testing frameworks (e.g. [Mocha.js](https://mochajs.org/), [Jasmine](https://jasmine.github.io/), etc.).
