# Simple, Safe JSON

> 🌶️  &nbsp; 🧩   This document uses `compose` from **The Prelude** as well as functions from the Maybe Lib and Promise_ Lib


```typescript

type ExpectedJson = {
  userId: number,
  id: number,
  title: string,
  completed: boolean
}

const runtimeInterfaceJson = {
  userId: $number,
  id: $number,
  title: $string,
  completed: $boolean
}

pipe(
  fetch('https://jsonplaceholder.typicode.com/todos/1'),
  then(response => response.json()),
  then(
    compose(
      match(runtimeInterfaceJson, (json: ExpectedJson) => console.log(`yay - ${ json.title }`)),
      match($unknown, a => console.log(`Unexpected JSON response from API`))
    )
  )
)

```
