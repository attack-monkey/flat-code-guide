# Control Flow

JS / TS have many ways to control the flow of a program. In readme.md we explained why `if` and `switch` are less pure ways to control flow and in flat have been designated as Unsafe keywords.

In place of imperative Control Flow, are the following declarative alternatives:

- [Ternaries](https://github.com/attack-monkey/flat-code-guide/blob/master/Ternaries.md)
- [Short Circuits](https://github.com/attack-monkey/flat-code-guide/blob/master/Short%20Circuits.md)
- 🌶️  &nbsp; 🧩 &nbsp; [Conditionals](https://github.com/attack-monkey/flat-code-guide/blob/master/Conditionals.md) ( Utilising **The Prelude** + **Maybe Library** )

So what's the difference between them and which should you use?

Ternaries and Short Circuits are native and require nothing to get started - however Conditionals are often cleaner and more powerful.
Recursion is a method of looping that doesn't require the mutation of a variable.

So for simple if / else logic, use ternaries...

```javascript

const a = 10

const b =
  a === 10
    ? 'exactly 10'
    : a < 10
      ? 'less than 10'
      : 'greater than 10'
```
Short-circuits are great for things like setting default values...

```javascript

const a = 'garfield'
const b = undefined

const catMaker = cat => cat || 'default cat'

catMaker(a) // garfield
catMaker(b) // default cat

```

Short-circuits are also good for guards

```javascript

const a = getValue() || die('No value to get') // 🌶️  die is found in The Prelude and is used to throw an error.

```

Conditionals use scenarios. When a given scenario is matched, the corresponding function fires. Only the first scenario fires in a given pipe.
Conditionals are often cleaner than ternaries - but require more processing since they are not native.

In this example, it's easy to match an object against another object. Note that the object doesn't have to be completely the same - just that all the parts that it does have, match that of the original object. This is known as a pattern match.

```javascript

const person = {
  name: {
    first: 'Johnny',
    last: 'Bravo'
  }
}

pipe(
  person,
  match({ name: first: 'Johnny' }, ({ name: { first: a }}) => `hello ${a}`),
  match($unknown, _ => `not johnny`)
)
```

## Loops

For loops, recursion is 'pure' but takes a performance hit (not noticeable in most situations). Loops like `for` and `while` are faster, but work by mutating an iterable counter - so they are less 'pure'.

## [Next: `const`, `let`, `freeze`, and `deRef`-ing](https://github.com/attack-monkey/flat-code-guide/blob/master/const%2C%20let%2C%20Readonly%2C%20freeze%20and%20deRef-ing.md)
