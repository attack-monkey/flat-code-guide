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

... and other neat things.

Flat is just javaScript (or even better with typeScript), but with some rules and principles and rules applied.
The reult is flat, untangled code.

Flat takes a fp over oo approach - so you won't see any classes or even prototype inheritance here. 

While it takes on fp principles, it doesn't dive anywhere near as deep. 
Flat is primarily concerned with making flat untangled code.

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

## The Old School

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

Flat code removes imperative code from logic blocks, so they are free to simply convey logical outcomes.  

This is firstly done by removing most if / else, and switch blocks, and instead using ternaries.  

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

This is actually very different to if / else / switch which execute blocks of code conditionally based on coditions.
This is where things get messy - and what flat tries to avoid.

Since ternaries are used to return one value from many possibilities - it also means we're only using one return statement in a fuction.

## The Middle Way

Jumping to a completely functional language like haskell might be awesome for purity, but doesn't exactly work on the browser. In saying that Elm is functional and designed for the browser - and there are plenty of other projects out there too. Flat however is just javascript / typescript, written with flat principles in mind. It accepts some risk that developers may bend the rules - but hopes that other more enlightened folk may guide the way. It might just breed some better developers along the way.

## The Rules

Only use `if`, when calling a function conditonally...

```javascript

if (thing === anotherThing) doSomething()

```

Notice how the if doesn't have curly braces... this indicates that what follows is just a single statement, not a block of code.

That's it.
There should never be a return inside an `if` or `switch`.
There should never be a mutation inside an `if` or `switch`.
In fact - you don't need to use `switch` at all!

Don't do this, as it mutates a value

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

As soon as you use a return statement in an if or switch block, you should refactor...

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

Don't use the following...

- switch
- for (or any other imperative looping. They all use mutation - instead use recursive functions and Array.prototype methods)

### Summary

#### Principles

- Reduce mutations - by using immutable operations
- Reduce nested logic - by normalising and abstracting logic
- Reduce the number of returns from functions - by using ternaries, short-circuits, and semantic functions

#### Rules

- Only use `if`, when calling a function conditonally... instead use ternaries, short-circuits, and semantic functions
- Don't use `switch` (see above)
- Don't use `for` (or other imperative looping) - instead use recursive functions and Array.prototype methods

Note - we haven't yet covered
- short-circuits
- semantic functions
- Recursive functions
- Array.prototype methods

It's ok for now to know that they are in your arsenal.
