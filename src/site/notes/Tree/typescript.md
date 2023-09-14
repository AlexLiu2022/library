---
{"dg-publish":true,"dg-path":"  typescript.md","permalink":"/typescript/","tags":["CS/web","CS/programming-languages "],"created":"2023-05-29T22:18:52.481+08:00","updated":"2023-09-14T17:17:25.827+08:00"}
---


# Basic
## basic ts config
### compile ts to js

```shell 
tsc hello-world.ts
```

### configuration  file for ts
```shell 
tsc --init
```


```json
// part of tsconfig.json
{
  "compilerOptions": {
    /* Visit https://aka.ms/tsconfig to read more about this file */
    /* Language and Environment */
    ...
    "target": "ES2016",                                  /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */

    /* Modules */
    "module": "commonjs",                                /* Specify what module code is generated. */
    "rootDir": "./src",                                  /* Specify the root folder within your source files. */

    "outDir": "./dist",                                   /* Specify an output folder for all emitted files. */
    "removeComments": true,                           /* Disable e

    /* Type Checking */
    "strict": true,                                      /* Enable all strict type-checking options. */

    "skipLibCheck": true                                 /* Skip type checking all .d.ts files. */
  }
}
```

so we can now just use `tsc` to compile all the typescript files in our project.

## debug ts code

enable sourceMap option in `tsconfig.json`
create a `launch.json` file, this tells vscode how to debug this application
Add an option `"preLaunchTask": "tsc: bulid - tsconfig.json"`
Then we can debug. **F5 is the shortcut ** 

# fundamental
## bulid-in types
![diagram-of-build-in-types-of-js-ts.png](https://cdn.jsdelivr.net/gh/AlexLiu2022/resources/img/diagram-of-build-in-types-of-js-ts.png)

We can separate big number using _ like:
```ts
let sales: number = 123_456_789;
```

### The any type

Don't use it 

### Arrays

```ts
let numbers : number[] = [1 ,2, 3]; // remove number[] it can still be infer ed
```

```ts
let numbers : number[] = []; // to avoid numbers being any type
numbers.forEach(n => n.XXX // it can be auto...) 
```

### tuple

```ts
let user : [number, string] = [1, 'Mosh'] // [better keep it 2]
```

### enum