# TypeScript - Interfaces

## Introduction
Interfaces fill the role of naming these types and are a powerful way of defining contracts within your code as well as contracts with code outside of your project.  

The compiler only checks that at least the ones required are present and match the types required. 

```js
interface LabelledValue {
    label: string;
}

function printLabel(labelledObj: LabelledValue) {
    console.log(labelledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};
printLabel(myObj);
```

## Optional Properties
Interfaces with optional properties are written similar to other interfaces, with each optional property denoted by a **?** at the end of the property name in the declaration.
```js
interface SquareConfig {
    color?: string;
    width?: number;
}
```

## Readonly properties
```js
interface Point {
    readonly x: number;
    readonly y: number;
}
```

TypeScript comes with a ReadonlyArray<T> type that is the same as Array<T> with all mutating methods removed, so you can make sure you don’t change your arrays after creation.

## Excess Property Checks
Combine interface and optional properties would let you to shoot yourself in the foot the same way you might in JavaScript.  
**Object literals** get special treatment and undergo excess property checking when assigning them to other variables, or passing them as arguments. If an object literal has any properties that the “target type” doesn’t have, you’ll get an error.

```js
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    // ...
}

let mySquare = createSquare({ colour: "red", width: 100 }); // error
```

Getting around these checks is actually really simple. The easiest method is to just use a type assertion:
```js
let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);
```

However, a better approach might be to add a string index signature if you’re sure that the object can have some extra properties that are used in some special way. If SquareConfig can have color and width properties with the above types, but could also have any number of other properties, then we could define it like so:

```js
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}
```

## Function Types
Interfaces are capable of describing the wide range of shapes that JavaScript objects can take. In addition to describing an object with properties, interfaces are also capable of describing function types.

```js
interface SearchFunc {
    (source: string, subString: string): boolean;
}
```

## Indexable Types
Indexable types have an index signature that describes the types we can use to index into the object, along with the corresponding return types when indexing.  

```js
interface StringArray {
    [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];

let myStr: string = myArray[0];
````

There are two types of supported index signatures: string and number. It is possible to support both types of indexers, but the type returned from a numeric indexer must be a subtype of the type returned from the string indexer. This is because when indexing with a number, JavaScript will actually convert that to a string before indexing into an object. That means that indexing with 100 (a number) is the same thing as indexing with "100" (a string), so the two need to be consistent.

```js
class Animal {
    name: string;
}
class Dog extends Animal {
    breed: string;
}

// Error: indexing with a numeric string might get you a completely separate type of Animal!
interface NotOkay {
    [x: number]: Animal;
    [x: string]: Dog;
}
```

```js
interface NumberDictionary {
    [index: string]: number;
    length: number;    // ok, length is a number
    name: string;      // error, the type of 'name' is not a subtype of the indexer
}
```

## Class Types

### Implementing an interface
One of the most common uses of interfaces in languages like C# and Java, that of explicitly enforcing that a class meets a particular contract, is also possible in TypeScript.

```js
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date);
}

class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}
```˝