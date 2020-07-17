# Simple, Safe JSON

- Create an expected JSON type
- Create a runtime interface representing the type
- Use the fetch API + Promise_ Lib `then` in a pipe to make the fecth call and get the response.
- Use `compose`, which is like `pipe` but returns a function that accepts the subject.
- Use `match` from the Maybe Lib to match against the runtime interface. This will automatically assign the `json` the corresponding type.
- Using `json: ExpectedJson` enforces that the type that the runtime interface produces is compatible with the expected json type.
- Use a `match($unknown...` to catch requests that don't meet the expected JSON

```typescript

type ExpectedJson = {
  "userId": number,
  "id": number,
  "title": string,
  "completed": boolean
}

const runtimeInterfaceJson = {
  "userId": $number,
  "id": $number,
  "title": $string,
  "completed": $boolean
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
