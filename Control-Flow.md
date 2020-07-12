# Control Flow

JS / TS have many ways to control the flow of a program. In readme.md we explained why `if`, `switch`, and imperative loops like `for` are poor ways to control flow and in flat have been designated as banned keywords - only for use in macros.

In place of imperative Control Flow, the following are better alternatives:

- [Ternaries](https://github.com/attack-monkey/flat-code-guide/blob/master/Ternaries.md)
- [Short Circuits](https://github.com/attack-monkey/flat-code-guide/blob/master/Short%20Circuits.md)
- 🌶️  &nbsp; 🧩 &nbsp; Conditionals ( Utilising **The Prelude** + **Maybe Library** )
- For loops, use recursion.

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

const a = getValue() || die('No value to get') // 🌶️  &nbsp; die is found in The Prelude and is used to throw an error.

```

Conditionals use scenarios. When a given scenario is matched, the corresponding function fires. Only the first scenario fires in a given pipe.
Conditionals are often cleaner than ternaries - but require more processing since they are not native.

In this example, it's easy to match an object against another object. Note that the object doesn't have to be completely the same - just that all the parts that it does have, match that of the original object.

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
  otherwise(_ => `not johnny`)
)
```
