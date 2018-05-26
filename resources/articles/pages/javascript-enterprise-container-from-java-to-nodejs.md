<div class="jec-jumbotron mat-elevation-z2">
    <ul class="article-permalink">
        <li><span>Title:</span> JavaScript Enterprise Container: From Java to Node.js</li>
        <li><span>Author:</span> Pascal Echemann</li>
        <li><span>Permalink:</span> <a  href="https://dzone.com/articles/javascript-enterprise-container-from-java-to-nodej" title="JavaScript Enterprise Container: From Java to Node.js">https://dzone.com/articles/javascript-enterprise-container-from-java-to-nodej</a></li>
        <li><span>Publish Date:</span> May. 22, 18</li>
    </ul>
</div>

# JavaScript Enterprise Container: From Java to Node.js

> We take a look at an open source project, and discuss why it's a flexible dev environment for enterprise apps and microservices using Node.js.

This article introduces the “[JavaScript Enterprise Container](http://jecproject.org/)” (JEC) project for both Java and JavaScript developers. It will show a flexible development environment for building enterprise applications and microservices over [Node.js](https://nodejs.org/en/).

## About JEC

JEC is an easy-to-use open source project that executes JavaScript code server-side to deploy web applications, such as microservices, RESTful APIs, CDN platforms, etc.

JEC has the following advantages over the standard MEAN stack:

- A standardized and portable development environment.
- An efficient alternative to Spring Boot and Java EE.
- An effective solution for JavaScript apps scalability.
- An _isomorphic_ approach for building and serving microservices and Angular apps.

## JEC for JavaScript Developers

JEC is entirely written in [TypeScript](https://www.typescriptlang.org/), which is the best solution to tackle complex JavaScript OOP architecture issues. TypeScript is easy to learn and provides the necessary rigor to create enterprise applications. Moreover, the TypeScript compiler allows the use of advanced techniques, such as Aspect Oriented Programming and Dependency Injection, based on declarative metadata processes.

Declarative metadata are useful for hiding the complexity of concepts and frameworks while increasing the environment's functionalities. For example, JEC URL mapping can be declared by using the `@WebJslet` decorator, without adding any additional configurations or JavaScript code:

```typescript
@WebJslet({
  name: "HelloWorld",
  urlPatterns: ["/say-hello"]
})
export class HelloWorldSvc extends HttpJslet {
  public doGet(req: HttpRequest, res: HttpResponse, exit: Function): void {
    exit(req, res.send("Hello World!"));
  }
}
```

This is a simplistic sample, but imagine how you would handle a hundred different URLs with another framework. Contrary to express-based apps, it becomes easy to structure your web services in a modular approach. JEC natively builds applications that are flexible, scalable, and fully testable.

But what is really interesting for Angular developers is the possibility to develop both parts of the presentation layer with the same language and the same SOA design. This is what we call the “_isomorphic approach_”.

## JEC for Java Developers

JEC offers two major advantage for Java developers who want to use some of the benefits of the Node.js platform:

- It has been developed by following both JEE and Spring Boot concepts.
- It has been designed to remove the complexity of traditional container-based enterprise apps.

Because TypeScript is really close to Java, and JEC architecture inherits directly from JEE and Spring, Java developers have absolutely no difficulties in dealing with the JEC ecosystem.

The following resource sample shows the similarities between [JAX-RS](https://en.wikipedia.org/wiki/Java_API_for_RESTful_Web_Services) and the JavaScript API for RESTful Services (JARS):

```typescript
@ResourcePath("/search")
export class Search {
  @GET({
    route: "/users"
  })
  public getUsers(@Exit exit: Function, @QueryParam name: any, @QueryParam age: any): void {
    let response: string = "searching for users with ";
    if(name) response += "name='" + name + "'" + (age ? " and " : "");
    if(age) response += "age='" + age + "'" ;
    exit(response);
  }
}
```

We also decided to implement a solution similar to [CDI](https://docs.oracle.com/javaee/6/tutorial/doc/giwhl.html) for implementing Dependency Injection. The reason is that CDI appeared to us much friendlier to learn and use than the new Spring concepts.

Keep in mind that JavaScript does not provide built-in processes for creating contexts. Thus, the JavaScript Dependency Injection (JDI) specification is simpler than the original CDI specification. However, JDI is much more powerful than others _JavaScipt / TypeScript_ DI frameworks.

Moreover, JEC ships with many development tools that are familiar to Java developers, such as:

- **GlassCat server:** the JEC reference implementation.
- **Wildcat:** a fully customizable archetype manager, similar to Maven archetypes.
- **JUTA:** a unit testing framework based upon JUnit 4+.
- Many different mock frameworks for testing JEC annotation-based apps.
- Provides the ability to turn on sessions and security capabilities based on the JEE security layer.
- Usage of a JSTL-like templating framework for creating web pages.

For more information on JUTA, please read the following article on DZone: [JUTA: JavaScript Unit Testing API](https://dzone.com/articles/juta-javascript-unit-testing-api).

## Ease of Use

One of the most important parts of JEC is that it has been designed to keep it simple. JEE is known to be complex to use and deploy. So, we took significant efforts to remove complexity, especially regarding installing and configuration.

[Videos in the documentation section](http://jecproject.org/wiki/docs/videos) show how it is easy to install and work with the GlassCat server.

Of course, this is not magic! You will need to learn some basics about configuration files and JEC structure and specs. But, GlassCat is so simple that it is actually DevOps-oriented for all developers!

## JEC and Microservices

When talking about container orchestration, API gateways, and microservices, deployment speed really matters! JEC focuses on servers' starting speed by combining the Node.js runtime capabilities and its specific architecture.

In production mode, a GlassCat server instance that embeds a RESTful microservice, we can start in less than 70 milliseconds _(depending on your host machine config)_.

At the same time, thanks to its non-blocking and asynchronous architecture, JEC is a real alternative to JAVA for reducing infrastructure costs.

Furthermore, GlassCat exclusively runs over its own encapsulated Node.js process. It means that you can emulate complex load balancing architectures by using tools like PM2, which provides stability and flexibility for managing your applications.

## An Open Technology

One difficult aspect when working with traditional JavaScript/TypeScript frameworks is that you often depend on the technology you choose for the lifetime of your project.

But, JEC is a set of standard specifications that lets you choose your preferred implementation, depending on your own considerations, such as business support, performances, community involvement, etc.

For example, the code below shows how to declare the JARS reference implementation to manage the previous `Search` resource class:

```typescript
import { BootstrapScript, Bootstrap, JecContainer } from "jec-commons";
import { SandcatBuilder } from "jec-sandcat";
@Bootstrap()
export class InitApp extends BootstrapScript {
  public run(container:JecContainer):void {
    // Sandcat is the JARS reference implementation:
    new SandcatBuilder().build(container)
                        .process((err:any)=>{});
  }
}
```

To use another JARS implementation, you just have to declare your preferred framework instead of the default one (_Sandcat_).

## Get Started With JEC

The best way to start with JEC is to create and deploy your own application in a GlassCat server instance. So, we have made some videos to show you how to install it in detail and how to create new projects by using GlassCat Project Models (GPMs).

All videos are available from the [JEC Video Channel](https://www.youtube.com/channel/UCA2Z3RDl6NET9qRH-HbNktQ).

To start working with the GlassCat server, you have to install the JEC Command Line Interface (JEC CLI):

```shell
[sudo] npm install jec-cli -g
```

Then, you create a new GlassCat server instance as follows:

```shell
cd <path/to/your/server>
jec glasscat-install
```

Finally, start this new server instance with the `start` command:

```shell
<path/to/your/server> glasscat start
```

Just use `ctrl+C` if you want to kill all processes and stop the server.

## To the JEC Foundation and Beyond!

The best way to sustain the JEC Project long term is to create a foundation or to be integrated into an existing one.

The first step consists in opening the project to a large and active community of users and developers with the following goals:

- Highly improve the project's quality.
- More quickly develop missing functionalities.
- Fix all issues and improve TDD processes.
- Provide an efficient open source alternative to business solutions for cloud computing.
- Be closer to developers' and companies' needs.

The entire JEC ecosystem, including GlassCat and other frameworks, was developed over the course of last year. Just imagine what we could all do together!

## Where Do We Go From Here?

To find more information about the JEC Project, please visit our website at http://jecproject.org/.

All projects are available from our [GitHub repo](https://github.com/jec-project) as well. 

Get informed by following us on [Twitter](https://twitter.com/i/flow/consent_violation_flow).

Video tutorials are available on our [YouTube channel](https://www.youtube.com/channel/UCA2Z3RDl6NET9qRH-HbNktQ). 

**Feel free to contact us from GitHub to get involved in the project.**