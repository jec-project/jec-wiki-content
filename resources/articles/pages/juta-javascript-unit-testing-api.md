<div class="jec-jumbotron mat-elevation-z2">
    <ul class="article-permalink">
        <li><span>Title:</span> JUTA: JavaScript Unit Testing API</li>
        <li><span>Author:</span> Pascal Echemann</li>
        <li><span>Permalink:</span> <a  href="https://dzone.com/articles/juta-javascript-unit-testing-api" title="JUTA: JavaScript Unit Testing API">https://dzone.com/articles/juta-javascript-unit-testing-api</a></li>
        <li><span>Publish Date:</span> Jun. 28, 17</li>
    </ul>
</div> 

# JUTA: JavaScript Unit Testing API

> JUTA, or JavaScript Unit Testing API, is an open source specification for TypeScript test cases, solving many problems of unit testing frameworks. Learn to get started.

JavaScript unit testing has become one of the major characteristics of modern Web development. We can choose now among tens of [JavaScript unit testing frameworks](https://stackoverflow.com/questions/300855/javascript-unit-test-tools-for-tdd).

Unfortunately, it also means that developers are dependent on the unit testing framework they decide to use when they start a new project. Moreover, most of the popular JavaScript unit testing frameworks have not been designed to fit the OOP philosophy, like JUnit does.

Consequently, developers face several issues when using the existing frameworks. Currently, unit testing processes are

- not standardized.
- not portable.
- less flexible than class-based processes.
- not natively based on the TypeScript language.
- less intuitive than annotations-based processes.

## What Is JUTA?

[JUTA](https://github.com/jec-project/jec-juta) is an open source specification to write test cases for TypeScript programs. It is part of the [JavaScript Enterprise Container](https://github.com/jec-project/JEC) (JEC) project.

JUTA introduces the following features to JavaScript unit testing:

- an easy-to-learn standard to write and run test cases
- a portable way to write test cases embedded within test classes
- an abstraction layer based on [TypeScript decorators](https://www.typescriptlang.org/docs/handbook/decorators.html)

JUTA implementations are built on top of existing unit testing frameworks, such as [Mocha](https://mochajs.org/) and [Jasmine](https://jasmine.github.io/).

![JUTA Abstraction Layers][jec-juta-abstraction-layers]

[jec-juta-abstraction-layers]: https://raw.githubusercontent.com/jec-project/jec-juta/master/docs/assets/juta-abstraction-layers.png?style=jec-main-logo

## Relation to Java

The aim of JEC is to introduce concepts from the Java community into the JavaScript ecosystem. By doing this, we want to take benefits of the highly industrialized processes available in Java environments. That is why JUTA inherits two major features from Java:

- a JUnit-like structure
- the JEE's APIs and annotations philosophy

## Uniforming Unit Testing

One of the most important features of JUTA is that it is exclusively devoted to unit testing projects written with TypeScript. Thus, you can use JUTA to test your [Angular](https://angular.io/) apps and your [Node.js](https://nodejs.org/en/) back end in the same way. This will be really helpful when the GlassCat application server (the default JEC implementation) will be released in a couple of months.

Uniformization of unit testing written in TypeScript provides a real gain regarding test quality and maintainability.

## Getting Started With JUTA

The easiest way to start with JUTA is to download the sample project, written with the [Tiger framework](https://github.com/jec-project/jec-tiger).

Tiger is the default implementation of the JUTA specification. It is built over the [Mocha.js](https://mochajs.org/), which is the unit testing framework of the JEC APIs. Note that Tiger is the unit testing framework of all JEC default implementations.

Go to [this link](https://github.com/jec-project/jec-sample-tiger) and follow the instructions for installing the project. We assume that you are familiar with [TypeScript](https://www.typescriptlang.org/) projects and the [Node.js](https://nodejs.org/en/) development environment.

The project structure is quite easy to address:

- The `src` folder contains the source code of a basic calculator.
- The `test` folder contains all of the test suites related to each class in the src folder.

We recommend you to use the [Standard Directory Layout](http://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html) as specified by the Apache Software Foundation to keep tests separate from source files.

## Tiger From Scratch

At the moment, Tiger is the first JUTA implementation available for production purposes. Once you have written unit tests for your Angular or Node.js app, you can install Tiger to run them:

```shell
$ npm install jec-tiger –save-dev
$ npm install mocha --save-dev
```

Then, you have to add a basic configuration file (e.g. `test-config.ts`) to your project to bootstrap the lookup process for test classes:

```typescript
import { TestStats } from "jec-juta";
import { Tiger, TigerFactory } from "jec-tiger";
const factory:TigerFactory = new TigerFactory();
const tester:Tiger = factory.create();
tester.process((stats:TestStats)=> {
  if(stats.error) console.error(stats.error);
});
```

Finally, set up the command line of your `package.json` file as follows:

```json
"scripts": {
  "test": "mocha test-config"
}
```

Now, unit testing can be run by using the standard `npm` command:

```shell
$ npm test
```

## JUTA Ease-of-Use

JUTA's simplicity helps developers to focus on the test case itself, more than the structure of the test suite. Test suites are TypeScript classes that implement JUTA decorators to define a collection of test cases. Test classes are declared by using the `@TestSuite` decorator, as shown below:

```typescript
import { TestSuite, Test } from "jec-juta";
@TestSuite({
  description: "Test Greetings class methods."
})
export class GreetingsTest {
  //Test cases here...
}
```

This approach determines the portability concept of JUTA; test suites are completely decoupled from test execution environments.

The `@Test` decorator indicates that a method contains a test case. You just have to utilize an assertion library in the body of this test method to check the behavior of the specified object.

You can use any assertion library to write test cases. The following sample code shows the usage of the Assert API, imported from the [Chai.js](http://www.chaijs.com/) library, to implement strict equality tests:

```typescript
@Test(
  description: 'should say "Hello" to the world'
)
public sayHelloTest():void {
  let greetings:Greetings = new Greetings();
  expect(greetings.sayHello()).to.equal("Hello");
}
```

## Testing Asynchronous Code

Running code asynchronously is one of the most important parts of the modern JavaScript paradigm. JUTA makes testing asynchronous code as simple as testing synchronous code. By adding the `@Async` decorator to the callback parameter of a test method, you indicate that the test case must be invoked asynchronously:

```typescript

@Test(
  description: 'should say "Hello" to the world asynchronously'
)
public sayHelloAsyncTest(@Async done:Function):void {
  let greetings:Greetings = new Greetings();
  greetings.sayHelloAsync((message:string)=> {
    expect(message).to.equal("Hello");
    done();
  });
}
```

## JUTA Features

The JUTA API comes with many features to help you write high quality and readable tests. JUTA decorators let you set the

- test repetitions,
- test execution order,
- disabling of tests, and
- test fixtures.

Test parameters are defined by interfaces, which provide developers autocomplete capabilities when they work with editors like [VS Code](https://code.visualstudio.com/).

For a complete overview of the API, please go to the [project wiki](https://jecproject.org/wiki/docs/reference/jec-apis/juta/jec-juta).

## More to Come...

We have planned to add parameterized tests to the JUTA specification. This feature should be added to the API in the next release. The Tiger framework will be updated at the same time, to fit the specification.

Unlike JUnit, parameterized tests will not use constructor initialization. Indeed, we want JUTA classes to be Plain Old JavaScript Objects to ensure a better portability. Thus, parameterized tests will only use the declarations below:

- The `parametrized` property of the `@TestSuite` decorator indicates whether the test suite is parametrized (`true`), or not (`false`, default).
- The `@Parameters` decorator marks a method as being the parameters provider for the test suite.
- The `@Parameter` decorator defines a parameter to be initialized by the test runner.

Referring to JUnit, the JUTA features should be enough to cover the whole spectrum of test cases. JUTA is an open specification, so improvement requests can be discussed by the community.

## Summary

In this article, we have exposed a brief description of the JavaScript Unit Testing API. JUTA is a small part of the JEC project and the only one that can be used outside of a JavaScript container.

In the coming month, we will show you the GlassCat application server and many useful projects based upon the JEC philosophy: "Standardization and portability of JavaScript web applications."

For more guides and tutorials on how to use JUTA, check out the [GitHub page](https://github.com/jec-project/jec-juta)!

For an early preview of GlassCat and the JEC project, please go to the [project pages](https://jecproject.org/).