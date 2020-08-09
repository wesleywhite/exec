# @bconnorwhite/exec
![dependencies](https://img.shields.io/david/bconnorwhite/exec)
![minzipped size](https://img.shields.io/bundlephobia/minzip/@bconnorwhite/exec)
![typescript](https://img.shields.io/github/languages/top/bconnorwhite/exec)
![npm](https://img.shields.io/npm/v/@bconnorwhite/exec)

Execute commands while keeping flags easily configurable as an object.

```
yarn add @bconnorwhite/exec
```

- Run one or multiple commands
- Easily define arguments and flags
- Run commands in parallel or series
- Inject environment variables
- Automatically pass through output by setting `stdio: "inherit"`

### API
---
#### exec
###### Types
```ts
exec(command: Command) => ChildProcess | SpawnSyncReturns<Buffer>

type Command = {
  command: string;
  args?: string | string[];
  flags?: Flags;
  env?: NodeJS.ProcessEnv;
}

type Flags = {
  [flag: string]: string | boolean | string[] | undefined;
}
```
###### Usage
```js
import exec from "@bconnorwhite/exec";

exec({
  command: "babel",
  args: ["./src"],
  flags: {
    "out-dir": "./build",
    "config-file": "./babel.config.json",
    "watch": true
  }
});

// Equivalent of:
// babel ./src --out-dir ./build --config-file ./babel.config.json --watch
```

---

#### execAll
###### Types
```ts
execAll(
  commands: Command[],
  options: Options
) => Promise<(ChildProcess | SpawnSyncReturns<Buffer>)[]>

type Command = {
  command: string;
  args?: string | string[];
  flags?: Flags;
  env?: NodeJS.ProcessEnv;
}

type Options = {
  env?: NodeJS.ProcessEnv;
  parallel?: boolean;
}
```
###### Usage
```js
import { execAll } from "@bconnorwhite/exec";

execAll([{
  command: "babel",
  args: ["./src"],
  flags: {
    "out-dir": "./build",
    "config-file": "./babel.config.json",
    "watch": true
  }
}, {
  command: "tsc",
  flags: {
    "emitDeclarationOnly": true
  }
}], {
  env: {
    NODE_ENV: "development"
  },
  parallel: false
});
// Equivalent of:
// NODE_ENV=development babel ./src --out-dir ./build --config-file ./babel.config.json --watch && tsc --emitDeclarationOnly
```

---

#### flagsToArray
###### Types
```ts
flagsToArray(flags?: Flags) => string[]

type Flags = {
  [flag: string]: string | boolean | string[] | undefined;
}
```
###### Usage
```js
import { flagsToArray } from "@bconnorwhite/exec";

flagsToArray({
  "out-dir": "./build",
  "config-file": "./babel.config.json",
  "watch": true
});
// ["--out-dir", "./build", "--config-file", "./babel.config.json", "--watch"]
```
