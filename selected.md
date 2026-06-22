**Avoid Redundant Information**

```ts
class User {
  constructor(
    public name: string,
    public age: number,
  ) {}
}

// Bad: we already know a User has a name and age
const userWithNameAndAge = new User("Max", 31);

// Good: concise and clear
const user = new User("Max", 31);
console.log(user);
// User { name: 'Max', age: 31 }
```

**Be Consistent**

```js
function getUsers() {
  return [];
}
function fetchProducts() {
  return [];
}
```

**When good naming isn't enough, especially for regex**

```ts
// Min. 8 characters, at least: one letter, one number
const passwordRegex = /^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$/;
```

**Avoid functions where a boolean flag splits behavior**

```ts
// Bad: boolean flag hides the function's true behavior
function log(message: string, isError: boolean): void {
  if (isError) {
    console.error(message);
  } else {
    console.log(message);
  }
}
log("Hi there!", false); // unclear what "false" means

// Good: separate functions, each does one thing
function logMessage(message: string): void {
  console.log(message);
}

function logError(errorMessage: string): void {
  console.error(errorMessage);
}

logMessage("Hi there!");
logError("An error!");
```

**Don't Repeat Yourself**

```ts
// Not DRY: repeated validation logic across functions
function createUserBad(email: string): void {
  if (!email || !email.includes("@")) {
    console.log("Invalid email!");
    return;
  }
  console.log("Saving user:", email);
}

function createSupportChannelBad(email: string): void {
  if (!email || !email.includes("@")) {
    console.log("Invalid email. Could not create channel");
    return;
  }
  console.log("Support channel created for:", email);
}

createUserBad("bademail", "");
// Invalid email!

createSupportChannelBad("test@test.com");
// Support channel created for: test@test.com
```

```ts
// DRY: shared validation logic reused across functions
function createUser(email: string): void {
  if (!isEmail(email)) {
    console.log("Invalid email!");
    return;
  }
  saveUser(email, password);
}

function createSupportChannel(email: string): void {
  if (!isEmail(email)) {
    console.log("Invalid email!");
    return;
  }
  console.log("Support channel created for:", email);
}

function isEmail(email: string): boolean {
  return !!email && email.includes("@");
}
```

**Avoid deep nesting: use guards**

```ts
// Bad: deeply nested, hard to follow
function messageUser(user: any, message: string): void {
  if (user) {
    if (message) {
      if (user.acceptsMessages) {
        const success = user.sendMessage(message);
        if (success) {
          console.log("Message sent!");
        }
      }
    }
  }
}

messageUser({ acceptsMessages: true, sendMessage: () => true }, "Hello!");
// Message sent!

messageUser(null, "Hello!");
// (returns early, nothing printed)
```

```ts
// Good: guards at the top, flat code body
function messageUser(user: any, message: string): void {
  if (!user || !message || !user.acceptsMessages) {
    return;
  }

  const success = user.sendMessage(message);
  if (success) {
    console.log("Message sent!");
  }
}

messageUser({ acceptsMessages: true, sendMessage: () => true }, "Hello!");
// Message sent!

messageUser(null, "Hello!");
// (returns early, nothing printed)
```

**Use inheritance and polymorphism to eliminate `if` statements.**

```ts
// Bad: multiple if/else statements
class AnimalSound {
  makeSound(animal: string): void {
    if (animal === "dog") {
      console.log("Woof");
    } else if (animal === "cat") {
      console.log("Meow");
    } else if (animal === "bird") {
      console.log("Tweet");
    } else {
      console.log("Unknown sound");
    }
  }
}

const soundMaker = new AnimalSound();
soundMaker.makeSound("dog"); // Woof
```

```ts
// Good: polymorphism
interface Animal {
  makeSound(): void;
}

class Dog implements Animal {
  makeSound(): void {
    console.log("Woof");
  }
}

class Cat implements Animal {
  makeSound(): void {
    console.log("Meow");
  }
}

class Bird implements Animal {
  makeSound(): void {
    console.log("Tweet");
  }
}

// Now you just call the method on the object itself
const myDog: Animal = new Dog();
myDog.makeSound(); // Woof
```

**Law Of Demeter: Don't depend on the internals of "strangers."**

```ts
// Bad: The Driver reaches through the Car to start the Engine.
class Engine {
  start(): void {
    console.log("Engine starting...");
  }
}

class BadCar {
  engine = new Engine();
}

class BadDriver {
  startVehicle(car: BadCar): void {
    // Violation: Driver shouldn't know that the Car has an Engine!
    car.engine.start();
  }
}

const badDriver = new BadDriver();
badDriver.startVehicle(new BadCar());
// Engine starting...
```

```ts
// Good: The Driver asks the Car to start. The Car manages its own Engine.
class GoodCar {
  private engine = new Engine(); // (Assuming Engine class is defined above)

  start(): void {
    this.engine.start();
  }
}

class GoodDriver {
  startVehicle(car: GoodCar): void {
    car.start();
  }
}

const goodDriver = new GoodDriver();
goodDriver.startVehicle(new GoodCar());
// Engine starting...
```

**Avoid unexpected side effects: modifying a global variable**

```ts
// Impure: modifies external state. This causes unpredictable bugs!
let currentDiscount = 0;

function calculateTotal(price: number): number {
  currentDiscount = 10; // <-- side effect: changes external variable
  return price - currentDiscount;
}
function printDiscount(): void {
  console.log(`Your discount is $${currentDiscount}`);
}
// BUG: The behavior depends entirely on the order of function calls
printDiscount(); // Your discount is $0
calculateTotal(100);
printDiscount(); // Your discount is $10 (Unexpected state change!)
```
