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
- Organising code into functions and pipes over object oriented hierarchies

... and other neat things.

Flat is javascript (or even better with typescript), but with some principles, rules, and extensions applied.
The result is flat, untangled code.

For the best and safest experience

- use Typescript which adds type-safety into your code!
- :hot_pepper: &nbsp; 🧩 &nbsp; Use **The Prelude**; a small set of utility functions that introduce powerful ways to organise your code and make it even safer. Many of Flat's principles don't require this or any other library, but Flat becomes a much richer experience with it. Anytime you see a mention to **The Prelude** you'll see a :hot_pepper: &nbsp; at the start of the line. If there is a mention to another library, you'll see a 🧩.

Flat takes an fp over oo approach - so you won't see any classes or even prototype inheritance here. 

While it takes on fp principles, it doesn't dive anywhere near as deep. 
Flat is primarily concerned with making flat untangled, well organised and safe code.

So... the principles

## Principles

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

Each time you nest some logic, you are tangling code that little bit more.  
It makes code difficult to trace, and notoriously difficult to refactor.  
Instead keep logic flat or 'normalised'.

### Reducing the number of returns from functions

If you have functions that return values at different points in the function, then code becomes hard to trace.  
Especially when combined with variable mutations and nested logic.  
Instead use a single `return` and shift your logic to the right of the `return` using ternaries, switch-ternaries, short circuits, etc.

### Organise code into functions and pipes, rather than class / prototype based oo hierarchies.

Instead of classes, objects, and methods, flat emphasises the use of functions and pipes.
Pipes allow a value to be passed into a series of functions. The result of passing the value into the first function is the passed to the next and so on.

> 🌶️  &nbsp; pipe is a function found in **The Prelude** - but code can be written without it.

```javascript
const add20 = add(20)

console.log(
 pipe(10, add20, add20)
) // 50

// Is the same as 

console.log(
 add20(
  add20(
   10
  )
 )
)
```

## Why Flat

In an ol'school imperative program, you may notice that logic blocks have a lot of code in them - including more logic blocks. It's possible to mutate values and return values at multiple points in that code.

Let's play this out...

Each time you use an if-else (logic blocks) you create forks where blocks of code run when they meet specific conditions.  
When you use a logic-block inside another, that's another fork.

```
                x - starting block
              /   \
             if(i) else (e)    
           / | \    | \
          i  e  e   i  e
        / | / \/ \ / \/ 
       i  ei  ei  ei ei    - Oh!
```

Each of those forks are capable of mutating variables, not just in their immediate scope - but also outer scope

They can call functions, which implement there own spaghetti logic.
Those functions can even mutate the same outer-scope variables as before.
A function can also return values from any of it's forks.

Flat gets rid of this forking nightmare!

This is firstly done by removing if / else, and switch blocks, and instead using ternaries, short-circuits, and conditionals.

You may have noticed that curly braces indicate a block of code (usually multiple statements of code within) and if / else / switch are no exception.  
The blocks of code in (if / else / switch )es mean that we have logic (The if / else / switch) part mixing with code blocks (the content that executes within the (if / else / switch)es) so in large programs it's actually really difficult to track down what's even logically happening.

By using ternaries instead of if / else / switch to manage logic - this forces developers to not mix large code blocks into their logic.

Ternaries are used to return one value from many possibilities based on coditions.

```javascript
const b =
  a === 1
    ? 'a equals one'
    : 'a does not equal one'

```

This is actually very different to if / else / switch which execute blocks of code conditionally based on conditions.
This is where things get messy - and what flat tries to avoid.

> 🌶️  &nbsp; 🧩 &nbsp; The **The Prelude** + **Maybe Library** provides a powerful alternative to `if`, `switch`, and `ternaries`.
>
> Example...

```javascript

console.log(
 pipe(
  unknownAnimal,
  match('garfield', a => `${a} is a cat`),
  match('odie', a => `${a} is a dog`),
  some(a => `${a} has some value - just not odie or garfield`,
  none(_ => `no value`)
 )
)

```

## Why Functions and Pipes

Writing a method for a class, and then that method only be able to work with objects of that class is not very DRY.
Yeah you can use DI patterns to connect methods from one class into another - but all of this comes with an overload of complexity.

A function that works with any value of the correct type is very DRY.

```typescript

// Note that this example uses typescript

const valueLogger = (a: { value: string }) => console.log(a.value)

```

Pipes simply allow the result of one function to be passed into the next, into the next, etc. in a process known as function composition.

This process is far more flexible and less complicated than dealing with oo, ( however can take some time to get used to the paradigm shift! ).

## The Rules of Flat

**Only use `if`, when calling a function conditonally...**

```javascript

if (thing === anotherThing) doSomething()

```

Notice how the if doesn't have curly braces... this indicates that what follows is just a single statement.
It can be used to fire off a side-effect such as logging to the console.
It shouldn't be used to mutate something.

The above can be written with a ternary:

```javascript

thing === anotherThing ? doSomething() : undefined

```

Or using a 'short-circuit' pattern

```javascript

thing === anotherThing && doSomething()

```

🧩 &nbsp; Or using a Conditional from the **Maybe Library**

```
pipe(
 thing,
 match(anotherThing, _ => doSomething())
)
```

**Don't mutate anything in an `if` or a `switch`**

```javascript

let cat = 'bob'
if (thing === anotherThing) cat = 'charlie'

```

Instead...

```javascript

const cat =
  thing === anotherThing 
    ? 'charlie' 
    : 'bob'

```

**As soon as you use a return statement in an if or switch block, you should refactor...**

```javascript

const myFunction = () => {
  if (a === b) {
    return 10
  }
}

```

to something more like...

```javascript

const myFunction = () => 
  a === b
    ? 10
    : undefined

```

**Don't use more than one return in a function. This breeds blocks of conditional code.**

**Use `for` with caution (Same goes for any other imperative looping). 

`for` uses mutation, which is something that we try to avoid. The alternative is to use recursive functions and Array.prototype methods). This is a safer, and  cleaner approach than a `for` loop - but if you are processing millions of iterations - `for` is faster. Most of the time though - the recursive option is far better! 

### Summary

#### Principles

- Reduce mutations - by using immutable operations
- Reduce nested logic - by normalising and abstracting logic
- Reduce the number of returns from functions - by using ternaries, short-circuits, and piping through functions

#### Rules

- **Only use `if`, when calling a function conditonally...**
- **Don't mutate anything in an `if` or a `switch`**
- **As soon as you use a return statement in an if or switch block, you should refactor...**
- **Don't use more than one return in a function. This breeds blocks of conditional code.**
- **Don't use `for` (or any other imperative looping. They all use mutation - instead use recursive functions and Array.prototype methods)**

There's plenty more detail in the pages that follow :)

