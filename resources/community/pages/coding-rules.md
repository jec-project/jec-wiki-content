# Coding Rules

To ensure consistency throughout the source code, keep these rules in mind as you are working:

- All features or bug fixes must be tested by one or more [Unit Tests](./community/coding-rules#unit-testing).
- All API members (public, private, protected, contructor...) must be documented.
- We wrap all code at 80 characters.

## Unit Testing

Unit Testing must be the cornerstone of the JEC Project.

Unit Testing must be the cornerstone of the JEC Project. JEC uses two frameworks for writing test suites:

- [Mocha.js](https://mochajs.org/) is used to test all core APIs, such as JEC commons, JARS, or JDI...
- [JUTA](http://localhost:4200/docs/reference/jec-apis/juta/jec-juta) is used to test all implementations, such as GlassCat, Sokoke, or Sandcat...

### What should you test?

> Unit tests should cover the entire API.

Each new functionality must ship with its own test suites that test all public methods. It is a hard deal to cover the entire internal API by using the external API. Thus, Unit Testing design should be thought during development.

For example, prefer to separate complex internal algorithms into dedicated classes that can be tested individually.

## API Documentation

We use [Typedoc](http://typedoc.org) as documentation generator for JEC projects. 
JEC API documentation follows [TSDoc](https://github.com/Microsoft/tsdoc) specification for TypeScript doc comments.

All project contributions must include doc comments in the specified standard. **We cannot accept code without this.**