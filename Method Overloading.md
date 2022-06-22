# The best way to handle overloading args resoloving.

```ts
function resolve<T extends {}>(fn: (o: Partial<T>) => void): Partial<T> {
  const o: Partial<T> = {}
  fn(o)
  return o
}

function test(a: string, c: unknown, d: number): void
function test(a: string, b: number, c: unknown, d: number): void
function test(...args: any[] | [string, unknown, number] | [string, number, unknown, number]): void {
  const { a, b, c, d } = resolve<{
    a: string
    b?: number
    c: unknown
    d: number
  }>((o) => {
    switch (args.length) {
      case 3:
        [o.a, o.c, o.d] = args
        break
      case 4:
        [o.a, o.b, o.c, o.d] = args
        break
      default:
        throw new Error('please check')
    }
  })
  console.log(a, b, c, d)
}

test('string', 3, 4, 5)
test('string', 8, 9)
```
