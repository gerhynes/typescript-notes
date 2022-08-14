# TypeScript Notes
JavaScript was never intended to be the ubiquitous programming lnaguge that it is today. It was created to add some simple scripting behaviour to browsers.

TypeScript is a language built on top of JavaScript to help avoid the common pitfalls and bugs that arise in JavaScript, and to seek to improve the experience of writing JavaScript. It does this by adding types.

TypeScript performs static checking. It detects errors in our code without running it. It does this error checking on the basis of the "kinds of data" in our programs, that is **types**.

TypeScript's type system
- helps us find errors
- analyzes our code as we type
- only exists in development

After TypeScript has checked our code, it's compiled to regular JavaScript that can run in a browser or Node.js.

The easiest way to install TypeScript is via npm with `npm i -g typescript`, which gives you access to the `tsc` command anywhere in your terminal.

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

## Interfaces
Interfaces serve almost the exact same purpose as type aliases (with a slightly different syntax).

We can use them to create reusable, modular types that describe the shapes of objects.

```ts
interface Person {
	name: string;
	age: number;
}

const sayHappyBirthday = (person: Person) => {
	return `Hey ${person.name}, congrats on turning ${person.age}`
}

sayHappyBirthday({ name: "Jerry", age: 42 });
```

### Readonly and Optional Interface Properties
We can mark inteface properties as optional using `?`. Readonly properties can also be set.

```ts
interface Person {
	readonly id: number;
	first: string;
	last: string;
	nickname?: string; // optional
}
```

### Interface Methods
We can also describe what methods are included on an object, and their parameters and return types.

The parameter name doesn't matter.

```ts
interface Person {
	readonly id: number;
	first: string;
	last: string;
	nickname?: string;
	sayHi: () => string;
	sayHello(): string;
}

interface Product {
	name: string;
	price: number;
	applyDiscount(discount: number): number;
}

const shoes: Product = {
	name: "Blue Suede Shoes"
	price: 100,
	applyDiscount(amount: number) {
		return this.price * (1 - amount);
	}
}
```

### Reopening Interfaces
We can add new properties to interfaces after we have already described them. This extends, rather than overwrites, the initial interface.

```
interface Person {
	name: string;
}

interface Person {
	age: number;
}

const person:Person = {
	name: "Jerry",
	age: 42
}
```

### Extending Interfaces
Similar to how in OOP a class can inherit functionality from a parent class, you can extend an interface.

The new interface inherits all of the type rules from the original interface.

```ts
interface Dog {
	name: string;
	age: number;
	breed: string;
	bark(): string;
}

interface ServiceDog extends Dog {
	job: "drug sniffer" | "bomb sniffer" | "guide dog";
}

const chewie: ServiceDog = {
	name: "Chewie",
	age: 4.5,
	breed: "Lab",
	bark() {
		return "Bark!"
	},
	job: "guide dog"
}
```

You can extend multiple interfaces.

```ts
interface Person {
	name: string;
}

interface Employee {
	readonly id: number;
	email: string;
}

interface Engineer extends Person, Employee {
	level: string;
	languages: string[];
}

const Pierre: Engineer = {
	name: "Pierre",
	id: 12345,
	email: "pierrel@example.com",
	level: "senior",
	languages: ["JavaScript", "Python"]
}
```

### Interfaces vs Type Aliases
Interfaces can only describe the shape of an object, while type aliases can describe any sort of type, such as literals, union types, etc.

We can reopen interfaces, where TypeScript would complain if you duplicated type aliases.

Interfaces can extend other interfaces. To do something similar for type aliases, we would need to use an intersection type.

## The TypeScript Compiler
Running `tsc --init` will create a `tsconfig.json` file for your TypeScript config. 

You can pass the `-w` or `--watch` flag to the `tsc` command to tell TypeScript to recompile on file changes.

```
tsc -w index.ts
```

Using `tsc` on its own will compile all TypeScript files in the folder in which it was run.



You can tell TypeScript to only compile certain files using the `files` option in `tsconfig.json`. **Note:** This is a top-level option, not part of the `compilerOptions`.

```json
{    
	"compilerOptions": {},    
	"files": [      
		"core.ts",      
		"sys.ts",      
		"types.ts",      
		"scanner.ts",      
		"parser.ts",      
		"utilities.ts",     
		"binder.ts",      
		"checker.ts",      
		"tsc.ts"    
	]  
}
```

### `include` and `exclude`

Two more options to tell TypeScript which files to comile and which to ignore are `include` and `exclude`. They let you include/exclude specific files, directories and patterns. Any filenames are resolved relatively to the directory containing `tsconfig.json` (which should be in the root of your project).

`include` defaults to `**`, or everything. `exclude` defaults to `node_modules`

```json
{
	"include": ["src/**/**, tests/**/**"]
	"exclude": ["node_modules", "dontTouch.ts"]
}
```

### `outDir`
The `outDir` option lets us specify which directory the compiled JavaScript files should be emitted to. The original directory structure is maintained.

```json
"outDir": "./dist"
```

### `target`
The `target` option controls the JavaScript version that TypeScript compiles to.

```json
"target": "es2015"
```

### `strict`
Setting the `strict` option to `true` enables a nuber of additional type checks, such as `noImplicitAny`, and `strictNullChecks`.

## Working with the DOM
TypeScript automatically knows all the type definitions for the document object, `document: Document`. All of the document's properties and methods are defined and typed, such as `URL: string` or `getElementById(id: string): HTMLElement | null`, and the same is the case for all the elements.

### The Lib Compiler Option
TypeScript includes a default set of type definitions for built-in JavaScript APIs, such as `Math`, as well as things found in browser environments, such as `document`.

You can use the `lib` option to change this set, such as not including the DOM if building something purely server-side.

```json
	"lib": ["ES2021"]
```

### Non-Null Assertion Operator
The non-null assertion operator, a `!` after the expression, tells TypeScript that an expression is guaranteed not to be null. Only use this in situations where you are certain, since you are giving up null checks.

```ts
const btn = document.getElementbyId("btn")!; // type is HTMLElement
```

### Type Assertions
Sometimes you might have more specific information about a value's type, and you want to make sure typeScript knows it too.

You can assert a value's type by using the `as` keyword, followed by the specific type you want to assert.

There is an alternative syntax using angle brackets, though it is less common as it causes issues with JSX.

```ts
const myPic = document.querySelector(".profile-image"); // inferred HTMLElement

const myPic = document.querySelector(".profile-image") as HTMLImageElement;

<HTMLImageElement>myPic // alternate syntax
```

### Working with Events
If you're adding a callback function directly to an event handler, TypeScript can infer what type of event is happening, for example a `SubmitEvent` with a form.

If the callback function is defined separately, TypeScript doesn't have the context to know what type the event is.

```ts
// infers that e is SubmitEvent
form.addEventListener("submit", function (e) {
  e.preventDefault();
  console.log("submitted");
});

// need to specify type of event
function handleSubmit(e: SubmitEvent) {
  e.preventDefault();
  console.log("submitted");
}
```

## JavaScript Classes
Classes are templates for creating objects in JavaScript. Thye contain a few important pieces which allow for the creation and extension of customized (and nicely organized) objects.

```js
class Person {
	constructor(name, age) {
		this.name = name;
		this.age = age;
	}
	greet() {
		return `Hello ${this.name}!`;
	}
}

const jimmy = new Person("Jimmy", 25);
jimmy.greet(); // Hello Jimmy!
```

### Constructors
The constructor method is automatically called every time we instantiate a new instance of a class. Its arguments lets you store properties on a specific instance of the class.

### Class Fields
Class fields are a shortcut syntax to define fields or properties on a class instad of having to do it in the constructor. They are limited in that they only allow for hard-coded values.

```js
class Player {
	score = 0;
	numLives = 10;
	constructor(name, age) {
		this.name = name;
		this.age = age;
	}
}
```

### Private Fields
Private fields let you specify that certain properties should only be accessible inside the class.

```js
class Player {
	#score = 0;
	#numLives = 10;
	constructor(name, age) {
		this.name = name;
		this.age = age;
	}
	getScore(){
		return this.#score;
	}
	setScore(newScore){
		this.#score = newScore;
	}
	#secretMethod() {
		console.log("SECRET!!")
	}
}
```

### Getters and Setters
Getters and setters let us write something that we call as if it's a property that then runs some corresponding logic as if it was a method.

```js
class Player {
	#score = 0;
	#numLives = 10;
	constructor(first, last) {
		this.first = first;
		this.last = last;
	}
	get fullName() {
		return `${this.first} ${this.last}`;	
	}
	get score(){
		return this.#score;
	}
	set score(newScore){
		if(newScore < 0){
			throw new Error("Score must be positive");
		}
		this.#score = newScore;
	}
}

const player1 = new Player("Blue", "Steele");
player1.fullName; // Blue Steele
player1.score; // 0
player1.score = 50;
```

### Static Properties and Methods
The `static` keyword on a property or method tells JavaScript that the property/method exists on the class itself, not each individual instance. 

It lets you group functionality with the class that doesn't have anything to do with a particular instance, but has to do with the class itself, often some creation/helper method.

```js
class Player {
	static description = "Player in the game"
	
	constructor(first, last) {
		this.first = first;
		this.last = last;
	}
	static randomPlayer() {
		return new Player("Andy Samberg")
	}
}

Player.description;
Player.randomPlayer();
```

### Inheritance
In JavaScript a class can inherit shared functionality from a parent class. 

```js
class Player {...}

class SuperPlayer extends Player {...}

```

### `super`
The `super` keyword lets you call the constructor method from a parent class.

```js
class Player {
	constructor(first, last) {
		this.first = first;
		this.last = last;
	}
}

class AdminPlayer extends Player {
	constructor(powers) {
		super();
		this.powers = powers;
	}
	isAdmin = true;
}

const admin = new AdminPlayer("admin", "McAdmin", ["delete", "restore"])
```

## TypeScript Classes
With TypeScript, you need to define and type fields that come from constructor arguments.

```ts
class Player {
	first: string;
	last: string;
	
	constructor(first: string, last: string) {
		this.first = first;
		this.last = last;
	}
}
```

### Class Fields
TypeScript can infer the type of class fields, though you can also type them.

```ts
class Player {
	first: string;
	last: string;
	score = 0;
	
	constructor(first: string, last: string) {
		this.first = first;
		this.last = last;
	}
} 
```

### `readonly` Class Properties
Classes in TypeScript can use the `readonly` modifier so fields can't be updated once they're set.

```ts
class Player {
	readonly first: string;
	readonly last: string;
	score = 0;
	
	constructor(first: string, last: string) {
		this.first = first;
		this.last = last;
	}
} 
```

### `public` and `private` Modifiers
TypeScript adds `public` and `private` modifiers. In JavaScript, every property in a class is public by default. `private` properties and methods are only accessible inside the class.

```ts
class Player {
	public first: string;
	public last: string;
	public score: number = 0;
	private numLives: number = 10;
	
	constructor(first: string, last: string) {
		this.first = first;
		this.last = last;
	}

	private secretMethod(): void {
		console.log("Secret")
	}
} 
```

### Parameter Properties Shortcut
There is a shorter syntax for defining constructor parameter properties.

```ts
class Player {
	public score: number = 0;
	
	constructor(public first: string, public last: string) {}
} 

const elton = new Player("Elton", "John");
```

### Getters and Setters
By default, TypeScript makes getters readonly. TypeScript doesn't want return type annotations for setters.

```ts
class Player {	
	constructor(
	public first: string, 
	public last: string, 
	private _score: number
	) {}

	get fullName(): string {
		return `${this.first} ${this.last}`
	}

	get score():number {
		return this._score;
	}

	set score(newScore: number) {
		if (newScore < 0) {
			throw new Error("Score can't be negative");
		}
		this._score = newScore;
	}
} 

const elton = new Player("Elton", "John", 0);
elton.fullName;
elton.score;
elton.score(99);
```

### `protected`
`private` fields are only accessible in their specific class. `protected` fields are also accessible in child classes.

```ts
class Player {	
	constructor(
	public first: string, 
	public last: string, 
	protected _score: number
	) {}
} 

class SuperPlayer extends Player {
	public isAdmin: boolean = true;
	maxScore(){
		this._score = 999999;
	}
}
```

### Classes and Interfaces
Interfaces can describe the shape of a class.

```ts
interface Colorful {
	color: string;
}

interface Printable {
	print(): void;
}

class Bike implements Colorful {
	constructor(public color: string) {}
}

class jacket implements Colorful, Printable {
	constructor(public brand: string, public color: string) {}
	print() {
		console.log(`${this.color} ${this.brand} jacket`)
	}
}

const bike1 = new Bike("red");
const jacket1 = new Jacket("Patagonia", "blue");
```

### Creating Abstract Classes
Abstract classes define abstract methods that must be implemented by a child class. Child classes also inherit any non-abstract methods.
 
Abstract classes cannot be instantiated directly. You can extend an abstract class and implement an interface at the same time.

```ts
abstract class Employee {
	constructor(public first: string, public last: string){}
	abstract getPay(): number;
	printDetails(): void {
		console.log(`${this.first} ${this.last}`);
	}
}

class FullTimeEmployee extends Employee {
	constructor(
		public first: string, 
		public last: string, 
		private salary: number
	){}
	getPay(){
		return this.salary;
	}
}

class PartTimeEmployee extends Employee {
	constructor(
		public first: string, 
		public last: string, 
		private hourlyRate: number, 
		private hoursWorked: number
	){}
	getPay(){
		return this.hourlyRate * this.hoursWorked
	}
}
```

## Generics
Generics let us define reusable functions and classes that work with multiple types rather than a single type.

```ts
function identity<T>(item: T): T {
	return item;
}

identity<number>(42);

function getRandomElement<T>(list: T[]): T {
	const randIdx = Math.floor(Math.random() * list.length);
	return list[randIdx];
}

getRandomElement<number>([5, 33, 29, 87]);

document.querySelector("#username") // Element
document.querySelector<HTMLInputElement>("#username") // HTMLInputElement
```

In many cases, TypeScript can infer generic type parameters.

```ts
getRandomElement<string>(["a", "b", "c"]);
getRandomElement(["a", "b", "c"]); // also valid
```

A quirk of generics is that in `.tsx` files you need to add a trailing comma to arrow functions so that TypeScript doesn't confuse generics with JSX tags.

```ts
const getRandomElement = <T,>(list: T[]): T => {
	const randIdx = Math.floor(Math.random() * list.length);
	return list[randIdx];
}
```

Generic functions can have more than one type parameter.

```ts
function merge<T, U>(object1: T, object2: U) {
	return {
		...object1,
		...object2
	};
}

const comboObj = merge({name: "Colt"}, {pets: ["blue", "elton"]});
```

With generics, you can constrain the possible types for type parameters.

```ts
function merge<T extends object, U extends object>(object1: T, object2: U) {
	return {
		...object1,
		...object2
	};
}

interface Lengthy {
	length: number;
}

function printLength<T extends Lengthy>(thing: T): void {
	console.log(thing.length);
}
```

You can also set a default type for type parameters.

```ts
function makeEmptyArray<T = number>(): T[] {
	return [];
}

const nums = makeEmptyArray(); // defaults to type number
const bools = makeEmptyArray<boolean>(); // still works
```

TypeScript also supports generic classes.

```ts
interface Song {
	title: string;
	artist: string;
}

interface Video {
	title: string;
	creator: string;
	resolution: string;
}

class Playlist<T> {
	public queue: T[] = [];
	add(el: T){
		this.queue.push(el);	
	}
}

const songs = new Playlist<Song>();
const videos = new Playlist<Video>();
```

## Type Narrowing
### `typeof` Guards
Type narrowing refers to reducing a less-precise type to a more-precise type.

The easiest way to do this is to use a typeof guard. This involves doing a type check before working with a value.

Since unions allow multiple types for a value, we can first check what came through before working with it.

```TS
count isTeenager = (age: number | string) => {
	if(typeof age == "string") {
		console.log(age.charAt(0) === 1);
	}
	if(typeof age == "number") {
		console.log(age > 12 && age < 20);
	}
}

isTeenager("20"); // false
isTeenager(13); // true
```

This works well with primitives but less well with reference types.

### Truthiness Guards
Truthiness type guards involve checking a value for being truthy or falsy before working with it.

This is helpful in avoiding errors when values might be null or undefined.

```TS
const printLetters = (word: string | null) =>{
	if(!word) {
		console.log("No word was provided");
	} else {
		name.forEach(letter => console.log(letter));
	}
}
```

### Equality Narrowing
Equality type guards involve comparing types to each other before doing certain operations with values.

By checking two values against one another, we can be sure they're both the same before working with them in a type-specific way.

```TS
const someFunc = (x: string | boolean, y: string | number) => {
	if (x === y) {
		x.toUpperCase();
		y.toUpperCase();
	} else {
		console.log(x);
		console.log(y);
	}
}
```

### `in` Operator Narrowing
JavaScript's `in` operator helps check if a certain property exists in an object.

This means we can use it to check if a value exists in an object, according to its type alias or aliases, before working with it.

```TS
type Cat = { meow: () => void };
type Dog = { bark: () => void };

const talk = { create: Cat | Dog } => {
	if ("meow" in creature) {
		console.log(creature.meow());
	} else {
		console.log(creature.bark());
	}
}

const kitty: Cat = { meow: () => "Meowwww" };
talk(kitty); // Meowwww
```

### `instanceof` Narrowing
`instanceof` is a JavaScript operator that allows us to check if one thing is an instance of another, that is if it exists on the prototype chain.

This can help narrow types when working with things that exist in JavaScript like classes, arrays and dates.

```TS
const printFullDate(date: Date | string) {
	if (date instanceof Date) {
		return date.toUTCString();
	} else {
		return new Date(string).toUTCString();
	}
}
```

### Type Predicates
TypeScript allows us to write custom functions that can narrow the type of a value. These functions have a special return type called a type predicate.

A type predicate takes the form `PARAMETER_NAME is TYPE`.

```TS
const isCat(pet: Cat | Dog): pet is Dog {
	return (pet as Dog).bark !== undefined;
}

let pet = getAnimal();

if (isCat(pet)) {
	pet.meow();
} else {
	pet.bark();
}
```

### Discriminated Unions
A common pattern in TypeScript involves creating a literal property that is common across multiple types. We can then narrow the type using that literal pattern.

`kind`, `type`, and `_type` are commonly used.

```TS
// kind isn't any string, it's the literal string "circle"
interface Circle {
	kind: "circle";
	radius: number;
}

interface Square {
	kind: "square";
	sideLength: number;
}

type Shape = Circle | Square;

function getArea(shape: Shape) {
	switch(shape.kind){
		case("circle"):
			return Math.PI * shape.radius * shape.radius;
		case("square"):
			return shape.sideLength * shape.sideLength;
	}
}
```

### Exhaustiveness Checks with `never`
`never` is a special type that can be assigned to any type but no type can be assigned to `never`, except `never` itself. 

This means you can use type narrowing and rely on `never` turning up to do exhaustive checking in a switch statement.

The compiler will complain if there is a case we didn't handle and it's now being assigned to `never`.

```TS
function getArea(shape: Shape) {
	switch(shape.kind){
		case("circle"):
			return Math.PI * shape.radius * shape.radius;
		case("square"):
			return shape.sideLength * shape.sideLength;
		default:
			// We should never get here if we handled all cases correctly
			const _exhaustiveCheck: never = shape;
			return _exhaustiveCheck;
	}
}
```

## Type Declarations
Type declaration files end in `.d.ts` and contain only type information that TypeScript can use to enforce type rules on our code. They don't produce `.js` outputs.

For example, TypeScript knows about `console` and all its methods because there is a `Console` interface defined in `lib.dom.d.ts`.

### Working with Third-Party Libraries
Increasingly, third-party libraries, such as Axios, will include their own TypeScript type declaration files.

In their `package.json`, they will have a `types` and/or `typings` property pointing to their type declaration files.

You can define a custom type using a Type Alias or Interface.

```TS
import axios from "axios";

interface User {
	id: number;
	name: string;
	username: string;
	email: string;
	address: {
		street: string;
		suite: string;
		city: string;
		zipcode: string;
		geo: {
			lat: string;
			long: string;	
		} 	
	}; 
	phone: string;
	website: string;
	company: {
		name: string;
		catchPhrase: string;
		bs: string;
	}
}

axios
	.get<User>("https://jsonplaceholder.typicode.com/users/1")
	.then((res) => {
		const { data } = res;
		printUser(data);
	})
	.catch((e) => {
		console.log("Error", e)
	});


function printUser(user: User) {
	console.log(`${user.name} ${user.email} ${user.phone}`)
}
```

### Installing Types Seperately
Some libraries, such as Lodash, do not come with a type declaration file. But the most popular libraries do have type declaration files that have been written for them. The [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) is particularly popular.

You have to install these seperately, which will make an `index.d.ts` file available.

```
npm install --save-dev @types/lodash
```

You can use https://www.typescriptlang.org/dt/search to search for type declaration files.

## Modules
The JavaScript module system remains kind of a fractured landscape.

TypeScript supports modules and lets us control the output it generates (CommonJS or ES Modules).

TypeScript technically has a proprietary module format called namespaces but most of its features are offered by ES Modules and the docs recommend just using ES Modules.

### Working Without Modules
The JavaScript specification declares that any JavaScript files without an `export` or top-level `await` should be considered a script, not a module.

Inside a script, any variables and types are declared to be in the shared global scope.

### Using TypeScript Modules
Any JavaScript file with an `export`, or top-level `await`, is considered a module. Each module is treted as its own namespace, and you will need to manually import and export any functionality you want to share.

Import statements can't use `.ts`, since the code will be compiled to JavaScript. So for a `utils` module, import it using `import { add } from "utils"`.

### Changing Compilation Module System
JavaScript in the browser doesn't understand CommonJS syntax.

You can configure TypeScript to use a different module system by updating the `module` property in your `tsconfig.json`.

```json
"module": "ES6"
```

To make modules work in the browser, you also need to specify `type="module"` when including your script.

```HTML
<script type="module" src="./dist/index.js"></script>
```

### Importing Types
Import statements that are purely for types won't be included in the compiled JavaScript. Export statements will be empty.

```TS
// types.ts
export interface Person {
	username: string;
	email: string;
}

export type Color = "red" | "green" | "blue";

// types.js
export {};
```

Babel can have issues importing types. TypeScript 3.8 added a new syntax to clarify import statements. 

```TS
import type { Person } from "types.js";
// or
import { type Person, otherThing } from "types.js";
```

## Webpack and TypeScript
Webpack helps us bundle assets into a single script to keep complex code organized while cutting down on the number of HTTP requests our web app needs to make.

In order to use Webpack from the command line or call it from within `package.json`, you also need webpack-cli.

It's a good practice to include TypeScript in your `package.json` to specify what version you're working with.

ts-loader acts as a middleman between TypeScript and Webpack. It compiles TypeScript to JavaScript and then hands it over to Webpack for bundling.

### Configuring Webpack
The basic Webpack config for TypeScript tells Webpack to enter through `./index.ts`, load all `.ts` and `.tsx` files through the `ts-loader`, and output a `bundle.js` file in our current directory.

```js
const path = require("path");

module.exports = {
  entry: "./src/index.ts",
  devtool: "inline-source-map",
  devServer: {
    static: {
      directory: path.join(__dirname, "./")
    }
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: "ts-loader",
        exclude: /node_modules/
      }
    ]
  },
  resolve: {
    extensions: [".tsx", ".ts", ".js"]
  },
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
    publicPath: "/dist"
  }
};
```

Then add a `"build": "webpack"` script to your `package.json`.

Webpack will get confused if the imports in your TypeScript files use `.js` extensions, so you should omit them. For example, `import Dog from "./Dog";`.

To make sense of the bundle, you  will want to set `sourceMap` to true in `tsconfig.json`.

The Webpack dev server acts as a live server, handles the bundling behind the scenes, and does it in memory instead of writing a seperate bundle file to disk every single time.

You could set up a common Webpack config file, a development config file and a production config file.

It's common to introduce a hash into the bundle filename to help the browser with caching, such as `filename: "[contenthash].bundle.js"`. This will lave multiple bundles in the dist directory, which can be countered using the `clean-webpack-plugin`.

## TypeScript and React
You can manually add type definitions to a React project. Create React App also has an option to create a new project with TypeScript.

```
npx create-react-app APP_NAME --template typescript
```

You can also add TypeScript to an existing Create React App project by insatlling the necessary packages and changing file extensions to `.tsx`.

```
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
```

TSX is the TypeScript flavour of JSX. TypeScript can infer the return type of React function components but you can also specify that their return type is `JSX.Element`. 

There isn't a lot of official documentation on React and TypeScript but there is  [React+TypeScript Cheatsheets](https://github.com/typescript-cheatsheets/react#reacttypescript-cheatsheets). It is opinionated but a good resource.

### Props with TypeScript
Just like any function, you can type the parameter of a React function component. In other words, you can type props.

```TS
function Greeter(props: { person: string }): JSX.Element {
  return <h1>Hello, { props.person }</h1>;
}
```

For more complex props objects, you can create a seperate type or interface for props. This works with destructured props too.

```TS
interface GreeterProps {
  person: string;
}

// with regular props
function Greeter(props: GreeterProps): JSX.Element {
  return <h1>Hello, { props.person }</h1>;
}

// with destructured props
function Greeter({ person }: GreeterProps): JSX.Element {
  return <h1>Hello, { person }</h1>;
}
```

Hooks can be typed with the value they should return. For example, `useState` needs to know the type of the state it will be managing.

```TS
export default interface Item {
  id: number;
  product: string;
  quantity: number;
}

const [items, setItems] = useState<Item[]>([]);
```

Types can be stored in a `/models` directory, or in a `models.ts` file in the same directory as the component.

With the `useRef` hook, provide only the type element as an argument and use `null` as the initial value.

```TS
const inputRef = useRef<HTMLInputElement>(null);
```

For example, you can access an input's value using `inputRef.current!.value`.

