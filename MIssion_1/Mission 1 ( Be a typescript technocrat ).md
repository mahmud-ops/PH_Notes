# Module 1

- nvm (node version manager) - can change, download, use different node version from CLI
- node 22.18.0+ can run ts

**literal data type**

A **TypeScript literal type** allows you to specify the exact, concrete value that a string, number, or boolean must have, rather than just accepting any value within that general type.
For example, `let direction: "north" | "south"` restricts the variable to only those two specific strings, preventing typos and providing excellent autocomplete in your editor.

```ts
// 1. String literal types in a union
type TrafficLight = "red" | "yellow" | "green";

let currentLight: TrafficLight = "red"; 
// Valid!

// currentLight = "blue"; 
// Error: Type '"blue"' is not assignable to type 'TrafficLight'.


// 2. Combining literal types with object shapes
interface Car {
  make: string;
  doors: 2 | 4;          // Number literal type
  isElectric: true;      // Boolean literal type
}

const myTesla: Car = {
  make: "Model 3",
  doors: 4,              // Must be exactly 2 or 4
  isElectric: true       // Must be exactly true
};
```

**Nullish coalescing**

The **nullish coalescing operator (`??`)** returns its right-hand side operand when its left-hand side operand is `null` or `undefined`. Unlike the logical OR (`||`) operator, it treats other falsy values—such as `0`, `false`, or `""`—as valid data, making it perfect for setting default values.

```ts
const userSettings = {
  volume: 0,
  username: ""
};

// Returns 0 because 'volume' is not null or undefined
const volumeLevel = userSettings.volume ?? 50; 

// If we used || here, it would evaluate to 50 because 0 is falsy
const alternativeVolume = userSettings.volume || 50; 

console.log(volumeLevel);      // Output: 0
console.log(alternativeVolume); // Output: 50
```


# Module 2 

## Generic interface

Generic interface in TypeScript lets you make a reusable type where one or more fields can accept different shapes while keeping the rest of the structure fixed.

Here `User<T>` is a generic type. The `T` acts as a placeholder for the smartWatch type, so each user can have a different kind of smartwatch without redefining the whole user structure.

Core idea:  
User structure stays same, only smartWatch changes based on type argument.

```ts
interface User<T> {
  name: string;
  age: number;
  device: {
    type: "computer" | "mobile";
    brand: string;
    model: string;
  };
  smartWatch: T;
}
```

Two smartwatch variants define different feature sets:

```ts
interface SmartWatch {
  heartRate: number;
  stopwatch: boolean;
}

interface ExpensiveSmartWatch {
  heartRate: number;
  stopwatch: boolean;
  AIfeats: boolean;
  call: boolean;
  calculator: boolean;
}
```

Now the same User type is reused with different smartwatches:

- `User<SmartWatch>` → minimal smartwatch features
    
- `User<ExpensiveSmartWatch>` → advanced smartwatch features
    

Example:

```ts
const poorDev: User<SmartWatch>
const richDev: User<ExpensiveSmartWatch>
```

TypeScript enforces correctness:

- poorDev cannot have AI/call/calculator fields
    
- richDev must include all required advanced features
    

Output shows normal JS objects at runtime, but TypeScript ensures structure safety during development.

```bash
{
  name: 'Mahmud',
  age: 21,
  device: { type: 'computer', brand: 'Dell', model: 'inspiron 15r 5521' },
  smartWatch: { heartRate: 72, stopwatch: false }
}
{
  name: 'Rich Mahmud',
  age: 21,
  device: { type: 'computer', brand: 'Dell', model: 'inspiron 15r 5521' },
  smartWatch: {
    heartRate: 70,
    stopwatch: true,
    call: true,
    calculator: true,
    AIfeats: true
  }
}
```

**Main takeaway:** 

Generics = reusable type blueprint where only specific parts (like smartWatch) change while keeping the overall schema strict and consistent.

## Generic function

TypeScript generic function that merges any object with a fixed course structure. The function is flexible because it accepts any type while preserving type safety.

- Uses generic `<T>` to accept any object shape
    
- Combines fixed object (`courseName`) with dynamic input
    
- Returns a merged object using spread operator
    
- Keeps original type properties intact in output


`addToCourse<T>(param1: T)` defines a generic function where `T` represents any input object type.  
Inside, a new object is returned containing a fixed property `courseName` plus all properties of `param1`.  
The spread operator ensures all fields from the input object are preserved in the returned object.  
TypeScript infers the final return type as intersection of `{ courseName: string } & T`.

Final full code:

```ts
const user_1 = {
  isStudent: true,
  name: "Mahmud",
};

const user_2 = {
  isTeacher: true,
  hasMac: true,
  id: 1234,
};

const addToCourse = <T>(param1: T) => {
  return {
    courseName: "Next Level",
    ...param1,
  };
};

console.log(addToCourse(user_1));
console.log(addToCourse(user_2));
```

Output:

```bash
{ courseName: 'Next Level', isStudent: true, name: 'Mahmud' }
{ courseName: 'Next Level', isTeacher: true, hasMac: true, id: 1234 }
```

## Constrain

Constraint in TypeScript generics limits what types a generic can accept.  
Using `extends`, you enforce that the type must satisfy a specific structure.  
This ensures type safety while still allowing flexible, extended shapes.

This shows a **generic with constraint (`T extends User`)** that enforces a minimum structure while still allowing extra properties. However, it also exposes a common pitfall: TypeScript won’t catch typos in additional fields.

- Ensures `id`, `name`, `email` must exist
    
- Allows extra fields like `role`, `teacherId`
    
- Typos in extra fields (e.g., `teaherId`) go unchecked
    
- Constraint ≠ strict validation of full object shape
    

Snippet explanation:

```ts
const addToCourse = <T extends User>(param1: T) => {
```

Here:

- `T` must satisfy `User`
    
- TypeScript checks only required fields from `User`
    
- Extra properties are accepted blindly → why `teaherId` is not flagged
    
- Return type becomes `{ courseName: string } & T`, preserving all fields (even wrong ones)
    

Final full code:

```ts
interface User {
  id: number;
  name: string;
  email: string;
}

const user1 = {
  id: 123,
  name: "Mahmud",
  email: "abdullahmahmud0189@gmail.com",
  role: "student",
};

const user2 = {
  id: 213,
  name: "Rakib",
  email: "rakib89@gmail.com",
  role: "teacher",
  teaherId: 1234, // typo (not caught)
};

const addToCourse = <T extends User>(param1: T) => {
  return {
    courseName: "Next Level",
    ...param1,
  };
};

console.log(addToCourse(user1));
console.log(addToCourse(user2));
```

Output:

```bash
{
  courseName: 'Next Level',
  id: 123,
  name: 'Mahmud',
  email: 'abdullahmahmud0189@gmail.com',
  role: 'student'
}

{
  courseName: 'Next Level',
  id: 213,
  name: 'Rakib',
  email: 'rakib89@gmail.com',
  role: 'teacher',
  teaherId: 1234
}
```

# Module 3 (OOP)


## Parameter Property (Constructor Shortcut)

TypeScript provides a cleaner way to define and initialize class properties directly in the constructor. This reduces boilerplate and makes the class more concise.

**Explanation**  
In the normal approach, properties are declared separately and then assigned inside the constructor. With parameter properties, adding `public` (or `private/protected`) in constructor parameters automatically creates and initializes those properties. This removes the need for manual assignments like `this.name = name`.

**Normal Code**

```ts
class Animal {
  name: string;
  type: string;
  sound: string;

  constructor(name: string, type: string, sound: string) {
    this.name = name;
    this.type = type;
    this.sound = sound;
  }

  makeSound(): void {
    console.log(`${this.name} says ${this.sound}`);
  }
}

// instances
const animal1 = new Animal("Dog", "Mammal", "Bark");
const animal2 = new Animal("Cat", "Mammal", "Meow");

animal1.makeSound();
animal2.makeSound();
```

**Parameter Property Code**

```ts
class Animal {
  constructor(
    public name: string,
    public type: string,
    public sound: string
  ) {}

  makeSound(): void {
    console.log(`${this.name} says ${this.sound}`);
  }
}

// instances
const animal1 = new Animal("Dog", "Mammal", "Bark");
const animal2 = new Animal("Cat", "Mammal", "Meow");

animal1.makeSound();
animal2.makeSound();
```

**Output**

```txt
Dog says Bark
Cat says Meow
```

## **OOP Features in TypeScript (Single Clean Example)**  

This example demonstrates the main OOP concepts: class structure, inheritance, polymorphism, encapsulation, and constructor parameter properties. It models a simple animal hierarchy.

**Explanation**  
A base class `Animal` stores shared data like type, genus, and species using constructor parameter properties with `readonly` for immutability. It also provides common methods like `getInfo()` and `makeSound()`.

A child class `Dog` extends `Animal`, fixes the type as `"dog"`, and overrides `makeSound()` to show polymorphism (same method name, different behavior).

**Full Code**

```ts
class Animal {
    constructor(
        public readonly type: string,
        public readonly genus: string,
        public readonly species: string
    ) {}

    getInfo(): void {
        console.log(`${this.type}'s scientific name: ${this.genus} ${this.species}`);
    }

    makeSound(): void {
        console.log(`${this.type} is making sound.`);
    }
}

class Dog extends Animal {
    constructor(genus: string, species: string) {
        super("dog", genus, species);
    }

    makeSound(): void {
        console.log("The dog is barking.");
    }
}

// Usage example
const myDog = new Dog("Canis", "lupus");

myDog.getInfo();
myDog.makeSound();
```

**Output**

```txt
dog's scientific name: Canis lupus
The dog is barking.
```

## **TypeScript Abstraction**

**Intro**  
Abstraction in TypeScript is an OOP concept used to hide implementation details and show only essential features.  
It is used to reduce complexity and expose only what is necessary to the user or developer.

**Explanation**

- Abstraction focuses on _what an object does_, not _how it does it_
    
- Achieved using **abstract classes** and **abstract methods**
    
- Abstract class cannot be instantiated directly
    
- Abstract methods must be implemented in derived classes
    
- Helps enforce a consistent structure across related classes
    

Key idea:

- Base class defines a contract
    
- Child classes provide specific implementation
    

**Code (Normal Version)**

```ts
abstract class Shape {
  abstract area(): number;

  display(): void {
    console.log("This is a shape");
  }
}

class Circle extends Shape {
  radius: number;

  constructor(radius: number) {
    super();
    this.radius = radius;
  }

  area(): number {
    return Math.PI * this.radius * this.radius;
  }
}

class Rectangle extends Shape {
  width: number;
  height: number;

  constructor(width: number, height: number) {
    super();
    this.width = width;
    this.height = height;
  }

  area(): number {
    return this.width * this.height;
  }
}
```


**Output**

```txt
Area: 78.53981633974483
Area: 50
```

**Real World Mapping (Web Dev)**

- Backend APIs use abstraction to define service contracts (e.g., payment gateway interfaces)
    
- Frontend components share abstract behavior (e.g., base UI components like Button, Modal)
    
- Helps enforce consistent structure across large codebases (React services, NestJS architecture)
    

**One Line Summary**  
Abstraction in TypeScript hides implementation details and forces derived classes to follow a shared contract.