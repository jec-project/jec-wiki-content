# JEC Projects Architecture

The JEC Architecture, as shown in the following figure, is very flexible; allowing the application architect to split application logic functionally into many separated containers. The presentation layer is implemented using *Domain Containers* and executes in the *EJP Container*:

![JEC Containers Architecture](https://raw.githubusercontent.com/jec-project/jec-wiki-content/master/resources/assets/dist/jec-spec/jec-domains-architecture.png "JEC Containers Architecture")

Each Domain Container is associated with one *HTTP Task*. HTTP Tasks can be used to serve multiple domains, but a good practice is to associate only one Domain Container per HTTP Task.

