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