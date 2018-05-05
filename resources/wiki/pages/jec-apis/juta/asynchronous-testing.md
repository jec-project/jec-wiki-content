# Asynchronous Testing

## `@Async` decorator

You can easily test asynchronous code by using the `@Async` decorator, associated with a callback method. Once the test is complete, you just invoke the callback method.

```typescript
@Test({
  description: "My asynchronous test case description"
})
public myTestMethod(@Async done:Function):void {
  db.findUser(10, (user:User)=> {
    expect(user.name).to.equal("DOE");
    done();
  });
}
```

The `@Async` decorator can be passed as parameter of methods associated with the following decorators:
- [`@Test`](./docs/reference/jec-apis/juta/test-decorator)
- [`@BeforeAll`](./docs/reference/jec-apis/juta/test-fixtures#beforeall-decorator)
- [`@BeforeClass`](./docs/reference/jec-apis/juta/test-fixtures#beforeclass-decorator)
- [`@Before`](./docs/reference/jec-apis/juta/test-fixtures#before-decorator)
- [`@AfterAll`](./docs/reference/jec-apis/juta/test-fixtures#afterall-decorator)
- [`@AfterClass`](./docs/reference/jec-apis/juta/test-fixtures#afterclass-decorator)
- [`@After`](./docs/reference/jec-apis/juta/test-fixtures#after-decorator)

## Timeout parameter

Asynchronous test can be automatically failed when they take too long to complete, depending on each JUTA implementation.
JUTA allows you optionally specify timeout in milliseconds to cause a test method to fail if it takes longer than that number of milliseconds. Thus, you can extend the amount of time to deal with asynchronous tests unpredictable durations.

```typescript
@Test({
  description: "My long test case description",
  timeout: 6000
})
public myTestMethod(@Async done:Function):void {
  db.findUser(10, (user:User)=> {
    expect(user.name).to.equal("DOE");
    done();
  });
}
```

The `timeout` parameter can be used with the following decorators:
- [`@Test`](./docs/reference/jec-apis/juta/test-decorator)
- [`@BeforeAll`](./docs/reference/jec-apis/juta/test-fixtures#beforeall-decorator)
- [`@BeforeClass`](./docs/reference/jec-apis/juta/test-fixtures#beforeclass-decorator)
- [`@Before`](./docs/reference/jec-apis/juta/test-fixtures#before-decorator)
- [`@AfterAll`](./docs/reference/jec-apis/juta/test-fixtures#afterall-decorator)
- [`@AfterClass`](./docs/reference/jec-apis/juta/test-fixtures#afterclass-decorator)
- [`@After`](./docs/reference/jec-apis/juta/test-fixtures#after-decorator)