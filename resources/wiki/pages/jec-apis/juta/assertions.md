# Assertions

JUTA allows you to use any assertion library you wish. This means you can use libraries such as:

* [should.js](https://github.com/shouldjs/should.js) - BDD style shown throughout these docs
* [expect.js](https://github.com/Automattic/expect.js) - `expect()` style assertions
* [chai](http://chaijs.com/) - `expect()`, `assert()` and `should-`style assertions
* [better-assert](http://chaijs.com/) - `expect()`, `assert()` and `should-`style assertions
* [unexpected](http://unexpected.js.org/) - “the extensible BDD assertion toolkit”

## Assertions usage

Using assertions with JUTA is very easy; you just have to import an assertion library and to use it in the body of a `@Test` method, as shown below:

```typescript
import { TestSuite, Test } from "jec-juta";
import { expect } from "chai";

@TestSuite({
  description: "Tests assertions"
})
export class AssertionTest {

  @Test({
    description: "should validate the result of the sum"
  })
  public testEqual():void {
    expect(2).to.equal(1 + 1);
  }
}
```

All assertion libraries can be run with any JavaScript unit testing framework. It means that unit tests written with JUTA will always work, regardless of the JUTA implementation you use.


