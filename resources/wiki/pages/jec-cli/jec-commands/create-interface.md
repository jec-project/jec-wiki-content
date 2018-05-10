# JEC CREATE-INTERFACE

## Overview

`jec create-interface` creates a new a JEC interface.

## Options

<details>
    <summary>name</summary>
    <p><b>- required:</b> yes</p>
    <pre>jec create-interface --name=MyInterface</pre>
    <p>The name of the interface.<p>
</details>
<details>
    <summary>pkg</summary>
    <p><b>- required:</b> no</p>
    <pre>jec create-interface --name=MyInterface --pkg="com.domain.services"</pre>
    <p>The name of the package for this interface.<p>
</details>

## Usage

The command:

```shell
    jec create-interface --name=MyInterface --pkg="com.domain.services"
```

will build the following interface:

```typescript
    import {Interface} from "jec-commons";

    /**
     * <code>MyInterface</code>: auto-generated interface.
     */
    export interface MyInterface {
    // TODO Auto-generated interface stub
    }

    /*
    * MyInterface interface declaration.
    */
    export const MyInterface:Interface = Interface("com.domain.services.MyInterface");
```