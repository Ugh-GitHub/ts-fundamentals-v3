## Chapter 1

##### tsconfig

` {
  "compilerOptions": {
    "outDir": "dist", // where to put the TS files
    "target": "ES3" // which level of JS support to target
  },
  "include": ["src"] // which files to compile
} `

##### ts files

A good way to think of TS files:

.ts files (source files) contain both type information and code that runs
.js files (compiled code) contain only code that runs
.d.ts files (declaration files) contain only type information

https://www.typescript-training.com/course/fundamentals-v3/02-hello-typescript/

## Chapter 2

type inference: places where typescript infers type where no type info was provided (i.e. in `let age = 6;` `age: number`, whereas `const apples = 6` will be `apples: 6` [a literal type]). [See the link for more specific info](https://www.typescriptlang.org/docs/handbook/type-inference.html).

const: const variable declarations cannot be reassigned. They are immutable value types, also known as literal types (in the apples example, the const can only hold 6).

any: most flexible type. Typescript can infer from a simple declaration of a unassigned variable (i.e. `let endTime`).

type annotation: example `let endTime: Date`

function args and return values: 
```
function add(a: number <!-- argument value -->, b: number): number <!-- return  value -->  {
  return a + b;
}
```





