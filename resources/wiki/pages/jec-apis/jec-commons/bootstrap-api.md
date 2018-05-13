# JEC Bootstrap API

The Bootstrap API allows developers to create classes that interact with JEC containers to initialize their applications.

## JEC containers Initialization

JEC specifies that containers invoke [`BootstrapScript`](http://jecproject.org/docs/jec-commons/api/interfaces/bootstrapscript.html) objects to add specific JEC implemantions to an application.

Developers can create as many bootstrap classes as needed by extending the [`AbstractBootstrapScript`](http://jecproject.org/docs/jec-commons/api/classes/abstractbootstrapscript.html) abstract class. **However, JEC bootstrap process is synchronous.**

JEC containers invoke each bootstrap class depending on its priority to ensure dependency declarations order. The `priority` parameter is optional.

## Bootstrap Classes Declarations

Bootstrap classes declarations can be done by:

- adding information to the `web.json` file
- using the [`@Bootstrap`](http://jecproject.org/docs/jec-commons/api/globals.html#bootstrap) decorator directly in the class

Notice that using config file declaration is faster than using decorator declaration.

### Config File Declaration

To declare a bootstrap class in the `web.json` file, just add a JSON object to the `bootstrap` array field.
Each object must specifies a `path` property that indicates the path to the bootstrap class from the `src` folder. Priorities can be defined by specifying the optional `priority` property:

```json
    {
    "webapp": {
        "name": "my-app",
        ...
        "bootstrap": [
        {
            "path": "bootstrap/InitApp",
            "priority": 3
        }
        ],
        ...
    }
```

### Decorator Declaration

JEC containers automatically detect bootstrap classes that are annotated with the `@Bootstrap` decorator:

```typescript
    import {AbstractBootstrapScript, Bootstrap} from "jec-commons";
    import {DomainContainer} from "jec-glasscat-core";

    /**
     * Runs frameworks initialization scripts.
     */
    @Bootstrap({
      index: 2
    })
    export class MyBootstrapClass extends AbstractBootstrapScript {

      /**
      * @inheritDoc
      */
      public run(container:DomainContainer):void {
        // TODO Auto-generated method stub
      }
    }
```

## Disabling a Bootstrap Class

You can temporarily disable a bootstrap class invocation by setting the `disabled` property to `true` in both, the config file declaration, or the `@Bootstrap` decorator:

```typescript
    import {AbstractBootstrapScript, Bootstrap} from "jec-commons";

    /**
     * Waiting initialization class.
     */
    @Bootstrap({
      disabled: true
    })
    export class MyBootstrapClass extends AbstractBootstrapScript {}
```

## Using JEC CLI

The JEC Command Line Interface provides a specific command for easily  creating bootstrap classes: `jec create-bootstrap`.

Please refer to the [JEC CLI documentation](./docs/reference/jec-cli/jec-commands/create-bootstrap) for more information about the `jec create-bootstrap` command.