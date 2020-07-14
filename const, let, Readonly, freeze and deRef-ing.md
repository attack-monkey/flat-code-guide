# const, let Readonly, freeze, and deRef-ing

> ⚠️ &nbsp; A lot of the code samples in this doc are written in Typescript

In Flat we use `const` a lot.
`const` makes a variable non-reassignable.
For primitives like strings, booleans, and numbers - this makes them immutable.

```javascript

const str = 'string'
const num = 7
const bool = true

```

To make a primitive mutable, use `let`; but remember Flat encourages as much immutable coding as possible. 

For objects and arrays, while `const` means a variable cannot be re-assigned a new value - it doesn't stop the variable's properties from being updated.

```javascript

const updatable = { name: { first: 'Bob', last: 'McBob' } }
updatable.name.first = 'not bob'

```

🌶️ &nbsp; To lock down an object, use the `freeze` function (from **The Prelude**) which makes all the properties in an object readonly.

To make a full object immutable, freeze also needs to be applied to any arrays or objects within.

```javascript

const p1 = freeze({ name: freeze({ first: 'ben', last: 'clark' })})
const p2 = freeze({ name: freeze({ first: 'ben', last: 'clark' })})
const p1p2 = freeze([p1, p2])

```

The following will cause typescript compilation errors since everything is set to readonly.
If you try anything tricky like assigning to a non-read-only type - javascript will throw a runtime error enforcing immutability.

```javascript

p1.name.first = 'bob'
p1p2.push(p1)

```

In Typescript / Javascript objects are actually only a reference to its properties and methods. When an object is assigned to a variable, it simply assigns these references. If that variable is assiged to another variable or referenced in another obejct - this again only copies the references. This is great as is it uses very little memory, since it just reuses the data from the original object.

```typescript

// Typescript

type MutCat = {
  value: 'cat',
  likesToScratch?: boolean
}
type Cat = Readonly<MutCat>

const obj1: Cat = { value: 'cat' }
let obj2: MutCat = obj1 // references obj1
obj2.likesToScratch = true // but adds a new property

```

It's great ... until you need to update an object's properties.
This updates the original property and therefore all the references of all the objects now point to this new value.
While `freeze` protects against any mutation, there are still times where you'll want to make a copy of an object,
so you can mutate it.

In Flat this is done by dereferenceing an object when you assign it to another variable...

> 🌶️ &nbsp; `deRef` is found in **The Prelude**

```
const obj3: MutCat = deRef(obj2) We have made obj3 mutable and deRef'd it, as to not mutate obj1 or obj2
obj3.likesToScratch = false
```

If we didn't do this then obj3 would be able to mutate obj1 and obj2's properties even though they have been set to readonly!!!

For example

```typescript

// Typescript

const obj4: MutCat = obj1

obj4.likesToScratch = false
console.log(obj1.likesToScratch) oh no obj4 mutated obj1
```

It is almost always better to simply make new versions of an object than mutating an object.

In the above we could simply create an object that references the original, but adding or over-writing anything that we want to be different...

```typescript

// Typescript
const obj5: Cat = { ...obj1, likesToScratch: false }

```

And while freeze is great at locking things down, it can be overkill - and is by simply following immutable patterns almost all
issues just go away.

So `const`, `freeze`, and `deRef` are all extremely important tools in safe fast typescript but there is no substitute for immutable patterns!
