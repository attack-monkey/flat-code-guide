```
 ______   __         ______     ______  
/\  ___\ /\ \       /\  __ \   /\__  _\ 
\ \  __\ \ \ \____  \ \  __ \  \/_/\ \/ 
 \ \_\    \ \_____\  \ \_\ \_\    \ \_\ 
  \/_/     \/_____/   \/_/\/_/     \/_/ 

```

-------------------------------------------------------------

Welcome to flat.  
Flat emphasizes the simplification of logic in code by:

- Reducing mutations
- Reducing nested logic
- Reducing the number of returns from functions
- Organising code into functions and pipes instead of object oriented hierarchies

Flat is javascript (or even better with typescript), but with some principles, and rules applied.
The result is flat, untangled code.

**For the best and safest experience:**
- use Typescript which adds type-safety into your code!
- Use **The Prelude**; a small set of utility functions that introduce powerful ways to organise your code and make it even safer. Many of Flat's principles don't require this or any other library, but Flat becomes a much richer experience with it. Anytime you see a mention to **The Prelude** you'll see a :hot_pepper: &nbsp; at the start of the line. If there is a mention to another library, you'll see a 🧩.

Flat takes an fp (functional programming) over oo (object oriented) approach - so you won't see any classes or even prototype inheritance here. 
Instead code is organised into functions and pipes.

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

... and putting code blocks inside reusable functions, nested logic will be a thing of the past.

### Reducing the number of returns from functions

If you have functions that return values at different points in the function, then code becomes hard to trace.  
Especially when combined with variable mutations and nested logic.  
Instead use a single `return` and shift your logic to the right of the `return` using ternaries, switch-ternaries, short circuits, etc.

> multiple `return` statements are a symptom of imperative logic blocks like `if` and `switch`, which are 'banned keywords' in flat.
> If your project already has them - well unless you refactor, theres not much you can do, but don't introduce more.

### Organise code into functions and pipes, rather than class / prototype based oo hierarchies.

Flat organises code into functions, and functions into libraries (libs).

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
 
## Banned keywords

There are a lot of constructs in javascript, which are deemed unsafe in flat. The following keywords are forbidden because they work against the intention of flat...

- `if`
- `switch`
- `for`
- `var`
- `while`
- `class`
- `new`
- `this`

> If your project already has banned keywords - well unless you refactor, theres not much you can do, but don't introduce more.

## [Next: Control-Flow](https://github.com/attack-monkey/flat-code-guide/blob/master/Control-Flow.md)

