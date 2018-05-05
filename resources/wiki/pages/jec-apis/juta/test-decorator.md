# `@Test` Decorator

 When a public void method is annotated with `@Test`, it can be run as a test case.

## Test description

The JUTA specification indicates that you must provide the description of a test case. You describe a test case by using the `description` property of the `TestParams` interface:

```typescript
@Test({
  description: "test is ignored as a demonstration"
})
public myTestMethod():void {}
```

## Ignoring a test case

If for some reason, you just want a test case to be ignored, you temporarily disable it.

To ignore a test case in the JUTA specification, you just set the optional `disabled` property of the `TestParams` interface to `true`:

```typescript
@Test({
  description: "my test case description",
  disabled: true
})
public myTestMethod():void {}
```

## Test execution order

By setting the [`testOrder`](./docs/reference/jec-apis/juta/testsuite-decorator#test-execution-order) property of the `TestSuiteParams` interface, you can specify the execution order of test method invocations whithin a test suite.

Moreover, by setting the `testOrder` property to `TestSorters.ORDER_ASCENDING`, or `TestSorters.ORDER_DESCENDING`, you can specify the execution order for each test. Thus, the `order` optional property of the `TestParams` interface defines the execution order of a test method invocations in the test suite:

```typescript
@Test({
  description: "this test case should be executed in third position",
  order: 3
})
public myThirdTestMethod():void {}

@Test({
  description: "this test case should be executed in first position",
  order: 1
})
public myFirstTestMethod():void {}

@Test({
  description: "this test case should be executed in second position",
  order: 2
})
public mySecondTestMethod():void {}
```

## Repeating tests

Sometimes, you may want to run certain tests repeatedly (e.g. measure performance, diagnose intermittent problems, etc.).
The JUTA specification makes this easy by introducing the `repeat` optional property of the `TestParams` interface.

This parameter specifies the number of repetitions for a `@Test` method:

```typescript
@Test({
  description: "should be repeated 5 times",
  repeat: 5
})
public myTestMethod():void {}
```

