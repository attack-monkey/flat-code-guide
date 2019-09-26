# Errors, Checks, and Guards

## Error Flow

It's important to catch errors, and deal with them appropriately.
This begins with wrapping your application in a try / catch.

Wrap your application with a try / catch and use the die function to throw errors easily. Routing errors to your own function gives you better control over how you want to treat errors.

```javascript

try {
  ...
  const cat = getCat() || die('no cats returned') // If no cats are returned, error out
  ...
} catch (e) {
  // do some thing with errors
}

```

```javascript

const die = (msg) => { throw new Error(msg); }

```

Many async operations require further error handling as they fall outside of the existing try / catch.

```javascript

const doThing = () => {
  try {
    ...
  } catch (e) {
    ...
  }
}

setInterval(doThing, 1000) // won't be caught by the try / catch

```

The above goes for things like setTimeout, setInterval, promises, ajax calls, etc.

Async Await is a little different though.
The original try / catch will catch errors just fine.

## Checks and Guards

Short circuiting to an exception on something that resolves to falsy is known as a guard.
Use guards to catch runtime exceptions easier.
By making your code fail fast, you'll fix things fast.

```javascript

cat || die('no cats here')
console.log(`
  If this runs then no exception was thrown
`)

```

The left hand side of the guard is also known as a check.

Here are some basic checks...

```javascript

cat               // truthy check
existy(cat)       // checks that the input is neither null nor undefined - SEE SHORT CIRCUITS.md for info on existy
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

const mustBeApple = input => isApple(input) || die('Input must be a type of apple')

const fruit = 'orange'
mustBeApple(fruit) // throws exception

```

If you're using Typescript or another javascript compiler with static type support, then most checks and guards can be avoided in favour of compile time type checking.

In saying that - there are times where you will not know a type at run time and the above dynamic type checking is still very important.

```typescript

type Apple = 'gala' | 'fuji'

// With just compilation time check
const fruit: Apple = 'gala'

// Adding runtime checks
const isApple = (input: Apple): boolean => ['gala', 'fuji'].includes(input)
const mustBeApple = (input: Apple): Apple | void => isApple(input) || die('Input must be a type of apple')

const fruit = mustBeApple('gala'); // Now has compilation-time type of Apple, enforced by run time check.

```