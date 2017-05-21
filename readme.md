## json-ts

> Automatically generate Typescript Definition files based on JSON input. This allows
 a frontend to verify it's using a backend correctly & optionally tie it into a CI environment.
 
![json-ts](https://cdn.rawgit.com/shakyShane/json-ts/37ce9b2b/json-ts2.gif)
 
## Install
```bash
npm install -g json-ts
```

## Usage (CLI)
Note: only stdin is supported right now, which requires the --stdin flag. 
Later I will add support for Windows, reading data from disk/network requests

```bash
curl https://jsonplaceholder.typicode.com/posts/1 | json-ts --stdin
```

... produces the following: 

```ts
interface IRootObject {
  userId: number;
  id: number;
  title: string;
  body: string;
}
```

## Usage (API)

```bash
npm install json-ts --save-dev
```

```js
const { json2ts } = require('json-ts');
const json = `
{
    "name": "Shane"
}
`;
console.log(json2ts(json))
```

... produces the following:

```ts
interface IRootObject {
  name: string;
}
```

For more examples, see the [Tests](https://github.com/shakyShane/json-ts/tree/master/__tests__) 

## TODO:

### options

- [ ] Allow choice of `I` prefix on interface names
- [ ] Allow naming of RootObject
- [ ] Allow choice of spaces/tabs

### Core
- [x] support array types
- [x] support boolean types
- [x] support null types
- [x] PascalCase as default for all interface names
- [x] de-dupe interfaces (it's dumb atm, POC)
- [x] de-dupe interfaces where propname differs, but members are the same
- [x] merge interfaces by creating union types for members
- [ ] union types for array that contain mixed literal types: `nums: [1, "2"] -> nums: number|string[]`
      (already works for complex objects)
- [x] quoted member names when needed
- [ ] handle invalid name for interface
- [ ] Allow wrapping in namespace: eg: 
    ```ts
        declare namespace Projects {
            export interface ILoc {
               lat: number;
               lng: number;
            }
            ...
        }
    ```
    
### CLI
- [x] CLI tool to accept stdin (with `--stdin` flag)
- [x] CLI tool to accept json file as input
- [ ] CLI tool to accept URL as input (for validating against remote API)
- [ ] configurable output (filename/stdout etc)
