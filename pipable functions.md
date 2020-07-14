# Pipable Functions

Pipable functions are functions that can be used in `pipe`s.
To be pipable they need to be Unary (accept 1 argument).
They should take an input value (known as the subject), and produce an output value, without mutating the input value.

```typescript

type NumToNum = (subject: number) => number

const inc: NumToNum = subject => subject + 1
const dec: NumToNum = subject => subject - 1

pipe(1, inc, inc, dec, console.log) // 2

```

Pipable functions are often partial, meaning they use a function to capture an initial configuration value,
then return a function that will capture the subject.
Once the subject has been passed in, the function processes to produce the output value.

A configuration value that is intended to be used inline within the pipe function is known as an
inline configuration value.

```typescript

type Add = (configurationValue: number) => (subject: number) => number

const add: Add =
  configurationValue =>
    subject =>
      subject + configurationValue

pipe(100, add(10), add(20), console.log)

```

Pipable functions often have three tiers to the partial function.
We've already seen the inline configuration value tier and the subject tier.
Often there is one before that known as the pre configuration value.
This most often set during the start-up phase of an app, and usually takes some sort of environment / config variable that 
determines how the function will behave in a particular app configuration.

```typescript

type LogInDev = (preconfig: string) => (inlineconfig: string) => (subject: number) => void

const logInDev: LogInDev = env => prefixMessage => numberToLog =>
  env === 'dev' && console.log(prefixMessage + ': ' + numberToLog)

const env = 'dev'
const logInDev_ = logInDev(env)

pipe(100, add(10), add(20), logInDev_(`Here's my number`)) // Only logs in dev

```
