# The Promise_ Library

> 🌶️  &nbsp; 🧩   This document uses `pipe` from **The Prelude** and introduces the Promise_ lib

The Promise_ Lib provides pipable equivalents of the Promise API

The Promise_ Lib's functions include
- resolve
- then
- parallel (equiv to Promise.all) * note the caveat below

Promise_ Lib also provides the `promise` function for generating Promises rather than `new Promise`.

Examples: 

```typescript

pipe(
  resolve('cat'), // Use resolve to convert a value into a promise
  then(console.log)
)

```

Example of building a wait function from setTimeout and promises

```typescript

const wait = (n: number) => <A>(a: PromiseValue<A>): Promise<A> => promise(resolve => {
  setTimeout(() => resolve(a), n)
})

```
Now you can use wait in a promise pipe...

```typescript

pipe(
  'hello world',
  wait(2000),
  then(greeting => greeting + '!!!'),
  then(console.log)
)

```

```typescript

const waitCat = pipe('cat', wait(2000))
const waitDog = pipe('dog', wait(3000))

pipe(
  [ waitCat, waitDog ],
  parallel, // parallel is equivalent to Promise.all - but has a type of (a: any) => any which means we need to annotate it
  a => console.log(a)
)

```

```typescript

// Parallel takes a tuple of generic values, which can each be a promise or not
type A = [Promise<string>, Promise<string>]
// It then unwraps the promises, while leaving non promises as is.
// It then returns the new array wrapped in a promise.
type B = Promise<[string, string]>
pipe(
  [ waitCat, waitDog ] as A,
  parallel as AtoB<A, B>,
  then(x => console.log(`${x} is of type [string, string]`))
)

```

## [Next: Simple, Safe JSON](https://github.com/attack-monkey/flat-code-guide/blob/master/Simple%20Safe%20JSON.md)
