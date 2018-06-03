# JEC CREATE-ROOT-PATH

## Overview

`jec create-resource` creates a new a JARS root path class.

## Options

<details>
    <summary>name</summary>
    <p><b>- required:</b> yes</p>
    <pre>jec create-root-path --name="AppV1" --ref="app-v1" --path="sample.app"</pre>
    <p>The name of the root path class.<p>
</details>
<details>
    <summary>ref</summary>
    <p><b>- required:</b> yes</p>
    <pre>jec create-root-path --name="AppV1" --ref="app-v1" --path="sample.app"</pre>
    <p>Specifies the reference used to identify the root path.<p>
</details>
<details>
    <summary>path</summary>
    <p><b>- required:</b> yes</p>
    <pre>jec create-root-path --name="AppV1" --ref="app-v1" --path="sample.app"</pre>
    <p>Indicates the path used to define a specific versioned route.<p>
</details>
<details>
    <summary>prefix</summary>
    <p><b>- required:</b> no</p>
    <pre>jec create-root-path --name="AppV1" --ref="app-v1" --path="sample.app" --prefix="vers"</pre>
    <p>specifies the prefix used to define a unique versioned route. Default value is <code>v</code>.<p>
</details>
<details>
    <summary>major</summary>
    <p><b>- required:</b> no</p>
    <pre>jec create-root-path --name="AppV1" --ref="app-v1" --path="sample.app" --major="2"</pre>
    <p>specifies the major version number used to define a unique versioned route. Default value is <code>1</code>.<p>
</details>
<details>
    <summary>minor</summary>
    <p><b>- required:</b> no</p>
    <pre>jec create-root-path --name="AppV1" --ref="app-v1" --path="sample.app" --minor="3"</pre>
    <p>specifies the minor version number used to define a unique versioned route. Default value is <code>1</code>.<p>
</details>

## Usage

The command:

```shell
    jec create-root-path --name="AppV1" --ref="app-v1" --path="sample.app"
```

will build the following class:

```typescript
    import {RootPath} from "jec-jars";

    /**
     * <code>AppV1</code>: auto-generated root path.
     */
    @RootPath({
        path: "/sample.app",
        ref: "app-v1",
        version: {
            prefix: "v",
            major: 1,
            minor: 0
        }
    })
    export class AppV1 {}
```