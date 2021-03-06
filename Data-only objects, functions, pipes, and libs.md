# Data-only objects, functions, pipes, and libs

In Flat, data-only objects are preferred over objects with both properties and methods.
This is not a hard and fast rule, since objects that contain functions are an integral part to flat programming too - but this chapter shows why data-only objects are preferred.

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

This is perfectly fine, but it means that the `says` method is really bound to the object that it exists on. 

Code in Flat is organised into functions, libs, and pipes, rather than methods and classes.

So instead, the `says` method becomes a function that accepts any object that has a `value` property of type string.

**function**

```typescript

// typescript

const says = (a: { value: string }) => console.log('meow')

```

**pipe**

> 🌶️  &nbsp; `pipe` is found in **The Prelude**

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

## [Next: Pipable Functions](https://github.com/attack-monkey/flat-code-guide/blob/master/pipable%20functions.md)
