<div class="jec-jumbotron mat-elevation-z2">
    <ul class="article-permalink">
        <li><span>Title:</span> Creating RESTful APIs With JEC — Part 3: Dependency Injection</li>
        <li><span>Author:</span> Pascal Echemann</li>
        <li><span>Permalink:</span> <a  href="https://dzone.com/articles/creating-restful-apis-with-jec-part-3-dependency-i">https://dzone.com/articles/creating-restful-apis-with-jec-part-3-dependency-i</a></li>
        <li><span>Publish Date:</span> August 16, 18</li>
    </ul>
</div>

# Creating RESTful APIs With JEC — Part 3: Dependency Injection

> This in-depth tutorial series shows how to set up RESTful microservices with JEC using the TypeScript language. Part 3 covers dependency injection.

This article is the third of a series of five focusing on building RESTful APIs over [Node.js](https://nodejs.org/en/) by using the "[JavaScript Enterprise Container (_JEC_)](http://jecproject.org/)" specification and its default implementation: GlassCat application server.

In the [previous article](https://dzone.com/articles/creating-restful-apis-with-jec-part-2-version-apis), we saw how to use "JavaScript API for RESTful Services" (_JARS_) to version RESTful APIs. In this third article, we will see how to improve the flexibility of our services by using the "JavaScript Dependency Injection" API (_JDI_, pronounce JeDI)..


## Prerequisites

In order to follow this tutorial, you need to install a GlassCat server instance and deploy the new version of the project, available on [GitHub](https://github.com/jec-project/jec-app-samples).

You also must be familiar with the concepts of Dependency Injection (_DI_) and Inversion of Control (_IoC_).

## Why Dependency Injection?

Because of the promise of fast-and-easy-to-develop code, [Node.js](https://nodejs.org/en/), and [express](https://expressjs.com/) developers often centralize all of the microservice implementations in a single JS file. The equivalent JS file in [JEC)](http://jecproject.org/) is either a jslet, similar to a [servlet in JEE](https://docs.oracle.com/javaee/6/tutorial/doc/bnafd.html), or a JARS resource, like in this tutorial series.

In the previous implementation, we have directly written the business code of our API in the resource file. But, [JEC)](http://jecproject.org/) best practices promote a clear separation between the business logic the data access layer of applications, by using DI.

The "JavaScript Dependency Injection" API (_JDI_) focuses on interfaces to implement DI and enables developers to have several different implementations of a bean.

Interface-based DI is pertinent in microservice development, since it:

- Allows the use of Mocks, to facilitate parallel development in huge units
- Simplifies technical evolutions, such as database migration
- Improves writing of test cases (e.g. jec-jdi-mock, jec-jars-mock...)
- etc.

## A More Complex Application

In this new tutorial, we emulate a database access to serve data. To achieve that, we delegate the responsibility of data exposure to a Data Access Object (_DAO_), implemented as a service.

But, we also need to create a new version of the _user_ resource to introduce database row identifiers in the REST API. So, we must create a `UserV3` _root path_ and _version_, as we described in the [previous tutorial](https://dzone.com/articles/creating-restful-apis-with-jec-part-2-version-apis).

Then, let us extend `UserV2` class to implement the IDs API, by adding the following `UserV3` class to the business package:

```javascript
import {UserV2} from "./UserV2";
export class UserV3 extends UserV2 {
  public readonly id:string = null;
  constructor(id:string) {
    super();
    this.id = id;
  }
}
```

## Creating the Users DAO Interface

### The Basics

This new interface allows us to specify the contract that must be supplied by all DAOs responsible to expose application users. For the purpose of this tutorial, we will only implement two basic methods:

- `getUsers()`, to get the list of all registered users
- `getUser(id:string)`, to retrieve a specific user, depending on its ID

Moreover, we make this interface generic to ensure that it could be used with all versions of our API.

As `UserDao` interface represents a _data access service_; it is declared into the `service` package, at the root of the `src` folder.

### Understanding the JEC `Interface` Construct

The [JEC](http://jecproject.org/) specification has entirely been designed for [TypeScript](https://www.typescriptlang.org/), to let its implementations be compiled in JavaScript and any other OOP languages. One particularity of this ecosystem is that interfaces are not preserved when compiling from TypeScript to JavaScript.

Fortunately, [JEC](http://jecproject.org/) provides the `Interface` construct to preserve reference to a TypeScript interface at runtime. This process is syntactic sugar based upon TypeScript approaches for preserving interfaces references and _typings_.

Thanks to the `Interface` construct, we can simply declare `UserDao` interface as shown below:

```javascript
import {Interface} from "jec-commons";
import {UsersException} from "../exception/UsersException";
export interface UserDao<T> {
  getUsers(result: (data:T[], err:UsersException) => void):void;
  getUser(id:string, result: (data:T, err:UsersException) => void):void;
}
/*
 * UserDao interface declaration.
 */
export const UserDao:Interface = Interface("service.UserDao");
```

Notice that we use the fully qualified class name as parameter of the `Interface` construct, to guarantee the uniqueness of the new interface in the application.

### JEC CLI Integration

The easiest way to create a [JEC](http://jecproject.org/) interface is to use the `create-interface` command provided by [JEC CLI](http://jecproject.org/wiki/docs/reference/jec-cli/jec-cli):

```shell
jec create-interface --name=UserDao --pkg="service.UserDao"
```

If you run the preceding code from the terminal into the `service` folder, you should generate the [JEC](http://jecproject.org/) interface declaration below:

```javascript
import {Interface} from "jec-commons";
/**
 * <code>UserDao</code>: auto-generated interface.
 */
export interface UserDao {
  // TODO Auto-generated interface stub
}
/*
 * UserDao interface declaration.
 */
export const UserDao:Interface = Interface("service.UserDao.UserDao");
```

For more details about [JEC CLI](http://jecproject.org/wiki/docs/reference/jec-cli/jec-cli), please read the [documentation](http://jecproject.org/wiki/docs/reference/jec-cli/jec-cli).

## Creating the _Users_ DAO Implementation

### Presentation

The initial implementation of the _Users_ DAO is a mock that loads data from the `util/MockModel.json` file. This object uses a basic adapter, `UserV3Adapter`, which turns all the JSON information into `UserV3` instances.

### Bean Declaration

[JEC](http://jecproject.org/) specifies the `@Injectable` decorator to add beans to the _application context_. The `@Injectable` decorator accepts objects of the type of `InjectableParams`, that provide all metadata needed to parametrize beans.

You can see below the bean declaration for the Users DAO:

```javascript
@Injectable({
  type: UserDao,
  name: "MockUserDao",
  retention: ["DEV"]
})
export class MockUserDao implements UserDao<UserV3> {
  ...
}
```

Here is the explanation of this bean declaration:

- `type` represents the bean interface used by JDI to let the app resolve itself which implementation to inject, among a list of valid beans.
- `retention` is used by the application to determine which bean to inject, depending on `type` and the current server execution environment. This parameter is optional whether only one implementation is provided for the specified type.
- `name` can be used to inject a bean by using its unique name reference. In this case, `type` and `retention` are optional parameters.

We have used three different parameters for didactic purpose. The following code shows the minimal bean declaration we could use in this tutorial to do the same:

```javascript
@Injectable(UserDao)
export class MockUserDao implements UserDao<UserV3> {
  ...
}
```

### About Bean Scopes

JDI specification allows you to indicate the lifecycle of a bean. [Like in JAVA](https://docs.oracle.com/javaee/6/tutorial/doc/giwhl.html), this process is managed by the bean container and uses bean scope declarations to determine the bean life cycle. JDI specifies four different bean scopes:

- `ScopeType.APPLICATION`: one bean instance exists over the running of the application
- `ScopeType.DEPENDENT`: the container injects a new instance of the bean for each instance of the dependent class
- `ScopeType.SESSION`: the container injects a new instance of the bean for each new user's session
- `ScopeType.REQUEST`: the container injects a new instance of the bean for each HTTP request

The default scope of a bean is `ScopeType.APPLICATION`.

Please note that `ScopeType.SESSION` and `ScopeType.REQUEST` are not implemented yet by GlassCat application servers.

## Bean Concrete Code

The rest of the bean implementation is relatively common in a _CRUD_ context:

```javascript
public getUsers(result:(data:UserV3[], err:any)=>void):void {
  result(this.USER_LIST, null);
}
public getUser(id:string, result:(data:UserV3, err:any)=>void):void {
  const user:UserV3 = this.USER_LIST.find(
    (user: UserV3) => {
  return user.id === id;
    }
  );
  result(user, this.getNotFoundError(id, user));
}
private getNotFoundError(id:string, user:UserV3):UsersException {
  let error:UsersException = null;
  if(!user) {
    error = UsersExceptionBuilder.build(
  `Invalid user ID: ${id}.`, HttpStatusCode.NOT_FOUND
    );
  }
  return error;
}
```

The whole code for `MockUserDao` class is available in the `service/impl` package.

## Users DAO Injection

To inject the _Users_ DAO into `UserV3`, we must create an instance member and use the `@Inject` decorator with a valid bean type:

```javascript
@Inject({ type: UserDao })
public dao:UserDao<UserV3>;
```

Thanks to the JDI IoC process, developer does not need to know how and when bean instances are created. Since the resource class is available at runtime, beans are accessible for processing data.

Thus, the new implementation of the `getUsers()` method would look like this:

```javascript
@GET()
public getUsers(@Exit exit:Function):void {
  this.dao.getUsers((data:UserV3[], err:UsersException)=>{
    exit(data, err, this.getErrorCode(err));
  });
}
```

The `getUsers()` method is quite similar, except that we use a different route to access the user's ID through the `@PathParam` decorator, defined in the JARS specification:

```javascript
@GET({
  route: "/:userId"
})
public getUser(@PathParam userId:string, @Exit exit:Function):void {
  this.dao.getUser(userId, (data:UserV3, err:UsersException)=>{
    exit(data, err, this.getErrorCode(err));
  });
}
```

## Summary

In this article, we have exposed the benefits of using Dependency Injection (_DI_) and Inversion of Control in RESTful services. We also have provided a fully functional DI example based on the "JavaScript Dependency Injection" (_JDI_) specification and GlassCat application server.

In the next article of this series, we will explain how to implement the "[JavaScript Unit Testing API](https://dzone.com/articles/juta-javascript-unit-testing-api)" (_JUTA_) specification to improve the quality of applications based upon JARS and JDI.