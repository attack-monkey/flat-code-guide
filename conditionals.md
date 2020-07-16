# Conditionals

> :hot_pepper: &nbsp; 🧩 &nbsp; This document uses `pipe` from **The Prelude** and introduces the **Maybe lib**

Conditionals are pipable functions that deal with ambiguous types and values.

The Maybe lib provides the `none`, `some`, and `match` conditionals.

By using `some` and `none` it's possible to create paths where in one the 'subject' has a value and 
the other, it doesn't. In the `some` path the compiler knows that the subject has a value.
In the `none` path the compiler treats the subject as undefined.

When using conditionals, only the first path to match will fire.
Once a path has been matched, a special Done object is returned that informs any additional conditionals that a match has already been made.
At the end of a pipe, if the object being returned is a Done object, then it will actually return the result from the matched path.

```typescript

let b: string
b = 'cat'

const c = pipe(
  b,
  some(b => `it's ${b}`),
  none(_ => `it's nothing`)
)

```

The `match` conditional is a powerful pattern matching condtional.

Fistly simple equality matches can be made...

```typescript

const d = 'cat' as unknown

const e = pipe(
  d,
  match('cat', a => `hello kitty`),
  match('dog', a => `hello doggy`),
)

```

Secondly partial objects and arrays can be matched against an object / array.

```typescript

const f = {
  name: {
    first: 'johnny',
    last: 'bravo'
  }
}

pipe(
  f,
  match({ name: { first: 'johnny '} }, a => `matching on first name`)
)

```

Which is particularly useful when used in combination with destructuring

```typescript

pipe(
  f,
  match({ name: { first: 'johnny '} }, ({ name: { first: a }}) => `Hey it's ${a}`)
)

```

Even if the value being piped in is unknown, the pattern matching
is able to verify a pattern at runtime.

Since the compiler already knows the type of the subject if a given path fires, it allows 
for very type-safe coding.

Special runtime interfaces can be used to match against in place of values...

```typescript

pipe(
  f,
  match({ name: { first: $string }}, ({ name: { first: a }}) => `${a} is a string`)
)

```

Runtime interfaces are powerful...

```typescript

const g = [1, 2, 3]

pipe(
  g,
  match($array($number), g => `${g} is an array of numbers`)
)

```

```typescript

pipe(
  g,
  match([1, $number, 3], ([_, a, __]) => `${a} is a number`)
)

```

```typescript

const h = {
  a: [1, 2],
  b: [3, 3, 4],
  c: [1, 5, 99]
}

pipe(
  h,
  match($record($array($number)), a => `A record of arrays of strings - whoa`)
)

```

```typescript

const i = 'cat' as unknown
console.log(
  pipe(
    i,
    match($lt(100), a => `< 100`),
    match($gt(100), a => `> 100`),
    match(100, a => `its 100`),
    match($unknown, a => `no idea ... probably a cat`) // Use $unknown as a catch all
  )
)

```

```typescript

const j = 'cat' as string | number

pipe(
  j,
  match($union([$string, $number]), a => `a is string | number`)
)

```

Runtime interfaces include

$string
$number
$boolean
$array()
$record()
$union([])
$unknown
$lt
$gt
$lte
$gte

In addition to literal matching and partial matching.
