# Test Fixtures

> A software test fixture sets up the system for the testing process by providing it with all the necessary code to initialize it, thereby satisfying whatever preconditions there may be. [_[Wikipedia]_](https://en.wikipedia.org/wiki/Test_fixture)

JUTA provides decorators so that test classes can have fixture to setup your test environments.
There are four fixture annotations:
* [`@BeforeClass`](./docs/reference/jec-apis/juta/test-fixtures#beforeclass-decorator)
* [`@AfterClass`](./docs/reference/jec-apis/juta/test-fixtures#afterclass-decorator)
* [`@BeforeAll`](./docs/reference/jec-apis/juta/test-fixtures#beforeall-decorator)
* [`@AfterAll`](./docs/reference/jec-apis/juta/test-fixtures#afterall-decorator)
* [`@Before`](./docs/reference/jec-apis/juta/test-fixtures#before-decorator)
* [`@After`](./docs/reference/jec-apis/juta/test-fixtures#after-decorator)

## `@BeforeClass` decorator

The `@BeforeClass` decorator allows to invoke a **static** method before running all `@Test` methods of a test class. You typically use this decorator to initialize behaviour of components shared by test methods, such as connection to a data base, etc..

```typescript
@TestSuite({
  description: "My test suite",
  instantiationPolicy: InstantiationPolicy.MULTIPLE
})
export class MyClassTest {
  
  @BeforeClass({
    description: "should be run before 'methodToTest'"
  })
  public static initTest():void {}

  @Test({
    description: "should be run after 'initTest'"
  })
  public methodToTest():void {}
}
```

This decorator is ignored when the [`instantiationPolicy`](./docs/reference/jec-apis/juta/testsuite-decorator#instantiation-policy) property is not specified, or set to `InstantiationPolicy.SINGLE`.

## `@AfterClass` decorator

The `@AfterClass` decorator allows to invoke a **static** method after all the `@Test` methods of a test class have been run. You typically use this decorator to reset behaviour of components shared by test methods, such as closing connection to a data base, etc..

```typescript
@TestSuite({
  description: "My test suite",
  instantiationPolicy: InstantiationPolicy.MULTIPLE
})
export class MyClassTest {
  
  @AfterClass({
    description: "should be run after 'methodToTest'"
  })
  public static resetTest():void {}

  @Test({
    description: "should be run before 'resetTest'"
  })
  public methodToTest():void {}
}
```

This decorator is ignored when the [`instantiationPolicy`](./docs/reference/jec-apis/juta/testsuite-decorator#instantiation-policy) property is not specified, or set to `InstantiationPolicy.SINGLE`.

## `@BeforeAll` decorator

The `@BeforeAll` decorator allows to invoke a method before running all `@Test` methods of a test class. You typically use this decorator to initialize behaviour of components shared by test methods, such as connection to a data base, etc..

```typescript
@TestSuite({
  description: "My test suite"
})
export class MyClassTest {
  
  @BeforeAll({
    description: "should be run before 'methodToTest'"
  })
  public initTest():void {}

  @Test({
    description: "should be run after 'initTest'"
  })
  public methodToTest():void {}
}
```

This decorator is ignored when the [`instantiationPolicy`](./docs/reference/jec-apis/juta/testsuite-decorator#instantiation-policy) property is set to `InstantiationPolicy.MULTIPLE`.

## `@AfterAll` decorator

The `@AfterAll` decorator allows to invoke a method after all the `@Test` methods of a test class have been run. You typically use this decorator to reset behaviour of components shared by test methods, such as closing connection to a data base, etc..

```typescript
@TestSuite({
  description: "My test suite"
})
export class MyClassTest {
  
  @AfterAll({
    description: "should be run after 'methodToTest'"
  })
  public resetTest():void {}

  @Test({
    description: "should be run before 'resetTest'"
  })
  public methodToTest():void {}
}
```

This decorator is ignored when the [`instantiationPolicy`](./docs/reference/jec-apis/juta/testsuite-decorator#instantiation-policy) property is set to `InstantiationPolicy.MULTIPLE`.

## `@Before` decorator

The `@Before` decorator allows to invoke a method before running each `@Test` methods of a test class. If you need to initialize the same data for each test, you put that data in instance variables and initialize them in a `@Before` setup method.

```typescript
@TestSuite({
  description: "My test suite"
})
export class MyClassTest {
  
  @Before({
    description: "should be run before 'method1ToTest' and 'method2ToTest'"
  })
  public initTest():void {}

  @Test({
    description: "should be run immediately after 'initTest'"
  })
  public method1ToTest():void {}

  @Test({
    description: "should be run immediately after 'initTest'"
  })
  public method2ToTest():void {}
}
```

## `@After` decorator

The `@After` decorator allows to invoke a method after running each `@Test` methods of a test class. If you need to clean up data after each test, you put that data in instance variables and reset them in a `@After` method.

```typescript
@TestSuite({
  description: "My test suite"
})
export class MyClassTest {
  
  @After({
    description: "should be run after 'method1ToTest' and 'method2ToTest'"
  })
  public resetTest():void {}

  @Test({
    description: "should be run just before 'resetTest'"
  })
  public method1ToTest():void {}

  @Test({
    description: "should be run just before 'resetTest'"
  })
  public method2ToTest():void {}
}
```

## Test invocation example

The call sequence for a class with two test methods and all the fixture decorators is:

1. Call `@BeforeClass` _(`InstantiationPolicy.MULTIPLE`: static init test suite)_
2. Call `@BeforeAll` _(`InstantiationPolicy.SINGLE`: init test suite)_
3. Call `@Before` _(init test data)_
4. Call `@Test` method test1
5. Call `@After` _(reset test data)_
6. Call `@Before` _(init test data)_
7. Call `@Test` method test2
8. Call `@After` _(reset test data)_
9. Call `@AfterAll` _(`InstantiationPolicy.SINGLE`: reset test suite)_
10. Call `@AfterClass` _(`InstantiationPolicy.MULTIPLE`: static reset test suite)_