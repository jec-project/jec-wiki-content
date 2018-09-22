<div class="jec-jumbotron mat-elevation-z2">
    <ul class="article-permalink">
        <li><span>Title:</span> Creating RESTful APIs With JEC — Part 4: Unit Testing</li>
        <li><span>Author:</span> Pascal Echemann</li>
        <li><span>Permalink:</span> <a  href="https://dzone.com/articles/creating-restful-apis-with-jec-part-4-unit-testing">https://dzone.com/articles/creating-restful-apis-with-jec-part-4-unit-testing</a></li>
        <li><span>Publish Date:</span> September 07, 18</li>
    </ul>
</div>

# Creating RESTful APIs With JEC — Part 4: Unit Testing

> Learn how to enable microservice architecture by building and testing RESTful APIs over [Node.js](https://nodejs.org/en/) using the JavaScript Enterprise Container (JEC) specification.

This article is the fourth of a series of five focusing on building RESTful APIs over Node.js by using the "[JavaScript Enterprise Container (_JEC_)](http://jecproject.org/)" specification and its default implementation, GlassCat application server.

In the [previous article](https://dzone.com/articles/creating-restful-apis-with-jec-part-3-dependency-i), we saw how to take benefits of Dependency Injection through the "JavaScript Dependency Injection" API (_JDI_), to make REST services more flexible. In this article, we explain how to test our services by using the "JavaScript Unit Testing API" (_JUTA_) and the different JEC mock APIs.

## Prerequisites

In order to follow this tutorial, we need to install a GlassCat server instance and deploy the lastest version of the project, available at [https://github.com/jec-project/jec-app-samples](https://github.com/jec-project/jec-app-samples).

You must also be familiar with the concepts of Unit Testing and Test-driven development (_TDD_).

Finally, you should read the article "[JUTA: JavaScript Unit Testing API](https://dzone.com/articles/juta-javascript-unit-testing-api)", to understand the TDD architecture as specified by [JEC](http://jecproject.org/).

## Testing JEC Services

In the microservices paradigm, the number of APIs tends to increase considerably comparing to other distributed services architectures. Experience shows us that the development of large APIs comes with a lot of issues when developers do not provide enough documentation or test cases.

Thus, the lack of documentation increases the risk to write duplicated code while missing or unmaintained tests generates regression issues.

[JEC](http://jecproject.org/) specification introduces test-driven development as a main part of its best practices. We encourage that philosophy, especially to fix common concerns encountered by a developer. Writing test cases at the same time as the development process produces more well-designed code; it allows to focus on test coverage and, so, to create smallest and specific code sections:

> "Make each program do one thing well." - [UNIX philosophy](https://en.wikipedia.org/wiki/Unix_philosophy)

But, one more important point is, for companies, to ensure that services always work as expected over the time. This becomes critical regarding dependencies to external libraries and their evolutions.

For those different reasons, [JEC](http://jecproject.org/) provides a set of tools to facilitate Unit Testing integration.

## JavaScript Unit Testing API (JUTA)

### Reminders

In our preceding article "[JUTA: JavaScript Unit Testing API](https://dzone.com/articles/juta-javascript-unit-testing-api)," we have described the API that is provided by [JEC](http://jecproject.org/) to create test suites for any kind of JavaScript, [TypeScript](https://www.typescriptlang.org/) and [Node.js](https://nodejs.org/en/) projects. JUTA simplifies test writings thanks to its class-based approach and the use of [JUnit-like](https://junit.org/junit5/) decorators.

The [Tiger sample application](https://github.com/jec-project/jec-sample-tiger) shows how to use JUTA for non-[JEC](http://jecproject.org/) applications. In this article, we use the API for testing an entire RESTful application, including the business layer, DAOs, and services integration with the JARS API.

Since we have deployed the project from the _microservice_ GPM, the JUTA default implementation, [Tiger framework](https://github.com/jec-project/jec-tiger), is ready-to-use.

We only have to run the following npm command, from the project root folder, to run tests:

```shell
$ npm test
```

### JEC CLI Integration

[JEC CLI](http://jecproject.org/wiki/docs/reference/jec-cli/jec-cli) includes a special command called `create-test-suite` to help developers to easily create test classes based on JUTA.

To use this command, open the terminal at the root of the application folder and run the command with the fully qualified name of the class to test. The class name is built over package syntax _a la Java_. Thus, considering the `MockUserDao` class located in the `service/impl` package, you must fill the command parameters as shown below:

```shell
jec create-test-suite --class=service.impl.MockUserDao
```

The `MockUserDaoTest` class will be created in the test/service/impl directory, as a result of the operation:

```javascript
import { TestSuite, Test, Before, After, Async } from "jec-juta";
import { MockUserDao } from "../../src/service/impl/MockUserDao";
@TestSuite({
  description: "Test the MockUserDao class methods"
})
export class MockUserDaoTest {
  @Before()
  public initTest():void {
    // TODO Auto-generated test method stub
  }
  @After()
  public resetTest():void {
    // TODO Auto-generated test method stub
  }
  @Test({
    description: "test case for the getUsers() method"
  })
  public getUsersTest():void {
    // TODO Auto-generated test method stub
  }
  @Test({
    description: "test case for the getUser() method"
  })
  public getUserTest():void {
    // TODO Auto-generated test method stub
  }
}
```

For more details about JEC CLI, please read the [documentation]((http://jecproject.org/wiki/docs/reference/jec-cli/jec-cli).

## Understanding JEC Mock Modules

### About the JavaScript Connector API for Decorators

[JEC](http://jecproject.org/) metadata behave like an abstraction layer, similar to JEE annotations. Like in JEE, this design allows separating the technical structure that rules an application, from the business logic. This process ensures the portability of applications.

[JEC](http://jecproject.org/) metadata are built on top of [TypeScript](https://www.typescriptlang.org/) decorators, which are themselves an implementation of the coming ES7 decorators. But, where JAVA annotation means abstraction, [TypeScript](https://www.typescriptlang.org/) decorator means implementation. For this reason, [JEC](http://jecproject.org/) includes a low-level API, JavaScript Connector API for Decorators (_JCAD_) that turns [TypeScript](https://www.typescriptlang.org/) decorators into an abstraction layer.

JCAD implements a Service Provider Interface (_SPI_) which deals with JEC metadata to supply concrete implementations at runtime. Concrete implementations are declared either by the [JEC](http://jecproject.org/) implementation (e.g. GlassCat server), or by a developer in bootstrap files (e.g. Sandcat).

However, test scenarios are not executed from a [JEC](http://jecproject.org/) implementation, but independently, to guarantee the respect of the portability concept, as promised by the [JEC](http://jecproject.org/) specification. It means that the JCAD SPI is not able to resolve [JEC](http://jecproject.org/) metadata used in the application during the test phase.

Fortunately, [JEC](http://jecproject.org/) ships with specific Mock modules that provide ghost implementations of [JEC](http://jecproject.org/) metadata, to deal with ES7 Decorators issues.

More details about JCAD can be found [here](http://jecproject.org/wiki/docs/reference/jec-apis/jec-commons/jcad-api).

### Adding Mock Modules to the Tiger Framework

Because the application uses JARS and JDI metadata, we must provide both `JarsMock` and `JdiMock` modules, to prevent [JEC](http://jecproject.org/) exceptions when running test suites.

As we mentioned in our article about [Unit Testing](https://dzone.com/articles/juta-javascript-unit-testing-api), Tiger is the default implementation of the JUTA specification. To declare mock modules in Tiger, we must add their references into the Tiger initialization file, located in the `juta/tiger` package.

Mock references have to be placed in the body of the `beforeProcess()`  method:

```javascript
import { TestStats } from "jec-juta";
import { Tiger, TigerFactory } from "jec-tiger";
import { JarsMock } from "jec-jars-mock";
import { JdiMock } from "jec-jdi-mock";
const factory:TigerFactory = new TigerFactory();
const tiger:Tiger = factory.create();
tiger.beforeProcess(()=>{
  JarsMock.getInstance().createContext();
  JdiMock.getInstance().createContext();
});
tiger.process((stats:TestStats)=> {
  if(stats.error) console.error(stats.error);
  else {
    console.log(
`Test stats:
- number of test suites: ${stats.numTestSuites}
- number of disabled test suites: ${stats.numDisabledTestSuites}
- number of synchronous test cases: ${stats.numTests}
- number of asynchronous test cases: ${stats.numAsyncTests}
- number of disabled test cases: ${stats.numDisabledTests}`
    );
  }
});
```

## Testing JARS Resources With JUTA

Once [JEC](http://jecproject.org/) mock modules included, it is quite easy to write test suites based upon JUTA for testing the web services. Because we cannot access the JDI implementation any longer, we have to instantiate and set the DAO object manually. But, thanks to the `@Before()` and `@After()` decorators, we can initialize and reset all test cases, as shown below:

```javascript
@TestSuite({
  description: "Test the UsersV3 class methods"
})
export class UsersV3Test {
  public users:UsersV3 = null;
  @Before()
  public initTest():void {
    this.users = new UsersV3();
    this.users.dao = new MockUserDao();
  }
  @After()
  public resetTest():void {
    this.users = null;
  }
}
```

Both JarsMock and JdiMock modules replace their respective default implementations, Sandcat and Sokoke, so we can create test cases based upon the mock objects (e.g. `USERS_MODEL`) we have previously defined to build the application:

```javascript
@Test({
  description: "should return the complete list of user registered in the app"
})
public getUsersTest(@Async done:Function):void {
  this.users.getUsers((data:any, err:any)=>{
    expect(data.length).to.equals(USERS_MODEL.length);
    done();
  });
}
```

## Summary

In this article, we have explained how [JEC](http://jecproject.org/) tackles metadata restrictions, due to the ES7 annotations implementation (_Decorators_), by using the JCAD low-level API. We also have introduced the [JEC](http://jecproject.org/) mock frameworks that deal with metadata resolutions at runtime, to provide an efficient solution to write unit testing. Finally, we saw how to structure a test class to create test cases, for REST APIs and microservices, built on top of the JARS and JDI specifications.

In the last article, we will explore deeper the JARS APIs and summarize all the concepts we have talked about in this series.