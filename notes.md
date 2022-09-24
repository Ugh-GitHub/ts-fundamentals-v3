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

## Chapter 5: Unions & Intersection Types

Union types are equivalent to `|` operator (inclusion)

Here's an example of a more complex union type:
`
function flipCoin(): "heads" | "tails" {
  if (Math.random() > 0.5) return "heads"
  return "tails"
}
 
function maybeGetUserInfo():
  | ["error", Error]
  | ["success", { name: string; email: string }] {
  if (flipCoin() === "heads") {
    return [
      "success",
      { name: "Mike North", email: "mike@example.com" },
    ]
  } else {
    return [
      "error",
      new Error("The coin landed on TAILS :("),
    ]
  }
}
 
const outcome = maybeGetUserInfo()

// const outcome: ["error", Error] | ["success", {
//  name: string;
//  email: string;
// }]
`

If the tuple is destructured, we end up with `const first: "error" | "success"` and `const second: Error | {
    name: string;
    email: string;
}`.

The first one registers as a string (as all possibilities are strings), while the second one only has the `name` property available, as that is the only property available to both the `Error` class and to this specific 'user info object.'

##### Typeguarding & Narrowing

(Overview)[https://www.typescriptlang.org/docs/handbook/2/narrowing.html]

Type guards are expressions, which when used with control flow statement, allow us to have a more specific type for a particular value.

`instanceof` is a great tool for typeguarding, but can also use `typeof` + `===` and other methods (see Overview for details). The typescript compiler recognizes these instances as narrowing.

(Discriminated or Tagged Unions)[https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html#discriminated-unions] exist in TS (kind of like `data` in Haskell), where each type contains a common property.

Lastly, (Intersection Types)[https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html#intersections] exist and are`&` sign associated. Not a lot of info on this one, as they're far less common to use. More info from (the following)[https://www.typescriptlang.org/play?ssl=83&ssc=1&pln=27&pc=1#code/PTAEBUE8AcFNQK4DsCWB7JBnUBDATvDqAO46ShoBmoAJrAMYA2+KSA5qAC4AWOnuSCgCMAVg04BYAFAhQ9NAkY1QQ+AFs0BLr0EZ4nGLAB006QbigAypzys2AeTwA5BGtV5QAXlCYbd0AA+oEiu7gDcZoagAAp4aPSwmJjWfIleoABEaHBIGYGZTGiYsDQZEVLm8PY0NC5usHiYAKpIdHjgsILeAIz5AMz5AKz5AOz5AJzllaAAggCyiZiQLehdmdywjIxoeUHdgwBs+QDeoA1xeABcXHgI8AC+5dKyAJLUPPAIxRTUWTl5OFaBW2xVKoAAbthfLZ2KAUJhnmAkLBiFw0KBIAoADRyDb0ADWFAQnGusAAHjg1NBGLBLowUJwGjhGAipIjQAB1eD0QGgNQoMm0FCUSgNTr8SrYVicdFEZCrHGAmjs4i8fjEWAAci0mDI-nh2j42ng4OZdzh2D0P20RX0hkwJjZMjAUDglnotmg6pQW2NghpOHB8ExCDRoEoKDYCC0Cn43DQqJl7LojLw-ORJG4KHo3AhZvgDLkCiUKkI-FuSE4KDUxlMztAKww2B5gkwaBrVZr2FUiFaDXTJRU5GmxBQdFQ7Cx7MomjOFKpNMudemHNYNATKUZ2G8f06u2BtrBQQy6erKAAXiV99C7E9628MQpQPGgx4cEI0EGcSG5Lzivo1U5NcN04VJWVkVR5C7XAfD8WEAFpgjQCUNl7VYjAgLMpWbT8xWUVh2XJSlqVpSp4NHcc7HgpV4KQfA4lHdg61eag5VQDALQEUB7AAJRxD5BF5aUGmKegqw4g1eRmJwABFHVeSsRPEVYuHtXAtFVTouGIdFJThRTGnEMMPVgVJ2SIZFE0MDDwCw3AtgTbAZw8aYoOgIoGXQuthLwSgcASUAAFE8AuAAJJV6VhY5pFAHwEHoBIkmuD80ADJByli85NAAfmuU4u11NhaVgmEOEeaR7m8gy-ICmY8E4HS8HxTAZL4IhoqkWL8AazRmryrgGUXEr-HuABtABdcpKqdHyavgOqq18Fq2tADquvq+FOEwfq6Jra4b1hMbJoq5iXQ2b5Zv8tIW1LIsqUPfTQAITB3KwNJVWzXNeCDdkPx4IssE28UzhC2cdBoSK2EVIEPhQDwE0EGg2sdSILAWxrmp4xJXu+bx0d65bQNAAAyILQbwcLWkhqYogWzbMCxl6m3gPGNqW1qidJ4Kwoi29TtAAAxWciIXWk63kLA4wi2A6aWxmcZZ0AAApnoV65Za2+XmYASi8AA+VaYrhagVex5mjCyvBdbW2KAbbGkLfJ02mbex2LiMAqcCK7WMttghOGjdKjemo2Jft4xtjYZ2FaMbr6Z9iq71kGY+QFa0XgM0TxMEoFG0EPTIPbNICGZRhIHZL5YEoRRMy0n9vu5HBimwTStF5NBRCM3hsCTetWCYBA6AoDMqG09FTUYO5tqq1M5tAABhEvGQ1gAhdAV6b+Abbj3wXhk-a4LYX2eDhmhom6yBcpUNBUtMoOpBDiookX0zl7Zzg17QLGAEcp-4bwX6pFXuvTeJNlanG4JwNQjAD6lVAPcE4fJ8D4nXMQJAsCRoJydLIJwCZHyhhuhgMucgl6ECerAX+iR1QbEED+fuk86DmXfnvAQyhYAMg2B4SB0CKAeDUMg1BSBxZNnVL1OwP8-7XEAW-RaH90ASKoekbezD96ZCEICZqkAMhTk6kgpqgjrgZA3kgTRnFeSAgwJADQXwgrsGYK0eC6jQSgDYHgHAIpPLqVkUYHx2jE7CMlioHANAFG+CkaQ4BX8KF-yUUbHenA96GPUSYpYfiH7lCAA]
`
// If a union is an OR, then an intersection is an AND.
// Intersection types are when two types intersect to create
// a new type. This allows for type composition.

interface ErrorHandling {
  success: boolean;
  error?: { message: string };
}

interface ArtworksData {
  artworks: { title: string }[];
}

interface ArtistsData {
  artists: { name: string }[];
}

// These interfaces can be composed in responses which have
// both consistent error handling, and their own data.

type ArtworksResponse = ArtworksData & ErrorHandling;
type ArtistsResponse = ArtistsData & ErrorHandling;

// For example:

const handleArtistsResponse = (response: ArtistsResponse) => {
  if (response.error) {
    console.error(response.error.message);
    return;
  }

  console.log(response.artists);
};

// A mix of Intersection and Union types becomes really
// useful when you have cases where an object has to
// include one of two values:

interface CreateArtistBioBase {
  artistID: string;
  thirdParty?: boolean;
}

type CreateArtistBioRequest = CreateArtistBioBase & ({ html: string } | { markdown: string });

// Now you can only create a request when you include
// artistID and either html or markdown

const workingRequest: CreateArtistBioRequest = {
  artistID: "banksy",
  markdown: "Banksy is an anonymous England-based graffiti artist...",
};

const badRequest: CreateArtistBioRequest = {
  artistID: "banksy",
};
`

## Chapter 6: Type Aliasing and Interfaces

Type aliases allow for more meaningful naming conventions, a singular location for the definition of types, and importing and exporting of types as you would a function or class. Other advantages include cleaner and more semantic tooltips.

`
// @filename: types.ts
type UserContactInfo = {
  name: string
  email: string
}
`

When aliasing a type, the `=` operator is used, `TitleCase` is used, and an alias with a given name can only be declared once in a given scope. Aliases can hold any type. (Inheritance) Aliases can also take advantage of intersections (`&`) to allow existing types to be repurposed in different ways. While types do not have a true `extends` keyword that defines them, this pattern has a very similar effect.

`
type SpecialDate = Date & { getReason(): string }
 
const newYearsEve: SpecialDate = {
  ...new Date(),
  getReason: () => "Last day of the year",
}
newYearsEve.getReason
`

##### Interfaces

An (Interface)[https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#interfaces] is a way of defining an (object type)[https://www.typescriptlang.org/docs/handbook/2/objects.html]. An "object type" can be thought of as, "an instance of a class could conceivably look like this" (?). Easiest to think of them as a contract of how the code is to be structured. Easiest technical def is that an interface is an abstract type used to define the behavior of a class. It's used to achieve abstraction (where only the essential details are displayed to the user, or identifying only the relevant characteristics of an object, i.e. how to make a car accelerate or break, not how these systems work). Refer to `Extends_v_Implements` for more details about the differences.

For example, `string | number` is not an object type, as it uses the union type operator. Like type aliases, interfaces can be imported/exported between modules just like values, and they serve to provide a “name” for a specific type.

`
interface UserInfo {
  name: string
  email: string
}
function printUserInfo(info: UserInfo) {
  info.name
}
`

Inheritance in interfaces is a bit broader and more complex than in aliases. 

`Extends`

If you’ve ever seen a JavaScript class that “inherits” behavior from a base class, you’ve seen an example of what TypeScript calls a heritage clause: extends

Just as in in JavaScript, a subclass extends from a base class.
Additionally a “sub-interface” extends from a base interface, as shown in the example below

`
class Animal {
  eat(food) {
    consumeFood(food)
  }
}
class Dog extends Animal {
  bark() {
    return "woof"
  }
}
 
const d = new Dog()
d.eat
d.bark
`

`
interface Animal {
  isAlive(): boolean
}
interface Mammal extends Animal {
  getFurOrHairColor(): string
}
interface Dog extends Mammal {
  getBreed(): string
}
function careForDog(dog: Dog) {
  \\ have access to all three methods
}
`

`IMPLEMENTS`
TypeScript adds a second heritage clause that can be used to state that a given class should produce instances that conforms to a given interface.

`
interface AnimalLike {
  eat(food): void
}
 
class Dog implements AnimalLike {
  bark() {
    return "woof"
  }
  eat(food) {
    consumeFood(food)
  }
}
`

While TypeScript (and JavaScript) does not support true multiple inheritance (extending from more than one base class), this implements keyword gives us the ability to validate, at compile time, that instances of a class conform to one or more “contracts” (types). Note that both extends and implements can be used together:

`
class LivingOrganism {
  isAlive() {
    return true
  }
}
interface AnimalLike {
  eat(food): void
}
interface CanBark {
  bark(): string
}
 
class Dog
  extends LivingOrganism
  implements AnimalLike, CanBark
{
  bark() {
    return "woof"
  }
  eat(food) {
    consumeFood(food)
  }
}
`

Can use `implements` without aliasing, but don't?
