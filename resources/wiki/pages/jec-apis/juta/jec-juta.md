# JavaScript Unit Testing API _(JUTA)_ 

## What Is JUTA?

JUTA is an open source specification to write test cases for TypeScript programs. It is part of the JavaScript Enterprise Container (JEC) project.

JUTA introduces the following features to JavaScript unit testing:

- an easy-to-learn standard to write and run test cases
- a portable way to write test cases embedded within test classes
- an abstraction layer based on TypeScript decorators

JUTA implementations are built on top of existing unit testing frameworks, such as Mocha and Jasmine:

![JUTA Abstraction Layers][jec-juta-abstraction-layers]

[jec-juta-abstraction-layers]: https://raw.githubusercontent.com/jec-project/jec-juta/master/docs/assets/juta-abstraction-layers.png

## Relation to Java

The aim of JEC is to introduce concepts from the Java community into the JavaScript ecosystem. By doing this, we want to take benefits of the highly industrialized processes available in Java environments. That is why JUTA inherits two major features from Java:

- a JUnit-like structure
- the JEE's APIs and annotations philosophy

## Uniforming Unit Testing

One of the most important features of JUTA is that it is exclusively devoted to unit testing projects written with [TypeScript](https://www.typescriptlang.org/). Thus, you can use JUTA to test your [Angular](https://angular.io/) apps and your [Node.js](https://nodejs.org) back end in the same way.

Uniformization of unit testing written in TypeScript provides a real gain regarding test quality and maintainability.