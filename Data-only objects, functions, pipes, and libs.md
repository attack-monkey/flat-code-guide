# Data-only objects, functions, pipes, and libs

In Flat, data-only objects are preferred over objects with both properties and methods.

Here's an object with a property and method...

```javascript

const a = {
  value: 'cat' as 'cat',
  says() {
    console.log(`${this.value} says meow`)
  }
}

a.says() cat says meow

```

One reason for using data-only objects, is that the deRef function which is used to copy objects while dereferencing the original objects
properties, also drops any associated methods.

> 🌶️  &nbsp; `deRef` is found in **The Prelude**

The following creates a data-only clone of `a`

```javascript

const b = deRef(a)

```

this will cause an error - because `b` has dropped the methods.

```javascript

b.says()

```

The other reason for using data-only objects is that code in Flat is organised into functions, libs, and pipes, rather than methods and classes.

**function**

```typescript

// typescript

const says = (a: { value: string }) => console.log('meow')

```

**pipe**

```javascript

pipe({ value: 'cat' }, says)

```

This allows the `says` function to be completely reusable accross any object that matches the signature of { value: string } and pipe allows us to pass that value through a series of reusable functions.

**libs**

A library (lib) is an object that only contains methods - and those methods are exactly the same as the stand alone functions they reference.
So the above `says` function can be assigned to one or more libs.

```javascript

const MyLib = {
  says
}

pipe({ value: 'garfield' }, MyLib.says)

```

Libs are purely an organisational thing. If you plan on using a number of methods from the same Lib - then it might make sense to just use one import.
