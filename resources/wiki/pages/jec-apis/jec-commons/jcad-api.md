# JEC JCAD API

The JavaScript Connector API for Decorators _(JCAD)_ is the cornerstone of JEC portability principles.
It turns TypeScript decorators into an abstraction layer that let developers choose implementations to execute routines depending on injected metadata.

Developers do not have to understand how JCAD is designed to work with JEC. It is a low level API that is used only by framework conceptors to create their own JEC APIs implementation.

## JCAD Architecture

JCAD works as as a Service Provider Interface _(SPI)_. It stores concrete decorator implementations in a global registry, which is manage by the `DecoratorConnectorManager` static class.

JCAD uses a context management process to prevent any concrete decorator implementation overwriting.

JEC implementations have built-in structure to access the JCAD layer and deal with both, decorators and implementation frameworks, declared in Entreprise JavaScript Projects (EJPs).

The following diagram shows how all of these components dialog between them through the JCAD Architecture:

![JCAD Architecture](https://raw.githubusercontent.com/jec-project/jec-wiki-content/master/resources/assets/dist/jec-apis/jec-commons/jcad-architecture.png "JCAD Architecture")

## JCAD Processing

### Principle

JEC decorators are processed at runtime during instantiation phases:

- JCAD detects JEC decorators before instantiation phases
- frameworks use JCAD to perform the autowiring process

### Separation of Concerns

Framework containers are responsible for:

- managing objects depending on JEC decorators (e.g. instantiation)
- injecting metada into managed objects
- establishing communications with the current JEC implementation

JCAD API is responsible for:

- providing context for safe storing of references to JEC decorator implementations
- managing JEC API entry points for injecting decorators implementations

### Performances

Developers can create their own processors to implicitly detect JEC decorators. However, JEC provides access to each built-in container processor to improve performances and save development time.

## JCAD Integration


### JCAD Abstraction

JCAD context declaration is pretty simple. You have to declare a unique connector reference which is used by the API to retrieve an implementation for the current  context.

The you use this context and the JEC decorator implementation to inject concrete  code into the TypeScript decorator function:

```typescript
    const REF:string = LogConnectorRef.CONNECTOR_REF;

    export function log(...args:any[]):Function {

        return function (target:any, key:string, descriptor:PropertyDescriptor):any {
            const ctx:JcadContext = JcadContextManager.getInstance().getContext(REF);
            return DecoratorConnectorManager.getDecorator(REF, ctx)
                                            .decorate(target, key, descriptor, args);
        }
    }
```

### Decorator Objects

JCAD decorators are Plain Old JavaScript Objects _(POJOs)_ that implement the [`Decorator`](http://jecproject.org/docs/jec-commons/api/interfaces/decorator.html) interface.

The [`Decorator`](http://jecproject.org/docs/jec-commons/api/interfaces/decorator.html) interface provides an easy-to-use mechanism, used by JCAD to integrate all JEC decorators implementations, as shown below:

```typescript
    export class Log implements Decorator {
        
        public decorate(target:any, key:string, descriptor:PropertyDescriptor, ...args:any[]):any {
            const originalMethod:Function = descriptor.value;
            descriptor.value = function(...args:any[]):any {
                console.log("The method args are: " + JSON.stringify(args));
                const result:any = originalMethod.apply(this, args);
                console.log("The return value is: " + result);
                return result;
            };
            return descriptor;
        };
    }
```

Such a class would provide logging functionalities exposed via the `@log` decorator:

```typescript
    export class MyClass {

        @log()
        public myMethod(arg:any):string {
            return "Message: " + arg;
        }
    }
```