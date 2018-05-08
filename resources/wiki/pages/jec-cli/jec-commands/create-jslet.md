# JEC CREATE-JSLET

## Overview

`jec create-jslet` creates a new a JEC jslet class.

## Options

<details>
    <summary>name</summary>
    <p><b>- required:</b> yes</p>
    <pre>jec create-jslet --name=MyJslet --urlPatterns="/my/url"</pre>
    <p>The name of the jslet class.<p>
</details>
<details>
    <summary>urlPatterns</summary>
    <p><b>- required:</b> yes</p>
    <pre>jec create-jslet --name=MyJslet --urlPatterns="/my/url"</pre>
    <p>The patterns associated with the jslet to specify URL mapping.<p>
</details>
<details>
    <summary>template</summary>
    <p><b>- required:</b> no</p>
    <pre>jec create-jslet --name=MyJslet --urlPatterns="/my/url" --template="myTemplate.ejs"</pre>
    <p>The template file associated with this jslet.<p>
</details>

## Usage

The command:

```shell
    jec create-jslet --name=MyJslet --urlPatterns="/my/url"
```

will build the following class:

```typescript
    import {HttpJslet, WebJslet, HttpRequest, HttpResponse} from "jec-exchange";
    import {HttpHeader} from "jec-commons";

    /**
    * <code>MyJslet</code>: auto-generated jslet.
    */
    @WebJslet({
      name: "MyJslet",
      urlPatterns: ["/my/url"]
    })
    export class MyJslet extends HttpJslet {
      
      /**
      * @inheritDoc
      */
      public doGet(req:HttpRequest, res:HttpResponse, exit:Function):void {
        // TODO Auto-generated method stub
        exit(req, res);
      }
    }
```