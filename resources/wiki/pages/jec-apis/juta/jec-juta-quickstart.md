# Quick Start

This small example shows you how to write a unit test. You need to have a functional [TypeScript](https://www.typescriptlang.org/) environment installed:

* [Node.js](https://nodejs.org/)
* [TypeScript](https://www.typescriptlang.org/)
* [Visual Studio Code](https://code.visualstudio.com/)

**We assume that you are familiar with Node.js, npm and TypeScript.**

## Preparation

Create a new folder `juta-example` and run the npm `init` command:

```bash
$ npm init
```

Install the JUTA module:

```bash
$ npm install jec-juta --save-dev
```

In this example, we will use [Chai.js](http://chaijs.com/) as assertion library for our tests:

```bash
$ npm install chai --save-dev
$ npm install @types/chai --save-dev
```

### JUTA Decorators

JUTA test classes are written in TypeScript and are based upon decorators defined by the JUTA specification.
To use JUTA decorators, you must enable the `experimentalDecorators` option of the TypeScript compiler in your `tsconfig.json` file:

```json
{
  "compilerOptions": {
     "target": "ES6",
      "experimentalDecorators": true
  }
}
```

## Create the class under test

Create a new `src` folder with a `Calculator.ts` file and copy the following code to this file:

```typescript
export class Calculator {
  public evaluate(expression:string):number {
    let sum:number = 0;
    let tokens:string[] = expression.split("+");
    tokens.forEach((summand:string)=> { sum += parseFloat(summand); });
    return sum;
  }
}
```

Now compile this class; the TypeScript compiler creates a file `Calculator.js`.

## Create a test

Create a new `test` folder with a `CalculatorTest.ts` file and copy the following code to this file:

```typescript
import { TestSuite, Test } from "jec-juta";
import { expect } from "chai";
import { Calculator } from "../src/Calculator";

@TestSuite({
  description: "Test all methods of the Calculator.ts class"
})
export class CalculatorTest {
  @Test({
    description: "should evaluate the specified expression and return the sum"
  })
  public evaluatesExpression():void {
    let calculator:Calculator = new Calculator();
    let sum:number = calculator.evaluate("1+2+3");
    expect(sum).to.equal(6);
  }
}
```

Compile the test; the TypeScript compiler creates a file `CalculatorTest.js`.

## Run the test

Before running tests, you must install a JUTA implemetation, such as [Tiger](https://github.com/jec-project/jec-tiger).

```bash
$ npm install jec-tiger --save-dev
$ npm install mocha --save-dev
```

Once you have [installed the JUTA implementation](https://github.com/jec-project/jec-tiger#tiger-framework-initialization), run the test from the command line.

```bash
$ npm test
```

The output is:

```bash
[06/11/17 10:30:23.293][TIGER] INFO: Tiger start
[06/11/17 10:30:23.302][TIGER] INFO: new source path added: test
[06/11/17 10:30:23.303][TIGER] INFO: new watcher set
[06/11/17 10:30:23.303][TIGER] INFO: new processor added: TigerAutowireProcessor
[06/11/17 10:30:23.304][TIGER] INFO: lookup process start
[06/11/17 10:30:23.453][TIGER] INFO: autowired test suite detected: source file='CalculatorTest'
[06/11/17 10:30:23.454][TIGER] INFO: lookup process complete
[06/11/17 10:30:23.456][TIGER] INFO: test suite run: CalculatorTest
[06/11/17 10:30:23.460][TIGER] INFO: test suite complete
[06/11/17 10:30:23.462][TIGER] INFO: Tiger complete

Test all methods of the Calculator.ts class
  √ evaluatesExpression: should evaluate the specified expression and return the sum

  1 passing (15ms)
```