# Receiver Functions & more async

A Receiver Function takes an object known as the 'receiver' and a value to apply to the receiver.
The function mutates the receiver using the value.

> :warning: &nbsp; Since the receiver is able to be mutated - care should be taken not to make unwanted references to the object - that may mutate the receiver in an unexpected way!

Since the value is the thing generally being piped through a pipe, this should be the last partial in a Receiver Function.

E.g.

```typescript

const mySubject = 100

const myReceiver = { value: 0 }

const receiverFunction =
  (receiver: { value: number }) =>
    (subject: number) => {
      receiver.value = subject
      return receiver
    }

pipe(mySubject, receiverFunction(myReceiver))

```

## Receiver Functions are great for handling async code...

Receiver objects tend to be like components in an electric circuit, and
they tend to provide concepts like emitters and capacitors.

Emitters allow values to trigger multiple functions at once.
Capacitors store values until some condition is met, where then a function is called.

**Example 1 - Capacitor**

A classic 3 value capacitor that takes an array of values and logs them when 3 are received.
Once the capacitor has logged the values - it clears it's store.

```typescript

// The Capacitor's Condition
const condition = <A>(receiver: { value: A[] }) => receiver.value.length > 2

// The Capactor's Action
const action = <A>(receiver: { value: A[] }) => {
  console.log('***')
  receiver.value.map(a => console.log(a))
  console.log('***')

  receiver.value = []
  return receiver
}

// The Receiver Function
const push = <A>(receiver: { value: A[] }) => (subject: A) => {
  receiver.value.push(subject)
  condition(receiver) && action(receiver) // If condition is met, then fire the action
  return receiver
}

const capacitor = { value: [] as number[] }

// Using the capacitor...

pipe(10, push(capacitor))
pipe(20, push(capacitor))
pipe(30, push(capacitor))
pipe(40, push(capacitor))
pipe(50, push(capacitor))
pipe(60, push(capacitor))

/*
***
10
20
30
***
***
40
50
60
***
*/

// Or using an arrayMap...

const arrayMap = <A, B>(f: (a: A) => B) => (a: A[]) => a.map(f)

pipe(
  [10, 20, 30, 40, 50, 60], arrayMap(push(capacitor))
)

/*
***
10
20
30
***
***
40
50
60
***
*/

```

**Example 2 - Emitter**

An Emitter is just a Record that holds functions
When emit is called - then each function is called with the given value

```typescript

type Emitter<A> = Record<string, (a: A) => any>
  
const emit = <A>(e: Emitter<A>) =>
  (value: A) =>
    Object.keys(e).forEach(key => e[key](value))

const a: Emitter<string> = {}

a.key1 = x => console.log(x + '???')
a.key2 = x => console.log(x + '!!!')

pipe('hello', emit(a))

// hello ???
// hello !!!

```
