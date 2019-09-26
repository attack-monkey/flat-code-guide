# Short Circuits

Short circuits, like ternaries are fantastic at tersely routing logic - but they can often be misunderstood...

Using `&&` is like saying...
If the thing on the left is truthy then assign the value of the thing on the right...

```javascript

// 'one'is on the left of the && and is truthy, so return the value on the right (two)
const num = 'one' && 'two'

// undefined is on the left of the && and is NOT truthy, so return the value on the left (undefined)
const num = undefined && 'two'

// 1 === 1 is on the left of the && and is truthy, so return the value on the right (bob)
const thing = 1 === 1 && 'bob'

// 1 === 2 is on the left of the && and is false (NOT truthy), so return the resulting value on the left (false)
const thing = 1 === 2 && 'bob'

```

Using `||` on the other hand is like saying...
If the thing on the left is falsy then assign the value of the thing on the right...

Use short circuits to implement default values

```javascript

const cat = getCat() || 'charlie' // If no cats are returned, make charlie the default

// If cats exist and there is a selected cat, return the selected cat - otherwise create a new one
const cat =
  catsExist()
    && selectedCat 
    || createNewCat()

```

## A note about truthy

In javascript something is falsy if evaluates to
- undefined
- null
- false
- 0
- NaN
- ''
- ""
- ``

Otherwise it's truthy. Problems can arise when delaing with numbers and expecting 0 to be truthy, as well as dealing with strings and expecting an empty string to still be truthy.

To get around this, create a function called `existy`.

## Existy

`existy` only returns true if something is not undefined or null. 0, empty strings, etc. all return true - since they exist.

```javascript

const existy =
  input =>
    input !== undefined
      && input !== null

```