# `@TestSuite` Decorator

When a class is annotated with `@TestSuite`, the tests in that class will be added to the test runner provided with the current JUTA implementation.

## Test suite description

The JUTA specification indicates that you must provide the description of a test suite.
You describe a test suite by using the `description` property of the `TestSuiteParams` interface:

```typescript
@TestSuite({
  description: "My test suite description"
})
export class MyClassToTest() {}
```

## Ignoring a test suite

If for some reason, you just want a test suite to be ignored, you temporarily disable it.

To ignore a test suite in the JUTA specification, you just set the optional `disabled` property of the `TestSuiteParams` interface to `true`:

```typescript
@TestSuite({
  description: "Test is ignored as a demonstration",
  disabled: true
})
export class MyClassToTest() {}
```

## Test execution order

The JUTA specification does not specify the execution order of test method invocations. Any test runner uses by default a deterministic, but not predictable, order.

By setting the `testOrder` optional property of the `TestSuiteParams` interface, you can specify the execution order of test method invocations. The `testOrder` property accepts all values defined by the `com.jec.juta.utils.TestsSorter` enum:

| Value | Description |
| --- | --- |
| TestSorters.DEFAULT | the test runner uses the default (not predictable) mechanism |
| TestSorters.NAME_ASCENDING | sorts the test methods by method name, in lexicographic order |
| TestSorters.NAME_DESCENDING | sorts the test methods by method name, in inverted lexicographic order |
| TestSorters.ORDER_ASCENDING | Sorts the test methods by using the order parameter of the TestParams interface, from lower to higher value |
| TestSorters.ORDER_DESCENDING | Sorts the test methods by using the order parameter of the TestParams interface, from higher to lower value |


```typescript
@TestSuite({
  description: "Test methods are executed in lexicographic order",
  testOrder: TestSorters.NAME_DESCENDING
})
export class MyClassToTest() {}
```

## Instantiation Policy

Instantiation policy allows developers to specify behavior of test isolation principle. By default, JUTA creates a single instance to execute all tests in a test suite.

By setting the optional `instantiationPolicy` property of the `TestSuiteParams` interface to `InstantiationPolicy.MULTIPLE`, you can force the test runner to create a new instance of the test class for each test.

```typescript
@TestSuite({
  description: "Each test is executed in a new instance of the MyClassToTest class",
  instantiationPolicy: InstantiationPolicy.MULTIPLE
})
export class MyClassToTest() {}
```

For more details, please refer to [Test Classes Instantiation Policy](./docs/reference/jec-apis/juta/test-classes-instantiation-policy).