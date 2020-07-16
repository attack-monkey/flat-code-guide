## Splitting code into programs with known inputs


> :hot_pepper: &nbsp; 🧩 &nbsp; This document uses `pipe` from **The Prelude** and introduces the **Maybe lib**

All the Maybe functions return a type of Maybe&lt;A&gt; where A is the type of the passed in subject.

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
