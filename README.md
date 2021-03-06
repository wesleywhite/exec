<div align="center">
  <h1>@bconnorwhite/exec</h1>
  <a href="https://npmjs.com/package/@bconnorwhite/exec">
    <img alt="npm" src="https://img.shields.io/npm/v/@bconnorwhite/exec.svg">
  </a>
  <a href="https://github.com/bconnorwhite/exec">
    <img alt="typescript" src="https://img.shields.io/github/languages/top/bconnorwhite/exec.svg">
  </a>
  <a href='https://coveralls.io/github/bconnorwhite/exec?branch=master'>
    <img alt="Coveralls github" src="https://img.shields.io/coveralls/github/bconnorwhite/exec.svg">
  </a>
  <a href="https://github.com/bconnorwhite/exec">
    <img alt="GitHub stars" src="https://img.shields.io/github/stars/bconnorwhite/exec?label=Stars%20Appreciated%21&style=social">
  </a>
  <a href="https://twitter.com/bconnorwhite">
    <img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/bconnorwhite.svg?label=%40bconnorwhite&style=social">
  </a>
</div>

<br />

> Execute commands while keeping flags easily configurable as an object.

- Run one or multiple commands in parallel or series
- Easily define arguments and flags
- Inject environment variables
- Set silent to ignore output

## Installation

```bash
yarn add @bconnorwhite/exec
```

```bash
npm install @bconnorwhite/exec
```

### API

- [exec](#exec)  
- [execSync](#execsync)  
- [execAll](#execall)  
- [flagsToArgs](#flagstoargs)
- [commandToString](#commandtostring)

##

## exec
### Usage
```js
import exec from "@bconnorwhite/exec";

// Simple usage:
exec("echo", "hello");

// Object usage:
exec({
  command: "babel",
  args: "./src", // for multiple args, use an array instead
  flags: {
    "out-dir": "./build",
    "config-file": "./babel.config.json",
    "w": true // single character flags will be set using a single dash
  }
});

// Equivalent of:
// babel ./src --out-dir ./build --config-file ./babel.config.json -w
```
### Types
```ts
function exec(command: string, args: Args, flags: Flags, { env, silent }: Options): Promise<ExecResult>;
function exec({ command, args, flags, env, silent }: Command): Promise<ExecResult>;

type Command = {
  command: string;
  args?: string | string[];
  flags?: Flags;
  cwd?: string;
  env?: NodeJS.ProcessEnv;
  silent?: boolean;
}

type Flags = {
  [flag: string]: string | boolean | string[] | undefined;
}

type ExecResult = {
  error: string;
  output: string;
  jsonOutput: () => JSONObject | undefined;
  jsonError: () => JSONObject | undefined;
}
```

##

## execSync
### Usage
```js
import { execSync } from "@bconnorwhite/exec";

// Simple usage:
execSync("echo", "hello");

// Object usage:
execSync({
  command: "babel",
  args: "./src", // for multiple args, use an array instead
  flags: {
    "out-dir": "./build",
    "config-file": "./babel.config.json",
    "w": true // single character flags will be set using a single dash
  }
});

// Equivalent of:
// babel ./src --out-dir ./build --config-file ./babel.config.json -w
```
### Types
```ts
function execSync(command: string, args: Args, flags: Flags, { env, silent }: Options): ExecResult;
function execSync({ command, args, flags, env, silent }: Command): ExecResult;

```

##

## execAll
### Usage
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
### Types
```ts
function execAll(
  commands: Command[],
  options: ExecAllOptions
): Promise<ExecResult[]>;

type ExecAllOptions = {
  cwd?: string;
  env?: NodeJS.ProcessEnv; // default, will not override individual commands
  silent?: boolean; // default, will not override individual commands
  parallel?: boolean;
}
```

##

## flagsToArgs

### Usage
```js
import { flagsToArgs } from "@bconnorwhite/exec";

flagsToArgs({
  "out-dir": "./build",
  "config-file": "./babel.config.json",
  "watch": true
});
// ["--out-dir", "./build", "--config-file", "./babel.config.json", "--watch"]
```
### Types
```ts
function flagsToArgs(flags?: Flags): string[];

type Flags = {
  [flag: string]: string | boolean | string[] | undefined;
}
```

flagsToArgs is useful for adding flags that must preceed later arguments. For example:

```ts
import { flagsToArgs } from "@bconnorwhite/exec";

const files = [...];

exec({
  command: "wc",
  args: flagsToArgs({ l: true }).concat(files)
});
// Equivalent of:
// wc -l [FILES]...
```

##

## commandToString

### Usage
```js
import { commandToString } from "@bconnorwhite/exec";

commandToString({
  command: "foo",
  args: ["a", "b"],
  flags: {
    c: true,
    d: "ok",
    long: true
  }
});
// "foo a b -c -d ok --long"
```
### Types
```ts
function commandToString(command: Command): string;

type Command = {
  command: string;
  args?: string | string[];
  flags?: Flags;
  env?: NodeJS.ProcessEnv;
}
```

##

<br />

<h2>Dependencies<img align="right" alt="dependencies" src="https://img.shields.io/david/bconnorwhite/exec.svg"></h2>

- [parse-json-object](https://npmjs.com/package/parse-json-object): Parse a typed JSON object.
- [terminating-newline](https://npmjs.com/package/terminating-newline): Add or remove a terminating newline

##

<br />

<h2>Dev Dependencies<img align="right" alt="David" src="https://img.shields.io/david/dev/bconnorwhite/exec.svg"></h2>

- [@bconnorwhite/bob](https://npmjs.com/package/@bconnorwhite/bob): Bob builds and watches typescript projects.
- [@types/node](https://npmjs.com/package/@types/node): TypeScript definitions for Node.js
- [coveralls](https://npmjs.com/package/coveralls): Takes json-cov output into stdin and POSTs to coveralls.io
- [jest](https://npmjs.com/package/jest): Delightful JavaScript Testing.

##

<br />

<h2>License <img align="right" alt="license" src="https://img.shields.io/npm/l/@bconnorwhite/exec.svg"></h2>

[MIT](https://mit-license.org/)

##
