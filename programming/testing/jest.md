# Jest

{% embed url="https://stackoverflow.com/questions/45692456/whats-the-difference-between-tomatchobject-and-objectcontaining" %}

From first answer:

```text
  // objectContaining, with nested object, containing full props/values
  // PASSES
  expect({ position: { x: 0, y: 0 } }).toEqual(expect.objectContaining({
    position: {
      x: expect.any(Number),
      y: expect.any(Number)
    }
  }));

  // objectContaining, with nested object, containing partial props/values
  // FAILS
  expect({ position: { x: 0, y: 0 } }).toEqual(expect.objectContaining({
    position: {
      x: expect.any(Number)
    }
  }));

  // objectContaining, with nested object, also declared with objectContaining, containing partial props/values
  // PASSES
  expect({ position: { x: 0, y: 0 } }).toEqual(expect.objectContaining({
    position: expect.objectContaining({
      x: expect.any(Number)
    })
  }));

  // toMatchObject, with nested object, containing full props/values
  // PASSES
  expect({ position: { x: 0, y: 0 } }).toMatchObject({
    position: {
      x: expect.any(Number),
      y: expect.any(Number)
    }
  });

  // toMatchObject, with nested object, containing partial props/values
  // PASSES
  expect({ position: { x: 0, y: 0 } }).toMatchObject({
    position: {
      x: expect.any(Number)
    }
  });
```



