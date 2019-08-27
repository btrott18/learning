# Learning Typescript

Assumes we are using node.

TODO:
1. Min version of node/npm?

# Getting Started

Create a node project and execute the normal `npm init`

## Install the typescript compiler

The typescript compiler transpiles typescript code to javascript -- this is good if you are deploying to the web or to any JS runtime environment.

```    
npm install -g typescript
```

In order to run the typescript compiler against a file, execute the following:
```
tsc myfile.ts
```

The output of this command will be `myfile.js`, which can be run using `node myfile`


## Install the typescript node runner

The typescript node runner shortcuts the tsc step, so you can run your typescipt file via node in a single command.

```
npm install -g ts-node
```

Additionally install `typescript` as a local dependency in your project, to ensure you are using the correct version.
```
npm install typescript
```


With `ts-node` and `typescript` installed, you can now execute `myfile.ts` using a single command:
```
ts-node myfile
```


## Types

When defining a property or variable in typescript, you should include the type:

```
let v: number = 1;
```

If you do not assign a value of the correct type, then an error will occur:
```
let v: number = "Some string"; //compiler error
```

Available types are:

* boolean 
* number 
* string 
* array 
	> `let arr: number[] = [1,2,3];` or `let arr: list<number> = [1,2,3];`
* tuple 
	> `let myTuple: [string, number] = [1,2];` 
	> Can be destructured using `let [first, second] = myTuple;`
	> still need to be accessed by the index, however compiler/runtime know the `type` that the data should be in each index.

* enum 
	> `enum foods {pizza, cereal, nachos}`
	> `foods.pizza` or `foods[0] //pizza`
	> TODO:  Read up on enums

* any -- Dynamic, allows you to switch types.  Works as an opt-out of type-checking at compile time. 
```
	let something: any = true;
	something = "I don't actually know";
	something = 0;
```

void - when a function has no return type
`function myFunction(): void {`

`never` - return type for a function that never returns anything b/c it is either an infinite loop, or throws an error.
```typescript

function notComingBack(): never {
	throw new Error("BROKE!!!");
}

```



null
undefined

--strictNullChecks

uniontypes


### Type assertions (type-casting)
Come in 2 flavors:
```typescript

let myVar: any = "hello world";

(<string> myVar).toUpperCase();
(myVar as string).toUpperCase();
```

When using JSX, only the `as` type assertion is allowed.  


### Destructuring
Good documentation here: https://www.typescriptlang.org/docs/handbook/variable-declarations.html

1.  Good info on destructuring in general.
2.  Good info on overrides/defaults/optionals.



## Interfaces
```typescript

interface MyType {
	requiredProp: string;
	optionalProp?: number;
	readonly readOnlyProp: number; //cannot be modified
	[propName: string]: any; //allow other properties to be added to this object
}

function calculateSomething(param: MyType): void {
	console.log(param.requiredProp);
}

```

### Difference between `readonly` and `const`
* `const` is used for variables
* `readonly` is used for properties of objects


### Interface for a function:
```typescript
interface PrintString{
	(stringToPrint: string): void
}

let myfunc: PrintString;
myfunc = function(str: string): void {
	console.log(str);
}

```

### Interface for an indexable type:

```typescript
interface MyList {
	[index: number]: string;
}

let myArr: MyList;
myList = ["str1", "str2"];
```

#### More than one allowable type at a given index 

```typescript
interface MyCollection {
	[index: number]: number | string;
}

let myArr: MyCollection;
myList = ["str1", "str2", 1];
```

#### readonly array
```typescript
interface MyCollection {
	readonly [index: number]: string;
}

//must be assigned value at declaration time
let myArr: MyCollection = ["item1", "item2"];

```

#### class interface implementation
```typescript
interface Serializable {
	toString(): string;
}
class MyClass implements Serializable {
	toString(): string {
		return "Hello World";
	}
}
```

#### interfaces can extend other interfaces
```typescript
interface Shape {
	color: string;
}

interface Circle extends Shape {
	radius: number;
}
```

#### Interfaces can also extend multiple other interfaces (separated by commas)
```typescript
interface Interface extends OtherInterface1, OtherInterface2 {...}
```

#### Interface for hybrid type 

```typescript
//Example function with properties
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}
```

#### Additional interface notes
* Interfaces can extend classes -- the interface picks up the properties/functions, however not the implementations.



## Classes
```typescript
//interface definitions -- as a rule, should we define all interfaces in separate files?


//class definition
class MyClass extends ParentClass implements Interface {
	
	//static property only accessible on class, not object instance
	//example:  MyClass.className
	static className = "MyClass";

	//publically accessible by default
	prop1: string; 

	//publically accessible when specified public accessor
	public prop2: string; 
	
	//not accessible outside of the class when accessor is private
	private prop3: string; 

	//only accessible to sub-classes of this class
	protected prop4: string;

	//readonly must be initialized in the definition, or in the constructor.  
	//immutable value
	readonly prop5: string = "hello world";

	constructor(param1: string) {
		super();
		//init this instance of the class
	}

	//constructor can only be called from this class or a sub-class
	protected constructor(param1: string, param2: string) {
		super();
		//some other stuff
	}
}

```

### Accessors (getters/setters)

The accessor pattern allows what feels like direct access to properties, however the access is proxied through the corresponding getter or setter.

```typescript
class Person {
	private _fullName: string;

	get fullName(): string {
		return `Hello ${this._fullName}`;
	}

	set fullName(name: string) {
		this._fullName = name;
	}

}

let myPerson: Person = new Person();
myPerson.fullName = "My Name";
console.log(myPerson.fullName); //Hello My Name

```


### Abstract Class

Class that cannot be instantiated directly, and can only be extended.

```typescript
abstract class Parent {

	//this function must be implemented by any class that extends this abstract class
	abstract doThing(): void;

	//this function has a default implementation in the abstract class
	addValues(val1: number, val2: number): number {
		return val1 + val2;
	}
}
```

## Functions

### General syntax:
```
	function add(number1: number, number2: number): number {
		return number1 + number2;
	}
```

### Default values
```
	function add(number1: number, number2: number = 0): number {
		return number1 + number2;
	}
	
	add(1, 2); 	//3
	add(1); 	//1
```

### Optional values
```
	function add(number1: number, number2?: number): number {
		if(number2) {
			return number1 + number2;	
		}
		return number1;
	}
	
	add(1, 2); 	//3
	add(1); 	//1
```

Notes sourced from:  https://www.typescriptlang.org/docs/home.html

