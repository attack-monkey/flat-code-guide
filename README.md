
# CAUTION: https://github.com/attack-monkey/Lean-Functional-Typescript replaces this guide.

> That's not to say that there are not some useful concepts covered in FLAT.

```
 ___________                                      
|            |              .'.       `````|````` 
|______      |            .''```.          |      
|            |          .'       `.        |      
|            |_______ .'           `.      |      
                                                  
```

Welcome to flat.  
Flat emphasizes the simplification of logic in code by:

- Reducing mutations
- Reducing nested logic
- Reducing the number of returns from functions

Simple Flat is really a set of principles, and doesn't require any additional libraries.
Reading this Readme and the chapter on Control Flow, will help you write much cleaner code.

Advanced Flat also:

- Uses Typescript which adds type-safety into your code!
- :hot_pepper: &nbsp; Uses **The Prelude** and a minimal set of other utility libraries to...
- Organise code into functions and pipes instead of object oriented hierarchies

> Anytime you see a mention to **The Prelude** you'll see a :hot_pepper: &nbsp; at the start of the line. If there is a mention to another library, you'll see a 🧩.

Flat takes an fp (functional programming) over oo (object oriented) approach - so there is an emphasis on organising code into functions and pipes rather than oo based hierarchies.

While Flat takes on fp principles, it doesn't dive anywhere near as deep. 
Flat is primarily concerned with making flat untangled, well organised and safe code.

### Reducing mutations

Almost everything in your code should be immutable...

```javascript

// basic immutable addition
const a = 1
const b = a + 1

// adding keys to objects
const c = { cat: 'charlie' }
const d = { ...c, dog: 'xander' }

// adding array items
const e = [1,2,3]
const f = [ ...e, 4,5,6] // [1,2,3,4,5,6]
const g = [ ...e, ...f] // [1,2,3,1,2,3,4,5,6]
etc.

```

Don't use `var`.  
Rarely use `let`

Mutating variables can lead to unwanted side effects and introduce hard to trace bugs.  
Instead use immutable operations like the ones above, that generate a new state rather than mutating the existing state.

> Caveat: If writing a high compute piece of code that requires lots of recursion or traversing structures to create new immutable structures - then looping and mutation may be worth the loss of purity. Flat explains ways of handling mutation safely for these cases.

### Reducing Nested Logic

Nesting logic is when you put a logic block inside of another logic block. For example an `if` inisde another `if` or a `switch` inside an `if` etc.
Each time you nest some logic, you are tangling code that little bit more.  
It makes code difficult to trace, and notoriously difficult to refactor.  
Instead keep logic flat or 'normalised'.

By moving to immutable patterns ...

```javascript

const a = ...
const b = ...
const c = ... ? ... : ...

```

... and putting code blocks inside reusable functions, then you will find that nested logic no longer becomes a problem.

Welcome to flat coding :)

### Reducing the number of returns from functions

If you have functions that return values at different points in the function, then code becomes hard to trace.  
Especially when combined with variable mutations and nested logic.  
Instead use a single `return` and shift your logic to the right of the `return` using ternaries, switch-ternaries, short circuits, etc.

> multiple `return` statements are a symptom of imperative logic blocks like `if` and `switch`, which are 'Unsafe keywords' in flat.
> If your project already has them, theres not much you can do (unless you refactor), but don't introduce more.

### Organise code into functions and pipes, rather than class / prototype based oo hierarchies.

Advanced Flat organises code into functions, and functions into libraries (libs).

> :hot_pepper: &nbsp;You'll find `pipe` in **The Prelude**

```typescript

// Note that this is using typescript syntax

// functions
const upper = (str: string) => str.toUpperCase()
const lower = (str: string) => str.toLowerCase()

// String_ lib
const String_ = {
  upper,
  lower
}

```

An oo way to think about the above is that libs are classes with static methods. 

Programs are built by using `pipe` to pass a value through a series of functions.

```javascript

pipe('cat', String_.upper, String_.lower)

```

or

```javascript

import { lower, upper } from '...'

pipe('cat', upper, lower)

```

Which is no different than...

```javascript

String_.lower(
  String_.upper(
    'cat'
  )
)

```

... but visualises things as a pipeline rather a nested set of functions.

Pipes can be composed...

```javascript

const myPipe = compose(String_.upper, String_.lower)

```

and then have values piped through them.

```javascript

myPipe('cat')

// or

pipe('cat', myPipe, anotherPipe)

```

A given function can be used by any value that conforms to function's call signature. For example `upper` and `lower` from the above example are usable by any strings. This is different to a method that only works on objects created from a given class.
 
## Unsafe keywords

:warning: The following keywords are considered unsafe because they work against the intention of flat... use with caution!

- `if` - encourages mutation and multiple returns
- `switch` - encourages mutation and multiple returns
- `var` - Is a higher scoped varient of `let` which is no longer needed.

> If your project already has unsafe keywords - well unless you refactor, theres not much you can do, but think first before adding more.

## Loops vs Recursion

Loop constructs like `for` and `while` tend to be faster than recursion and are preferred in high compute situations. However they come at the cost of purity since they mutate variables in order to run a loop. For most cases recursion provides a pure way of achieving the same result with a tiny performance hit.

## Words that probably indicate that you are thinking in an oo way rather than flat / functional ...

- `class`
- `new`
- **`this`**

In most Flat code you won't need classes, prototypes or any other oo constructs. You will however most likely still need to use the `new` keyword when using other libraries.

## [Next: Control-Flow](https://github.com/attack-monkey/flat-code-guide/blob/master/Control-Flow.md)

