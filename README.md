# TypeScript Notes
JavaScript was never intended to be the ubiquitous programming lnaguge that it is today. It was created to add some simple scripting behaviour to browsers.

TypeScript is a language built on top of JavaScript to help avoid the common pitfalls and bugs that arise in JavaScript, and to seek to improve the experience of writing JavaScript. It does this by adding types.

TypeScript performs static checking. It detects errors in our code without running it. It does this error checking on the basis of the "kinds of data" in our programs, that is **types**.

TypeScript's type system
- helps us find errors
- analyzes our code as we type
- only exists in development

After TypeScript has checked our code, it's compiled to regular JavaScript that can run in a browser or Node.js.

The easiest way to install TypeScript is via npm with `npm i -g typescript`, whcih gives you access to the `tsc` command anywhere in your terminal.

The [TypeScript Playground](https://www.typescriptlang.org/play) is a browser-based environment where you can try out TypeScript and learn about it's features.

TypeScript files need to end in `.ts`. 

Run `tsc FILENAME` to compile TypeScript to to JavaScript.

## Type Annotation
You can tell TypeScript that a function returns a Boolean, or this function accepts two numbers and returns a number, or this object must have a property called "colors" set to an array of strings, or a variable is a string.

Add `:Type` after a variable's name to specify its type. This is a Type Annotation.

Strings are annotated with `string`.

```ts
const myAwesomeVariable: string = "So awesome!"
```

Numbers are annotated with `number`. There are no distinctions for `float`, `int` etc.

```ts
let myNumber: number = 42;
```

Booleans are annotated with `boolean`.

```ts
const myBoolean: boolean = true;
```

### Type Inference
Type Inference refers to the TypeScript compiler's ability to infer types from certain values in your code.

TypeScript can remember a value's type, even if you didn't provide a type annotation, and it will enforce that type going forward.

```ts
let x = 27;
x = "Twenty Seven" // Error - Type 'string' is not assignable to type 'number'
```

### Any
"any" is an escape hatch. It turns off type checking for this variable so you can do your thing. It also defeats the purpose of TypeScript, so use it sparingly.

```ts
const myComplicatedData: any = "I'm going to be complicated"

myComplicatedData = 87;
myComplicatedData = "abc...";
myComplicatedData = true;
```

### Delayed Initialization and Implicit Any
If you declare a variable without a type annotation before initializing it, TypeScript will infer that it's of type `any`.

```ts
const movies = ["Arrival", "The Thing", "Aliens", "Amadeus"];
// let foundMovie; // Any
let foundMovie: string;

for (let movie of movies) {
	if (movie === "Amadeus") {
		foundMovie = "Amadeus";
	}
}
```

## Functions
### Function Parameter Types
In TypeScript, we can specify the type of function parameters in a function declaration. Each parameter can have its own type.

This allows TypeScript to enforce the types for the values being passed into your function.

You can also use type annotations with default parameters.

TypeScript will complain if you pass too many or too few arguments.

```ts
const encourageStudent = (name: string) => {
return `Het ${name}, you're doing great!`
}

function doSomething(person: string, age: number, isFunny: boolean) {
	// do something here
}

function greet(person: string = "stranger") {
	return `Hi there, ${person}`
}

encourageStudent("you") // valid
encourageStudent(85) // Error
```

### Function Return Types
We can specify the type returned by a function. Even though TypeScript can often infer this, it's good to use explicit annotations.

```ts
const add = (x: number, y: number): number => {
	return x + y;
}

function square(num: number): number {
	return num * num;*
}
```

### Anonymous Function Contextual Typing
When TypeScript can infer how an unnamed function is going to be called, it can automatically infer its parameters' types.

```ts
const numbers = [1,2,3]

numbers.forEach(num => { // you don't need num: number
	return num.toUppercase(); // Error
});
```

### Void
Void is a return type for functions that don't return anything. TypeScript can infer this type fairly well but sometimes you will want to annotate a function with a void return explicitly.

```ts
const annoyUser = (num: number): void =>
	for(let i = 0; i < num; i++) {
		alert("HIIIII!!")
	}
```

### Never
The `never` type represents values that never occur. We might use it to annotate a function that never returns, whether it always throws an exception or never finishes executing.

Unlike `void`, which is technically still a return value, with `never` a function doesn't even finish executing.

```ts
const neverStop = (): never => {
	while(true) {
		console.log("I'm still going!");
	}
}

const giveError = (msg: string) => {
	throw new Error(msg);
}
```

## Object Types
Objects can be typed by declaring what the object should look like in the annotation.

Accessing a property that isn't defined or performing operations without keeping types  in mind will throw errors.

```ts
// annotating object literal
let coordinate: { x: number, y: number } = {x: 34, y: 2 };

// annotating object as return type
function randomCoordinate(): { x: number, y: number } {
	return { x: Math.random(), y: Math.random() }
}

// annotating object as function parameter
const printName = (person: { first: string, last: string }): string => {
	return `Name: ${person.first} ${person.last}`
}

printName({ first: "Will", last: "Ferrell"});
```

### Excess Properties
Object literals get special treatment and undergo **excess property checking** when they are assigned to other variables, or passed as arguments to functions. 

If an object literal has any properties that the “target type” doesn’t have, you’ll get an error.

If you pass a variable referencing an object as a parameter, it doesn't receive excess property checking. As long as the necessary properties are there, and the correct types, it works.

```ts
const printName = (person: { first: string, last: string }): string => {
	return `Name: ${person.first} ${person.last}`
}

printName({ first: "Mick", last: "Jagger", age: 473 }); // Error

const singer = { first: "Mick", last: "Jagger", age: 473 }
printName(singer) // Name: Mick Jagger
```

### Type Aliases
Instead of writing out object types in an annotation, we can declare them separately in a type alias, which is simply the desired shape of the object. You'll often do this for more complicated types.

This lets us make out code more readable and even reuse the types elsewhere.

```ts
type Person = {
	name: string;
	age: number;
};

const sayHappyBirthday = (person: Person) => {
	return `Hey ${person.name}, congrats on turning ${person.age}!`;
}

sayHappyBirthday({ name: "Jerry", age: 42 });
```

### Nested Objects
Nested objects are annotated with the exact same syntax as other objetcs.

```ts
type Song = {
	title: string;
	artist: string;
	numStreams: number; 
	credits: { producer: string, writer: string };
};

function calculateRoyalties(song: Song): number {
	return song.numStreams * 0.0033
}

const describePerson = (person: {
												name: string;
												age: number;
												parentNames: {
													mom: string;
													dad: string;
												}
}) => {
	return `Person: ${name},
	Age: ${age},
	parents: ${parentNames.mom}, ${parentNames.dad}.`;
}

describePerson({name: "Jimmy", age: 10, parents: { mom: "Kim", dada: "Steve" }})
```

### Optional Properties
Object properties can be marked as optional with `?`.

```ts
type Point = {
	x: number;
	y: number;
	z?: number;
};

const myPoint: Point = { x: 1, y: 2, z: 3 };
const myOtherPoint: Point = { x: 1, y: 2 };
```

### `readonly` Modifier
`readonly` lets us mark certain properties in an object, array or class as read only.

```ts
type User = {
	readonly id: number;
	username: string;
};

const user: User = {
	id: 12387,
	username: "catgurl"
};

user.id = 23578 // Error
```

### Intersection Types
You can combine multiple types with `&`. You can also add additional properties to intersection types.

```ts
type Circle = {
	radius: number;
}

type Colorful = {
	color: string;
}

type ColorfulCircle = Circle & Colorful;

const happyFace: ColorfulCircle = {
	radius: 4,
	color: "yellow"
};

type Cat = {
	numLives: number;
};

type Dog = {
	breed: string;
};

type CatDog = Cat & Dog & {
	age: number;
};
```

## Array Types
Arrays can be typed using a type annotation followed by empty brackets, like `number[]` for an array of numbers, or alternatively `Array<number>`. You can use this with arrays of custom types as well as primitives.

These arrays only allow data of that one type inside them.

```ts
const users = [] // any
const activeUsers: [] = [] // always empty array

let names: string[] = ["Big Bird", "Oscar"];
let ages: number[] = [24, 32, 19, 29];

let names: Array<string> = ["Big Bird", "Oscar"];
let ages: Array<number> = [24, 32, 19, 29];

type Point {
	x: number;
	y: number;
}

const coords: Point[] = []
```

Nested arrays are annotated differently depending on how deeply nested they are.

```ts
const board: string[][] = [["X", "O", "X"],["X", "O", "X"],["X", "O", "X"]];

const demo: number[][][] = [[[1]]];
```

## Union Types
Union types let us give a value a few different possible types. If the eventual value's type is included, TypeScript will be happy. This is useful if we don't know the type at runtime.

We can create a union type using `|` to separate the types we want to include.

```ts
let age: number | string = 21;

const guessAge(age: number | string): string => {
	return "Your age is " + age;
}

type Point = {
	x: number;
	y: number;
}

type Location = {
	lat: number;
	long: number;
}

let coordinates: Point | Location = { x: 1, y: 34 };
coordinates = { lat: 21.213, long: 23.334 };
```

### Type Narrowing with Union Types
Sometimes we'll have a function that had a union type as a parameter and has an internal operation that only works with one of the types. TypeScript will complain if one of the parameter's types could cause a issue.

Type narrowing is simply doing a type check before working with a value. If our value is a string, we may want to use it differently than if we got a number.

Since unions allow for multiple types for a value, it's good to check what came through before working with it.

```ts
function calculateTotal(price: number | string, tax: number): number {
	if(typeof price === string) {
		price = parseFloat(price.replace("$", ""));
	} 
	return price;
}
```

### Union Types and Arrays
You can use union types with arrays that contain multiple different types. Do not confuse this with a union type of two different arrays.

```ts
const coords: (Point | Location)[] = [];

// array of numbers or strings 
const stuff: (number | string)[] = [1,2,3, "das"];

// array of numbers or array of strings, not both
const otherStuff: number[] | string[] = [1,2,3];
```

### Literal Types
Literal types are not just types but the literal values themselves too.

```ts
// must literally equal 0
let zero: 0 = 0;

// literally the string "hi"
let hi: "hi" = "hi";
```

They can be combined with union types to create fine-tuned options for TypeScript to enforce. For example, you can assert that a variable must be one of a small number of literal values.

```ts
let mood: "Happy" | "Sad" = "Happy"; 

const giveAnswer = (answer: "yes" | "no" | "maybe") => {
	return `The answer is ${answer}.`;
}

giveAnswer("no")

giveAnswer("I'm not sure") // Error
```

## Tuples and Enums
Tuples are a special type exclusive to TypeScript (they don't exist in JavaScript) for storing simple data that is intrinsically linked.

Tuples are arrays of fixed length and ordered with specific types - like super rigid arrays.

Weirdly, the way tuples have been designed doesn't actually prevent you from pushing on extra elements after their creation.

```ts
const color: [number, number, number] = [255, 0, 45];

let myTuple: [number, string];

myTuple = [10, "Typescript is fun"]

myTuple = ["TypeScript is fun, 10"] // Error

type HTTPResponse = [number, string];

const responses: HTTPResponse[] = []; // array of tuples

const goodRes: HTTPResponse = [200, "OK"];

goodRes[0] // 200

goodRes.push(123) // this works
goodRes.pop() // so does this
```

### Enums
Enums let us define a set of named constants. We can give these constants numeric or string values.

The constants don't need to be capitalized but often are by convention.

The constants don't all need to be of the same type, but usually are.

If you don't assign a value to the constants, they are assigned a number starting at 0. If you assign a number value to the first constant, the rest will increment from there.

```ts
enum Responses {
	no, // 0
	yes, // 1
	maybe // 2
}

enum Responses {
	no = "No",
	yes = "Yes",
	maybe = "Maybe"
}

enum Directions {
	UP = "up",
	DOWN = "down",
	LEFT = "left",
	RIGHT = "right"
}

enum OrderStatus {
	PENDING,
	SHIPPED,
	DELIVERED,
	RETURNED
}

const currentStatus = OrderStatus.DELIVERED;

function isDelivered(status: OrderStatus): boolean {
	return status === OrderStatus.DELIVERED
};

isDelivered(OrderStatus.SHIPPED); // false
```

When TypeScript containing enums is converted to JavaScript, an enum becomes a JavaScript object wrapped in an immediately invoked function expression.

```ts
// TypeScript
enum OrderStatus {
	PENDING,
	SHIPPED,
	DELIVERED,
	RETURNED
}

// JavaScript
"use strict";
var OrderStatus;
(function (OrderStatus) {    
	OrderStatus[OrderStatus["PENDING"] = 0] = "PENDING";   
	OrderStatus[OrderStatus["SHIPPED"] = 1] = "SHIPPED";   
	OrderStatus[OrderStatus["DELIVERED"] = 2] = "DELIVERED";    
	OrderStatus[OrderStatus["RETURNED"] = 3] = "RETURNED";
})(OrderStatus || (OrderStatus = {}));
```

Alternatively we can use a `const enum` which erases the enum and replaces every referenced value with the underlying value.

```ts
// TypeScript
const enum OrderStatus {
    PENDING,
    SHIPPED,
    DELIVERED,
    RETURNED,
}

const order = {
    orderNumber: 20935813,
    status: OrderStatus.PENDING
}

// JavaScript
"use strict";
const order = {
	orderNumber: 20935813,
	status: 0 /* OrderStatus.PENDING */
};
```