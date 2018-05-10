# JEC Commons

## Description

The JEC Commons API provides different sets of components that focus on abstraction layers and all aspects of  low level reusable components:

- **The Logging API** - Components that define a bridge between different JEC logging implementations.
- **The JavaScript Connector API for Descriptors (JCAD)** - An abstraction layer which allows developers to create JEC modules based on TypeScript Decorators.
- **The Boostrap API** - The components used to initialize JEC applications, especially by declaring custom JEC  implementations.
- **The Class Loading API** - The components that allow to load JavaScript classes and resources into JEC servers.

## API Reference

The JEC Commons API Reference is available at <http://jecproject.org/docs/jec-commons/api/>.

## Portability

The JEC Commons API is a pure JavaScript layer, with no dependencies to specific platforms, such as Node.js. This guarantees the portability of JEC applications and enables development of many different implementations, based on the core specification.