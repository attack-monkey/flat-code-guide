# Control Flow

JS / TS have many ways to control the flow of a program. In readme.md we explained why `if`, `switch`, and imperative loops like `for` are poor ways to control flow. `for` uses mutation to run loops - which is something that we try to avoid. `if` and `switch` lead to a tangled mix of Conditional Logic, Code Blocks, Mutiple Returns, and Hard-to-track Mutation.

We've been introduced to some better alternatives including:

- Ternaries
- Short Circuits
- 🌶️  &nbsp; 🧩 &nbsp; Conditionals ( Utilising **The Prelude** + **Maybe Library** )

So what's the difference between them and which should you use?

Ternaries and Short Circuits are native and require nothing to get started - however Conditionals are often cleaner and more powerful.

So for simple logic, use ternaries...

```javascript

const a = 10

const b =
  a === 10
    ? 'exactly 10'
    : a < 10
      ? 'less than 10'
      : 'greater than 10'
```

Where ternaries are good for logic that of `return a if x or b if y`, short-circuits are great for things like setting default values...

```javascript

const a = 'garfield'
const b = undefined

const catMaker = cat => cat || 'default cat'

catMaker(a) // garfield
catMaker(b) // default cat

```

Conditionals use scenarios. When a given scenario is matched, the corresponding function fires. Only the first scenario fires in a given pipe.
Conditionals are often cleaner than ternaries - but require more processing since they are not native.

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
