# Conditionals

> :hot_pepper: &nbsp; 🧩 &nbsp; This document uses `pipe` from **The Prelude** and introduces the **Maybe lib**

Conditionals are pipable functions that deal with ambiguous types and values.

The Maybe lib provides the `none`, `some`, and `match` conditionals.

## `some` and `none`

By using `some` and `none` it's possible to create paths where in one the 'subject' has a value and 
the other, it doesn't. In the `some` path the compiler knows that the subject has a value.
In the `none` path the compiler treats the subject as undefined.

When using conditionals, only the first path to match will fire.
Once a path has been matched, a special Done object is returned that informs any additional conditionals that a match has already been made.
At the end of a pipe, if the object being returned is a Done object, then it will actually return the result from the matched path.

```typescript

let a: string
a = 'cat'

const b = pipe(
  a,
  some(a => `it's ${a}`),
  none(_ => `it's nothing`)
)

```

## `match`

### Literal matching

The `match` conditional is a powerful pattern matching condtional.

Firstly simple equality matches can be made...

```typescript

const a = 'cat' as unknown

const a = pipe(
  a,
  match('cat', a => `hello kitty`),
  match('dog', a => `hello doggy`),
)

```

### Partial Matching & Destructuring

Secondly partial objects and arrays can be matched against an object / array.

```typescript

const a = {
  name: {
    first: 'johnny',
    last: 'bravo'
  }
}

pipe(
  a,
  match({ name: { first: 'johnny '} }, a => `matching on first name`)
)

```

Which is particularly useful when used in combination with destructuring

```typescript

pipe(
  a,
  match({ name: { first: 'johnny '} }, ({ name: { first: b }}) => `Hey it's ${b}`)
)

```

Even if the value being piped in is unknown, the pattern matching
is able to verify a pattern at runtime.

Since the compiler already knows the type of the subject if a given path fires, it allows 
for very type-safe coding.

### Runtime Interfaces

Special runtime interfaces can be used to match against in place of values...

```typescript

pipe(
  a,
  match({ name: { first: $string }}, ({ name: { first: b }}) => `${b} is a string`)
)

```

Runtime interfaces are powerful...

```typescript

const a = [1, 2, 3]

pipe(
  a,
  match($array($number), a => `${a} is an array of numbers`)
)

```

```typescript

pipe(
  a,
  match([1, $number, 3], ([_, b, __]) => `${b} is a number`)
)

```

```typescript

const a = {
  a: [1, 2],
  b: [3, 3, 4],
  c: [1, 5, 99]
}

pipe(
  a,
  match($record($array($number)), a => `A record of arrays of strings - whoa`)
)

```

```typescript

const a = 'cat' as unknown
console.log(
  pipe(
    a,
    match($lt(100), a => `< 100`),
    match($gt(100), a => `> 100`),
    match(100, a => `its 100`),
    match($unknown, a => `no idea ... probably a cat`) // Use $unknown as a catch all
  )
)

```

```typescript

const a = 'cat' as string | number

pipe(
  a,
  match($union([$string, $number]), a => `a is string | number`)
)

```

Runtime interfaces include

- `$string`
- `$number`
- `$boolean`
- `$array()`
- `$record()`
- `$union([])`
- `$unknown`
- `$lt`
- `$gt`
- `$lte`
- `$gte`

In addition to literal matching and partial matching.

## Roll your own Runtime Interfaces

```typescript

const $even =
  {
    runtimeInterface: true,
    test: (a: number) => a % 2 === 0
  } as unknown as number

const $odd =
  {
    runtimeInterface: true,
    test: (a: number) => a % 2 !== 0
  } as unknown as number

console.log(
  pipe(
    101,
    match($even, a => `number is even`),
    match($odd, a => `number is odd`)
  )
) // number is odd

```
A Runtime interface is an object with the property `runtimeInterface: true`.
This tells the `match` function to treat the value as a Runtime Interface.

Primitive Runtime Interfaces have a `type` property, but more complex ones have a `test` function that determines whether a match is being made.

In both `$odd` and `$even` the subject is piped into the test function and a boolean is returned which determines whether or not the subject matches.

Note that the Runtime Interface object is coerced into the expected type should the path match.

## Splitting code into programs with known inputs

All the Maybe functions return a type of Maybe<A> where A is the type of the passed in subject.

So if you do something like...

```typescript

const kitty = 'cat' as string
const maybeKitty = pipe(kitty, some(definitelyKitty => {}))

```

You will notice that `kitty` is a string type.
The use of `some` makes the pipe return a Maybe<string> type.
Inside the `some` path however - `definitelyKitty` is definitely a string.
So the paths have type-certainty however the returned pipe does not.

This gives way to the concept of 'always forward programming'.
Since the inner-path is certain, wouldn't it be nice to pass that certainty on.

Flat supports this concept and allows a path to collect any scope that it requires, to then
pass into a 'new program'. 

A program is a completely modular function that has no reliance on variables other than
those passed directly in or imported in. All imported and passed in variables are of a 
known type, so the program can run with type-certainty.

Programs are just a concept and can simply go into the existing File Structure under a module's functions directory.

Here we are showing program1 and program2 in the same file, but in the real world they represent two diferent files.

```typescript

const program2 = ([a, b, c]: [string, string, string]) => {
  console.log(a, b, c)
}

const program1 = () => {
  const a = 'cat' as unknown
  const b = 'dog' as unknown
  const c = 'monkey'
  const d = 'elephant' // we don't want this in program2 so we won't pass it.

  return pipe(
    [ a, b ],
    match([ $string, $string ], ([a, b]) => program2([a, b, c])),
    match($unknown, _ => { /* no match */ })
  )
}

program1()

```

The beauty of 'always forward programming' is that the path assigns type-certainty to variables but only under a certain runtime condition. Should that condition be met, the path fires and scope is passed to program2 with that type-certainty. Program2 doesn't really care what calls it; it is simply a program waiting to be called. However, when it is called - it has type-certainty.
