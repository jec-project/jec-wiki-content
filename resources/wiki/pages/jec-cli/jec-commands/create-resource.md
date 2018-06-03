# JEC CREATE-RESOURCE

## Overview

`jec create-resource` creates a new a JARS resource class.

## Options

<details>
    <summary>name</summary>
    <p><b>- required:</b> yes</p>
    <pre>jec create-resource --name="Users" --path="users"</pre>
    <p>The name of the resource class.<p>
</details>
<details>
    <summary>path</summary>
    <p><b>- required:</b> yes</p>
    <pre>jec create-resource --name="Users" --path="users"</pre>
    <p>Specifies the path used to declare the resource.<p>
</details>
<details>
    <summary>rootPathRefs</summary>
    <p><b>- required:</b> no</p>
    <pre>jec create-resource --name="Users" --path="users" --rootPathRefs="app-v1"</pre>
    <p>The list of references that specify all enable versions of the resource.<p>
</details>

## Usage

The command:

```shell
    jec create-resource --name="Users" --path="users" --rootPathRefs="app-v1"
```

will build the following class:

```typescript
    import {ResourcePath, RootPathRefs, GET, Exit} from "jec-jars";

    /**
     * <code>Users</code>: auto-generated resource.
     */
    @ResourcePath("users")
    @RootPathRefs(["app-v1"])
    export class Users {
    
        @GET()
        public sampleEndPoint(@Exit exit:Function):void {
            // TODO Auto-generated method stub
            exit("users resource end point called");
        }
    }
```