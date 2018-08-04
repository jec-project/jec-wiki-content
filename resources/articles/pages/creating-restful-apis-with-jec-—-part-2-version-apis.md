<div class="jec-jumbotron mat-elevation-z2">
    <ul class="article-permalink">
        <li><span>Title:</span> Creating RESTful APIs With JEC — Part 2: Version APIs</li>
        <li><span>Author:</span> Pascal Echemann</li>
        <li><span>Permalink:</span> <a  href="https://dzone.com/articles/creating-restful-apis-with-jec-part-2-version-apis">https://dzone.com/articles/creating-restful-apis-with-jec-part-2-version-apis</a></li>
        <li><span>Publish Date:</span> July. 24, 18</li>
    </ul>
</div>

# Creating RESTful APIs With JEC — Part 2: Version APIs

> In this second article, we will take a look at how to version REST APIs using JARS metadata.

This article is the second of a series focusing on building RESTful APIs over [Node.js](https://nodejs.org/en/) by using the "[JavaScript Enterprise Container](http://jecproject.org/)" ([JEC](http://jecproject.org/)) specification and its default implementation: GlassCat application server.

In the [previous article](https://dzone.com/articles/creating-restful-apis-with-jec-part-1-javascript-a), we saw how to use "JavaScript API for RESTful Services" (_JARS_) to build RESTful services over [Node.js](https://nodejs.org/en/). In this second article, we will look at how to version REST APIs using JARS metadata.


## Prerequisites

In order to follow this tutorial, you need to install a GlassCat server instance and deploy the [source code of the project](https://github.com/jec-project/jec-app-samples) we have discussed in the first article of the series.

## How to Version?

All software has to evolve over time, depending on technology changes, user experience feedback, specification updates, etc. In this context, you typically change a software version when you introduce major improvements or break changes.

All distributed APIs should respect this basic rule and provide mechanisms to help developers implement it. But, actually, REST does not provide any specification or guidelines to version APIs.

However, literature proposes two different ways to achieve API versioning: HTTP header and URI path. According to Richardson and Ruby [_Richardson et al.]_, "The simplest way to implement services versioning is to incorporate version information into the resources’ URIs." For this reason, Sandcat (the default JARS implementation) uses URI to distinguish between different versions of a REST API.

## Improving the User Class

In our previous sample application, we created a basic `User` class with only two public members: `name` and `rights`.

To improve our system, we can split the `name`  property into two new members and add the user's email. This new class is shown below:

```javascript
import {Rights} from "./Rights";
export class UserV2 {
  public firstname:string = null;
  public lastname:string = null;
  public email:string = null;
  public rights:Rights = Rights.NONE;
}
```

But now, if we replace the old implementation by the new one, we will break the contract we have previously defined. So, we must provide a new resource to preserve compatibility of existing specifications.

## Creating Version URI Paths

Contrary to JAX-RS and Spring, JARS has its own mechanism to expose different versions of an API. Like other functionalities, versioning is built over [JEC](http://jecproject.org/) metadata. It provides the `@RootPath` decorator to deal with such a complex process.

Even if Sandcatis based upon URI disambiguation, notice that JARS specification does not indicate whether API versioning should use URIs or HTTP headers.

A Sandcat URI version is composed of the context root, the API path, the version path, and the resource name. The combination of the last two parts is called "root path:"

![JEC @RootPath decorator](https://raw.githubusercontent.com/jec-project/jec-wiki-content/master/resources/articles/assets/root-path.png)

The `@RootPath` decorator accepts `RoutePathParams` objects to define a root path. It also has a `ref` property that is used by the resource decorators to specify the root path and to build URL mapping.

Thus, let us use [JEC CLI](http://jecproject.org/wiki/docs/reference/jec-cli/jec-cli) to create a root path for the previous User implementation. Open a command prompt in the `resource/version` folder and type the line below:

```shell
jec create-root-path --name="UsersV1" --ref="users-v1" --path="sample.app"
```

This will create the following class:

```javascript
import {RootPath} from "jec-jars";
/**
 * <code>UsersV1</code>: auto-generated root path.
 */
@RootPath({
  path: "/sample.app",
  ref: "users-v1",
  version: {
    prefix: "v",
    major: 1,
    minor: 0
  }
})
export class UsersV1 {}
```

Thanks to this new root path, we are able to expose resources by using the URI bellow:

```shell
http://localhost:8484/rest-api-sample/sample.app/v1.0/
```

To finish, type this command to create another root path that will expose the second version of our API:

```shell
jec create-root-path --name="UsersV2" --ref="users-v2" --path="sample.app" -–major="2"
```

## Mapping Resources to Root Paths

Mapping resources to version root paths is quite easy with JARS. We just associate the `@RootPathRefs` decorator and the `@ResourcePath` decorator, as shown below:

```javascript
@ResourcePath("users")
@RootPathRefs(["users-v1"])
export class UsersV2 {
  ...
}
```

The `@RootPathRefs` decorator consumes an array of strings where each one refers to a `ref` property, as specified by the `RoutePathParams` decorator. It means that a resource can be mapped to several different versions of an API.

```shell
jec create-resource --name="UsersV1" --path="users" --rootPathRefs="users-v1"
jec create-resource --name="UsersV2" --path="users" --rootPathRefs="users-v2"
```

## Exposing Distinct Versions of the Users Resource

As we mentioned before, we want to expose the initial "users" implementation by using class `UsersV1`, while class `UsersV2` exposes the second one. So, we can simply copy the first implementation of the `getUsers()` method into the `UsersV1` class:

```javascript
@ResourcePath("users")
@RootPathRefs(["users-v1"])
export class UsersV1 {
  @GET()
  public getUsers(@Exit exit:Function):void {
    const result: Array<User> = new Array<User>();
    const user:User = new User();
    user.name = "John DOE";
    user.rights = Rights.READ;
    result.push(user);
    exit(result);
  }
}
```

For the new `getUsers()` method implementation, we have to slightly adapt the code to the `UserV2` class API:

```javascript
@ResourcePath("users")
@RootPathRefs(["users-v2"])
export class UsersV2 {
  @GET()
  public getUsers(@Exit exit:Function):void {
    const result: Array<UserV2> = new Array<UserV2>();
    const user:UserV2 = new UserV2();
    user.firstname = "John";
    user.lastname = "DOE";
    user.email = "jdoe@domain.com";
    user.rights = Rights.READ;
    result.push(user);
    exit(result);
  }
}
```

Now, calls to the first URI version, `http://localhost:8484/rest-api-sample/sample.app/v1.0/users`, don't lead to different results, regarding our previous implementation. But, calls to the second one, `http://localhost:8484/rest-api-sample/sample.app/v2.0/users`, return objects built from the new `users` API:

```json
[{"firstname":"John","lastname":"DOE","email":"jdoe@domain.com","rights":0}]
```

## Summary

This article showed how RESTful API versioning is easy with JARS metadata while providing developer tools to solve regression issues due to APIs contract changes.

But, the quality of REST services does not depend only on regression concerns. In our next article, we will see how the "JavaScript Dependency Injection" (_JeDI_) specification helps developers create testable and more flexible REST APIs.

The source code for this article is available [here](https://github.com/jec-project/jec-app-samples).

## References

[_Richardson et al._]: Richardson L. & Ruby S. (2011). _RESTful Web Services_ (pp. 235-236). Sebastopol: O'Reilly.