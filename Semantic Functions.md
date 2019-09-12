# Semantic Functions

By making function names meaningful, we can then move them to a spearate file and just import them.
Most developers that come across the code can see what the function should be doing by just the function name.
They only need to go deeper if they have to.

```javascript

import { getCatFromHouse1 } from '...'

let cat = 
  livesIn === 'house1'
    ? getCatFromHouse1(catColor)
    : livesIn === 'house2'
      ? 'doug'
      : undefined

```

## Function Rollups

As above, use functions to remove imperative code.

If the function is only usable by a single file, then codsider creating a folder at the same level as the file. Then create a folder and file for the given function inside. Having a folder for the function, gives room to have an accompanying test.

```

  - my-code.ts
  - my-code-fns
    - some-function
      - some-function.fn.ts

```

Then just import the function in.

```javascript

import { someFunction } from './my-code-fns/some-function/some-function.ts'

```

Usually however we want code to be reusable, so consider 'rolling up' your function into a shared folder.

```

- shared-fns
  - some-function
    - some-function.fn.ts

```
