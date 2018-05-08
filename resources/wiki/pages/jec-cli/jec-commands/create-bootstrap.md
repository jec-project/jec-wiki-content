# JEC CREATE-BOOTSTRAP

## Overview

`jec create-bootstrap` creates a new a JEC bootstrap class.

## Options

<details>
    <summary>name</summary>
    <p><b>- required:</b> yes</p>
    <pre>jec create-bootstrap --name=MyBootstrapClass</pre>
    <p>The name of the bootstrap class.<p>
</details>
<details>
    <summary>index</summary>
    <p><b>- required:</b> no</p>
    <pre>jec create-bootstrap --name=MyBootstrapClass --index=2</pre>
    <p>Specifies the position of the bootstrap class instance in the bootstrap files execution queue.<p>
</details>
<details>
    <summary>disabled</summary>
    <p><b>- required:</b> no</p>
    <pre>jec create-bootstrap name=MyBootstrapClass --disabled=true</pre>
    <p>Indicates wheter the bootstrap class instance is ignored at the server start (<code>true</code>), or not (<code>false</code>).<p>
</details>

## Usage

The command:

```shell
    jec create-bootstrap --name=MyBootstrapClass --index=2
```

will build the following class:

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