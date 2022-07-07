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
}

const sayHappyBirthday = (person: Person) => {
	return `Hey ${person.name}, congrats on turning ${person.age}!`;
}

sayHappyBirthday({ name: "Jerry", age: 42 });
```