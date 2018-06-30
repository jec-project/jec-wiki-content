<div class="jec-jumbotron mat-elevation-z2">
    <ul class="article-permalink">
        <li><span>Title:</span> Creating RESTful APIs With JEC — Part 1: JavaScript API for RESTful Services</li>
        <li><span>Author:</span> Pascal Echemann</li>
        <li><span>Permalink:</span> <a  href="https://dzone.com/articles/creating-restful-apis-with-jec-part-1-javascript-a" title="Creating RESTful APIs With JEC — Part 1: JavaScript API for RESTful Services">https://dzone.com/articles/creating-restful-apis-with-jec-part-1-javascript-a</a></li>
        <li><span>Publish Date:</span> June. 28, 18</li>
    </ul>
</div>

# Creating RESTful APIs With JEC — Part 1: JavaScript API for RESTful Services

> This series shows how to create restful APIs. In part one, we'll learn about JARS and deploying microservices in the JavaScript Enterprise Container specification.

This article is the first of a series focusing on building RESTful APIs over [Node.js](https://nodejs.org/en/) by using the "[JavaScript Enterprise Container](http://jecproject.org/)" ([JEC](http://jecproject.org/)) specification and its default implementation: GlassCat application server.




## Prerequisites

In order to follow this tutorial, you need to install a GlassCat server instance and deploy a microservice project.

Rest assured that the JEC installation process is very simple:

- [GlassCat install tutorial](https://www.youtube.com/watch?v=q9PLW8ghxkU) (YouTube)
- [Microservice project deployment](https://www.youtube.com/watch?v=N_85zglEfe4) (YouTube)

We assume that you are familiar with [TypeScript](https://www.typescriptlang.org/) and have a fully functional [Node.js](https://nodejs.org/en/) environment installed.

The context root used for this project is `rest-api-sample`.

## JARS: JavaScript API for RESTful Services

In our [previous article](https://dzone.com/articles/javascript-enterprise-container-from-java-to-nodej), we talked about JEC easiness regarding building web applications. We saw that one of JEC cornerstones is the way it manages web services by using metadata declarations.

The "JavaScript API for RESTful Services" (_JARS_) specification defines the way that developers create RESTful APIs with JEC metadata. JARS provides a highly robust and testable framework for creating RESTful services, including complex APIs and microservices.

As GlassCat is the default JEC implementation, _Sandcat_ is the default JARS implementation, built over the GlassCat core API. However, the developer does not need to worry about how GlassCat and Sandcat work together. By creating a project from the microservice GPM (_Glasscat Project Model_), a pre-configured environment is ready to use and deploy.

Moreover, [JEC CLI](http://jecproject.org/wiki/docs/reference/jec-cli/jec-cli) ships with built-in tools that simplify the creation of custom resources and test suites.

### Some Words About JEC Portability

JEC specification enforces that all JEC metadata must be decoupled from their implementations. Thus, even if Sandcat is the only JARS implementation for the time, it will be easy to migrate to other coming implementations, depending on developer preferences.

## Deploying Microservices

If you already have created a microservice sample project, you can skip this section.

### Creating a New Project

Once the GlassCat server instance is installed, go to the server folder root and run the command `glasscat start`. Then, open the admin console at `http://localhost:9990/admin/console`. The default login is "_admin_" and password is "_glasscat_."

Click the "_New Project_" button and select the "_Microservice_" project model in the drop-down box.

To finish, fill all required fields with `rest-api-sample` and click on "_Create project_".

The GPM has been deployed in the server workspace. All projects are located in the workspace, which simplifies configuration and highly improves starting time. To open your project in your favorite code editor, go to `<server root>/workspace/rest-api-sample`.

### Loading the Project

At the end of the GPM creation process, you will be redirected to the "_domains_" section.

Click "_List project_" to see the new project appear in the "_Projects In Workspace_" panel.

> Notice that JEC projects are called “domains.”

Open the "_Load Domain_" panel and select the default server (`server1`), then apply changes.

> You must build the project to compile built-in classes, such as bootstrap, sample resource files, etc.

Restart the server instance to make these changes effective: type `ctrl+c` in the command prompt and retype the `glasscat start` command.

Finally, go to the project root, at `http://localhost:8484/rest-apis-sample/`, to check whether everything works fine.

### Coming Features

As mentioned in our [previous article][https://dzone.com/articles/javascript-enterprise-container-from-java-to-nodej], [JEC](http://jecproject.org/) is still under development. We are currently working on three features that will come during summer:

- Refactoring of the admin console by using Angular 6, to improve performance and user experience (_WIP_).
- Integration of all console actions in [JEC CLI](http://jecproject.org/wiki/docs/reference/jec-cli/jec-cli).
- Creation of tools to import/export/share [JEC](http://jecproject.org/) domains.

## Creating Our First Resource

[JEC](http://jecproject.org/) services are located in the `src` package. The microservice GPM created a `resource` folder, which contains a `version` folder. Both of these packages are commonly used to store JARS resources. The GPM also created two sample classes: `Hello` and `TestApi_v_1_0`. These classes will not be used in this tutorial; so, they can be deleted.

Now, we want to create a "`users`" resource to serve app users information through a RESTful web service. Let us use [JEC CLI](http://jecproject.org/wiki/docs/reference/jec-cli/jec-cli) to deal with tedious tasks.

Open a command prompt in the `resource` folder and type the following command:

```shell
  jec create-resource --name="Users" --path="users"
```

This will create a `Users` resource class, as shown below:

```javascript
import { ResourcePath, GET, Exit } from "jec-jars";

/**
 * <code>Users</code>: auto-generated resource.
 */
@ResourcePath("users")
export class Users {

  @GET()
  public sampleEndPoint(@Exit exit: Function): void {
    // TODO Auto-generated method stub
    exit("users resource end point called");
  }
}
```
Compile the project and start the server. You can access to the `users` endpoint by calling the following URL in your browser: `http://localhost:8484/rest-api-sample/users`. The expected response should be the `"users resource end point called"` string.

## Serving Data Through the Resource API

### Figure Out the JEC RESTful API

First, let us have a look at the `@ResourcePath` decorator which specifies a resource path declaration. JARS resource classes are mapped to a unique URL, defined by the application context root and the resource path as follow: `<domain>/<context-root>/<resource-path>`.

Then, replace the `sampleEndPoint()` method by the `getUsers()` method.

One important thing with JARS is that method names do not matter. You just have to design resource class members in terms of routes and HTTP verbs. This is enabled because URL mapping is realized by JARS decorators, such as `@GET`, `@PUT`, `@DELETE`, etc.

Concretely, if you change the method name, new calls to HTTP GET on the `/rest-sample/users` resource will continue to return `"users resource end point called"`.

Another major point is the use of the `@Exit` decorator to expose the callback function. Remember that JARS and [JEC](http://jecproject.org/) are designed to serve data asynchronously. The callback function, specified by the `@Exit` decorator, takes 3 optional parameters:

`data`: the body content of the HTTP response, of the type of `any`
`err`: an object that represents a custom error to be thrown during the process, of the type of `any`
`status`: the status of the HTTP response, of the type of `HttpStatusCode`

### Creating a Data Structure

In this first tutorial, let us use a very simple Data Transfer Object (_DTO_) to represent a user in our system. This first version of the `User` class only exposes the user's name and its access rights:

```javascript
import {Rights} from "./Rights";

export class User {
  public name:string = null;
  public rights:Rights = Rights.NONE;
}
```

Application access rights are defined in the `Rights` _enum_, as shown below:

```javascript
export enum Rights {
  NONE = -1,
  READ = 0,
  WRITE = 1,
  DELETE = 2
}
```

Both files are saved in a new `business` folder that we have created at the root of the `src` folder.

Next step consists in returning an array of users, which represents all users registered in our system, by using the `exit` _callback_ method:

```javascript
import {ResourcePath, GET, Exit} from "jec-jars";
import {User} from "../business/User";
import {Rights} from "../business/Rights";

/**
 * <code>Users</code>: auto-generated resource.
 */
@ResourcePath("users")
export class Users {

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

Then, compile and restart the server to see the result in your browser. When you call the API URL `http://localhost:8484/rest-sample-api/users`, you should see the following response:

```json
  [{"name":"John DOE","rights":0}]
```

## Summary

Herein, we have presented the "JavaScript API for RESTful Services" (_JARS_) specification and described how to create and deploy REST services from scratch, in few clicks.

We have talked about the minimal implementation to create a fully functional microservice. However, JARS provides much more features to easily manage RESTful services with [Node.js](https://nodejs.org/en/) and GlassCat.

In our next article, we will see how the JARS specification helps developers to version REST APIs.

The source code for this article is available at https://github.com/jec-project/jec-app-samples.
