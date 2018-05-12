# JEC CREATE-TEST-SUITE

## Overview

`jec create-test-suite` creates a new JUTA test suite class.

## Options

<details>
    <summary>class</summary>
    <p><b>- required:</b> yes</p>
    <pre>jec create-test-suite --class=com.project.MyJslet</pre>
    <p>The reference to the class for which to create a test suite.<p>
</details>

## Usage

The `create-test-suite` command must be invoked from the root of the EJP:
- `class` parameter paths refer to classes in the `src` directory
- test classes are generated into the `test` directory, by respecting the original class path hierarchy

The command:

```shell
    jec create-test-suite --class=com.project.HelloWorld
```

will take the `src/com/project/HelloWorld.ts` class:

```typescript
    export class HelloWorld {

        public sayHello():string {
            return "Hello World!";
        }
    }
```

to build the `test/com/project/HelloWorldTest.ts` class as shown below:

```typescript
    import { TestSuite, Test, Before, After, Async } from "jec-juta";
    import { HelloWorld } from "../../../src/com/project/HelloWorld";

    @TestSuite({
    description: "Test the HelloWorld class methods"
    })
    export class HelloWorldTest {

        @Before()
        public initTest():void {
            // TODO Auto-generated test method stub
        }

        @After()
        public resetTest():void {
            // TODO Auto-generated test method stub
        }
        
        @Test({
            description: "test case for the sayHelloTest() method"
        })
        public sayHelloTest():void {
            // TODO Auto-generated test method stub
        }

    }
```