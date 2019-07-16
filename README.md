Intro
======

Flat code is 
- terse
- meaningful
- and safe

Flat code removes complex nested logic and replaces it with terse, yet meaningful syntax with an emphasis on safety.

It has a very functional flavour, but places emphasis on semantic function names.

If another developer can look at a function name and know what it should do without digging further - then that is a good thing. It means the function can now be put into a separate module and be imported in.

## Before we start...

The following code uses some functions from the 'kit' section.
The kit is small library of functions to help write flatter code.

## Basics

Almost everything in your code should be immutable...

```javascript

const a = 1
const b = a + 1
const c = { cat: 'charlie' }
const d = Object.assign({}, c, { dog: 'xander' })
etc.

```

Only use `if`, when calling a function conditonally...

```javascript

if (thing === anotherThing) doSomething()

```

Or if you are returning something straight away...

```javascript

if (thing === anotherThing) return 'some thing'

```

Almost the same thing above can be written with a ternary...

```javascript

thing === anotherThing ? doSomething() : undefined

```

... which may seem a little strange at first since it's now an expression.
It just so happens that it calls `doSomething` along the way to producing a result.

The ternary however, means we can't use the return key word on the right-hand side.
It also means we can't write code blocks.
This is good because we want to avoid both of these things.

If you find yourself using an if statement that mutates values, or contains blocks of code interdispersed between ifs and elses - then there's probably a better way of writing your code.

Don't do this, as it mutates a value

```javascript

let cat = 'bob'
if (thing === anotherThing) cat = 'charlie'

```

Instead...

```javascript

const cat = thing === anotherThing ? 'charlie' : 'bob'

```

As soon as you use a return statement in an if block, you should refactor...

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
  a === b ? 10 : undefined

```

Use flat Ternary Blocks to resolve logic without having to traverse large chunks of if / elses

```javascript

/**
 * Use the following syntax structure
 * condition ? outcome :
 * otherwise ? outcome :
 * finally
 */

const cat = 
  livesIn === house1 ? 'charlie' :
  livesIn === house2 ? 'doug' :
  undefined

```

Some linters such as prettier will change the format of your ternary to...

```javascript

const cat = 
  livesIn === house1
    ? 'charlie' 
    : livesIn === house2
      ? 'doug'
      : undefined

```

It's still easy to read :)

Don't nest logic

```javascript

const cat = 
  livesIn === house1 ? 
    catColor === 'white' ? 'charlie' :
    'bob'
  livesIn === house2 ? 'doug' :
  : undefined

```

Instead abstract to a function or normalise the conditions

```javascript

// normalise

const cat = 
  livesIn === house1 && catColor === 'white' ? 'charlie' : 
  livesIn === house1 ? 'bob' :
  livesIn === house2 ? 'doug' :
  : undefined

```

```javascript

// abstract to function

const getCatFromHouse1 = catColor => 
  catColor === 'white' ? 'charlie' : 'bob'

const cat = 
  livesIn === house1 ? getCatFromHouse1(catColor) :
  livesIn === house2 ? 'doug' :
  : undefined

```

Make functions semantic, so they can be moved to a spearate file.
Most developers that come across the code can see what's happening without having to dig deeper into 
the `getCatFromHouse1` function. They only need to go deeper if they have to.

```javascript

import { getCatFromHouse1 } from '...'

let cat = 
  livesIn === house1 ? getCatFromHouse1(catColor) :
  livesIn === house2 ? 'doug' :
  : undefined

```

Use short circuits to implement default values, when something is falsy

```javascript

const cat = getCat() || 'charlie' // If no cats are returned, make charlie the default

```

Wrap your application with a try / catch and use the exception function to throw errors easily.

```javascript

try {
  ...
  const cat = getCat() || exception('no cats returned') // If no cats are returned, make charlie the default
  ...
} catch (e) {
  // do some thing with errors
}

```

```javascript

const exception = (msg) => { throw new Error(msg); }

```

Async operations require further error handling as they cause another process to spawn outside of the existing try / catch.

```javascript

const doThing = () => {
  try {
    ...
  } catch (e) {
    ...
  }
}

setInterval(doThing, 1000)

```

The above goes for things like setTimeout, setInterval, promises, ajax calls, etc.

Async Await is a little different though.
Rather than spawning a new process - the await keyword pauses a process until a response is recieved.
For this reason, the original try / catch will catch errors just fine.

## Guards

Short circuiting to an exception on something that resolves to falsy is known as a guard.
Use guards to catch runtime exceptions easier.
By making your code fail fast, you'll fix things fast.

```javascript

cat || exception('no cats here')
console.log(`
  If this runs then no exception was thrown
`)

```

The left hand side of the guard is also known as a check.

Here are some basic checks...

```javascript

cat               // truthy check
existy(cat)       // checks that the input is neither null nor undefined 
cat === 'charlie' // value check
['charlie', 'doug'].includes(cat) // Checks that cat is either charlie or doug

// Checks that cat is either charlie or undefined
// This is useful for checking optional arguments in a function
['charlie', 'undefined'].includes(cat)

typeof cat === 'string' // type check

```

You can turn checks into functions...

```javascript

const isApple = input => ['gala', 'fuji'].includes(input) 

```

And check functions into mustBe functions that either return the value back or throw an exception if the policy is not met.

```javascript

const mustBeApple = input => isApple(input) || exception('Input must be a type of apple')

const fruit = 'orange'
mustBeApple(fruit) // throws exception

```

If you're using Typescript or another javascript compiler with static type support, then most checks and guards can be avoided in favour of compile time type checking.

In saying that - there are times where you will not know a type at run time and the above dynamic type checking is still very important.

```typescript

type Apple = 'gala' | 'fuji';
const isApple = (input: Apple): boolean => ['gala', 'fuji'].includes(input);
const mustBeApple = (input: Apple): Apple => isApple(input) || exception('Input must be a type of apple');

const fruit = 'gala';

mustBeApple(fruit); // Now has compilation-time type of Apple, enforced by run time check.

```

Object notation can be written in a safer way...

Where obj.foo.bar would throw an error if obj or obj.foo were undefined, we can instead use

```javascript

safe(obj, 'foo', 'bar') // returns obj.foo.bar or otherwise undefined

```

## A note on object purity

Pure data objects are preferred over objects that contain methods.
This removes much of the need for constructors / classes, etc.
It also means that keeping objects immutable is much easier.

## Kit

Some useful functions for writing leaner code...

```javascript

const assign = (...inputs) => Object.assign({}, ...inputs) // alias for Object.assign

const keys = input => Object.keys(input) // alias for Object.keys

const values = input => Object.values(input) // alias for Object.values

// pipe
const _pipe = (f, g) => (...args) => g(f(...args)) // needed for next line
const pipe = (...fns) => fns.reduce(_pipe) // pipe the output of one function into the input of the next

const existy = input => input !== null && input !== undefined // checks that the input is neither null nor undefined

// throws an error if input doesn't exist
const mustExist = input => existy(input) || (() => { throw new Error('Input does not exist') })() 

const exception = input => { throw new Error(input); } // throws an error

// A safer way of writing object notation.
// Where obj.foo.bar normally throws an exception if obj or obj.foo are undefined, safe(obj, 'foo', 'bar') would simple return undefined.
const safe = (state, ...subs) => subs.reduce(
  (ac, cv) => !ac || !ac[cv] ? undefined : ac[cv]
  , state
)

```

## Example

```javascript

import { run } from '...'

try {
  run()
} catch (e) {
  console.error(e)
}

```

```javascript

// run

import { createPerson } from '...'

export const run = () => {
  const person1 = createPerson('Bob', 'McBobbob', 29)
  const person2 = createPerson('Jenny', 'McBobbob', 30)
  console.log (
    person1.age < person2.age ? 'Person 1 is younger' :
    person2.age < person1.age ? 'Person 2 is younger' :
    'They are both the same age'
  )
}



```

```javascript

// createPerson

import { mustBeString } from '...'
import { mustBeInteger } from '...'

export const createPerson = (firstName, lastName, age) => {
  [firstName, lastName].forEach(mustBeString)
  mustBeInteger(age)
  return { firstName, lastName, age }
}

```

```javascript

// mustBeString

export const mustBeString = 
  input => typeof input === 'string' || exception('Input must be a string')

```

```javascript

// mustBeInteger

export const mustBeInteger =
  input => 
    typeof input === 'number'
    && Math.floor(input) === input 
    || exception('Input must be an integer')

```


