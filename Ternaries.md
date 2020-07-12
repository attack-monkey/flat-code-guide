# Ternaries

Use Ternary Blocks to resolve logic without having to traverse large chunks of if / elses

```javascript

const cat = 
  livesIn === 'house1' ? 'charlie'  :
  livesIn === 'house2' ? 'doug'     :
  undefined

```

The above flat notation ternary is great when dealing with conditions that have the smae shape, and things align nicely into columns.

You can also use indented notation - which works well with more complex logic.

```javascript

const cat = 
  livesIn === 'house1'
    ? 'charlie' 
    : livesIn === 'house2'
      ? 'doug'
      : undefined

```

## Don't nest logic - even with ternaries!

It might look harmless now, but nesting logic = spaghetti code!

```javascript

const cat = 
  livesIn === 'house1'
    ? catColor === 'white'
      ? 'charlie'
      : 'bob'
    : livesIn === 'house2'
      ? 'doug'
      : undefined

```

Instead abstract to a function or normalise the conditions

```javascript

// normalise

const cat = 
  livesIn === 'house1'
    && catColor === 'white'
      ? 'charlie' // charlie lives in house one and has white fur
      : livesIn === 'house1'
        ? 'bob' // bob also lives in house one
        : livesIn === 'house2'
          ? 'doug' // doug lives in house two
          : undefined

```

```javascript

// abstract to function

const getCatFromHouse1 = catColor => 
  catColor === 'white' ? 'charlie' : 'bob'

const cat = 
  livesIn === 'house1' ? getCatFromHouse1(catColor) :
  livesIn === 'house2' ? 'doug'                     :
  undefined

```

```javascript

// abstracting the left hand side to a function

const isWhiteCatInHouse1 = (livesIn, catColor) =>
  livesIn === 'house1'
    && catColor === 'white'

const cat = 
  isWhiteCatInHouse1(livesIn, catColor)
    ? 'charlie' // charlie lives in house one and has white fur
    : livesIn === 'house1'
      ? 'bob' // bob also lives in house one
      : livesIn === 'house2'
        ? 'doug' // doug lives in house two
        : undefined

```

## Switch Ternaries

Switches lead to the same problem as if blocks (blocks of code with mutations, etc.), but they can also really help to visually break things up into easy to read chunks.

Instead of `switches` we can still use ternaries - but with more of a _switch_ flavor.

We call them `ternary switches`

```javascript

const _case = livesIn

return (
  _case === 'house1' ?
    getCatFromHouse1(catColor)
    :
  _case === 'house2' ?
    'doug'
    :
  undefined
)

```

Note that switch ternaries can leak an extra nesting of logic, but they also help to simplify the visuals.

🌶️  &nbsp; 🧩  &nbsp; If you are at the stage of using Switch Tenraries, consider using Conditionals from **The Prelude** + **Maybe Library**
