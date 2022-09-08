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

## Chapter 2: Variables & Values

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

## Chapter 3: Objects, Arrays, & Tuples

In general, object types are defined by:
1. The names of the properties that are (or may be) present
2. The types of those properties

```
let car: {
  make: string
  model: string
  year: number
}
```

NOTE: doesn't appear to be a way to assign values in the declaration.


optional property: use a `?` operator after the property name. ex: `chargeVoltage?: number`

Typescript also handles excess properties. If the error comes up, check the last part of the error message.

```
function printCar(car: {
  make: string
  model: string
  year: number
  chargeVoltage?: number
}) {
  // implementation removed for simplicity
}
 
printCar({
  make: "Tesla",
  model: "Model 3",
  year: 2020,
  chargeVoltage: 220,
  color: "RED", // <0------ EXTRA PROPERTY
```

This can be resolved by:
1. Remove the color property from the object
2. Add a color: string to the function argument type
3. Create a variable to hold this value, and then pass the variable into the printCar function

##### Index signatures

Useful when trying to represent dictionaries. The index signature is a way to represent the value assigned to a key in the dictionary object. It's a great when all of the names of the type's properties aren't known ahead of time, but the shape of the values is known. The index signature must be a number, string, (symbol)[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol], or a (template literal type)[https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html].


```
const phones = {
  home: { country: "+1", area: "211", number: "652-4515" },
  work: { country: "+1", area: "670", number: "752-5856" },
  fax: { country: "+1", area: "322", number: "525-4357" },
}

const phones: {
  [k: string]: {
    country: string
    area: string
    number: string
  }
} = {}
 
phones.fax
```

##### Array Types

Describing types for arrays is often as easy as adding [] to the end of the array member’s type. For example, `const fileExtensions = ['js', 'ts']` will have a type of `string[]`.

##### Tuples

A (tuple)[https://en.wikipedia.org/wiki/Tuple] multi-element, ordered data structure, where position of each item has some special meaning or convention.

```
let myCar: [number, string, string] = [
  2002,
  "Toyota",
  "Corolla",
]
```

WARNING: As of (TypeScript 4.3)[https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-3.html#separate-write-types-on-properties], there’s limited support for enforcing tuple length constraints. `push` and `pop` get around it.

## Chapter 4: Sturctured vs Nominal types

Typescript is statis (checks at compile time vs runtime) and structural (type checking doesn't care about the constructor name,
it cares about whether it has the corresponding properties with type equivalencies). Structural can be thought of similarily to the adage: "If it quacks like a duck, walks like a duck,... it's a duck" (though the phrase is mostly attributed to dynamic type systems).

## Chapter 5: 