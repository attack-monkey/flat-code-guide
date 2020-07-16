# File Structure

When writing a flat app, the common approach is to have a file structure that looks something like:

```

node_modules
src
  modules
    Main
      functions
        main.ts
      types
        ...
      index.ts

```

A module usually has a dir with a name corresponding to it's given library name.
Inside it has
- an index.ts file that exposes the library.
- A functions folder with a file per function in the library.
- A types folder, which usually also follows a one type per file setup. Only exportable types need to be here.

So the Main module often often uses Main/functions/main.ts as the entry point of the app, not requiring the index.ts

## [Next: Conditionals](https://github.com/attack-monkey/flat-code-guide/blob/master/Conditionals.md)
