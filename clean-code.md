# Clean Code Complete Guide

![Bidur Sapkota](https://www.bidursapkota.com.np/images/gravatar.webp "Bidur Sapkota - Developer")&nbsp;[Bidur Sapkota](https://www.bidursapkota.com.np/)

![Clean Code Complete Guide by Bidur Sapkota](clean-code-1200.webp "Clean Code Complete Guide – Blog by Bidur Sapkota")

## Table of Contents

1. [What Is Clean Code?](#what-is-clean-code)
2. [Naming](#naming)
3. [Comments & Formatting](#comments--formatting)
4. [Functions & Methods](#functions--methods)
5. [Control Structures & Errors](#control-structures--errors)
6. [Classes, Objects & Data Containers](#classes-objects--data-containers)
7. [The SOLID Principles](#the-solid-principles)
8. [Clean Code Checklist](#clean-code-checklist)

---

## What Is Clean Code?

Clean code is code that is easy to read, understand, and maintain. It's not about whether code works. A vast majority of development time is spent reading and understanding code, not writing it. Clean code should be readable and meaningful, avoid unintuitive names, complex nesting, and big code blocks, reduce cognitive load, and be concise and "to the point." Clean code is written over time through continuous refactoring and improvement.

### Clean Code vs Clean Architecture

| Clean Code                                 | Clean Architecture                     |
| ------------------------------------------ | -------------------------------------- |
| Focuses on how to write the code           | Focuses on where to write which code   |
| Deals with single problems or files        | Deals with the project as a whole      |
| Aims to be readable and easy to understand | Aims to be maintainable and extensible |

### Common Problems

- Names are often unclear for variables, functions, and classes
- Functions tend to have too many parameters, run too long, and do multiple things
- Comments and formatting suffer from redundant comments and poor vertical/horizontal spacing
- Conditionals and errors lead to deep nesting and missing error handling
- Classes and objects become bloated, with no clear distinction between objects and data containers

### Embrace Refactoring

Refactoring today is work you save tomorrow. A codebase can only survive and stay maintainable if it's continuously improved and refactored. Whenever you add something new, try to improve existing code along the way.

---

## Naming

Naming things (variables, properties, functions, methods, classes) correctly and in an understandable way is an extremely important part of writing clean code. If poor names are chosen, pretty much all other clean code concepts will not help that much.

### Why Names Matter

Well-named "things" allow readers to understand your code without going through it in detail.

```ts
class User {
  name = "Alice";
}
const database = {
  insert(user: User) {
    console.log("Inserted:", user.name);
  },
};

const user = new User();
database.insert(user);
// Inserted: Alice

const isLoggedIn = true;
if (isLoggedIn) {
  console.log("User is logged in");
}
// User is logged in
```

To understand the above code, we don't need to go through the full class or function definitions. The names do all the heavy lifting.

### How To Name Things

| Type                   | Convention            | Examples                                    |
| ---------------------- | --------------------- | ------------------------------------------- |
| Variables & Properties | Nouns or noun phrases | `user`, `product`, `isValid`, `emailExists` |
| Functions & Methods    | Verbs or verb phrases | `login()`, `createUser()`, `isEmail()`      |
| Classes                | Nouns                 | `User`, `Product`, `Transaction`, `Payment` |

### Name Casing

| Style        | Usage                         | Example       |
| ------------ | ----------------------------- | ------------- |
| `camelCase`  | Variables, functions, methods | `isValid`     |
| `PascalCase` | Classes, interfaces, types    | `AdminRole`   |
| `UPPER_CASE` | Constants, enum values        | `MAX_RETRIES` |

### Naming Variables & Properties

The name should imply which kind of data is being stored. For objects, use descriptive nouns. For booleans, answer a true/false question.

```ts
// Bad: what does "d" or "val" contain?
const d = new Date();
const val = true;

// Good: descriptive, value type is clear
const currentDate = new Date();
const isLoggedIn = true;
```

```ts
function fetchFromDatabase(table: string, id: string) {
  return { id, table, name: "Widget" };
}

// Bad: too generic, could be anything
const data = { name: "Alice", age: 28 };
const item = fetchFromDatabase("products", "p1");

// Good: specific, immediately understandable
const customer = { name: "Alice", age: 28 };
const product = fetchFromDatabase("products", "p1");

console.log(customer);
// { name: 'Alice', age: 28 }
console.log(product);
// { id: 'p1', table: 'products', name: 'Widget' }
```

Be as specific as context allows. Prefer `customer` over `user` if the code is doing customer-specific operations.

### Naming Functions & Methods

Functions perform tasks, so their names should describe what they do. Use verbs.

```ts
// Bad: unclear what "process" or "handle" does
function process(data: string): void {
  console.log(data);
}

// Good: intent is immediately clear
function logMessage(message: string): void {
  console.log(message);
}
```

```ts
const database = {
  find(table: string, id: string) {
    return { email: "max@test.com", id };
  },
};

// Bad: sounds like a property, not a function
function email(userId: string): string {
  return database.find("users", userId).email;
}

// Good: verb phrase makes the action obvious
function getEmail(userId: string): string {
  return database.find("users", userId).email;
}

console.log(getEmail("u1"));
// max@test.com
```

For boolean-returning functions, phrase the name as a yes/no question.

```ts
interface User {
  permissions: string[];
}

function isValidEmail(email: string): boolean {
  return email.includes("@") && email.includes(".");
}

function hasPermission(user: User, action: string): boolean {
  return user.permissions.includes(action);
}

// Usage reads like English
const input = "test@example.com";
if (isValidEmail(input)) {
  console.log("Valid email");
}
// Valid email

const admin: User = { permissions: ["read", "write", "delete"] };
console.log(hasPermission(admin, "write"));
// true
console.log(hasPermission(admin, "deploy"));
// false
```

### Naming Classes

Class names should describe the kind of object it will create. Use nouns and avoid redundant suffixes.

```ts
// Bad: "UEntity" and "ObjA" are meaningless
class UEntity {}
class ObjA {}

// Okay but redundant: "Obj" adds no information
class UserObj {}

// Good: clean, descriptive nouns
class User {}
class Admin {}
class SQLDatabase {}
```

### Avoid Redundant Information

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

### Avoid Slang, Abbreviations & Disinformation

```ts
const accounts = [
  { name: "Alice", active: true },
  { name: "Bob", active: false },
  { name: "Charlie", active: true },
];

// Bad: slang, abbreviation, disinformation
const ymdt = "20210121CET";
const userList = { u1: "Alice", u2: "Bob" }; // not a list, it's an object
const allAccounts = accounts.filter((a) => a.active);

// Good: clear, accurate, descriptive
const dateWithTimezone = "20210121CET";
const userMap = { u1: "Alice", u2: "Bob" };
const activeAccounts = accounts.filter((a) => a.active);

console.log(activeAccounts);
// [ { name: 'Alice', active: true }, { name: 'Charlie', active: true } ]
```

### Be Consistent

If you used `fetchUsers()` in one part of your code, use `fetchProducts()` (not `getProducts()`) in another part. Pick one convention and stick with it throughout the entire codebase.

```ts
type User = { name: string };
type Product = { title: string };
type Order = { id: string };

// Inconsistent: mixing "get", "fetch", "retrieve"
function getUsers(): User[] {
  /* ... */ return [];
}
function fetchProducts(): Product[] {
  /* ... */ return [];
}
function retrieveOrders(): Order[] {
  /* ... */ return [];
}

// Consistent: always use the same verb
function fetchUsers(): User[] {
  /* ... */ return [];
}
function fetchOrders(): Order[] {
  /* ... */ return [];
}

console.log(fetchUsers());
// []
console.log(fetchOrders());
// []
```

### Choose Distinctive Names

```ts
// Bad: methods sound too similar, hard to tell apart
class AnalyticsBad {
  getDailyData(day: string) {
    return `data-${day}`;
  }
  getDayData() {
    return "today-data";
  }
  getRawDailyData(day: string) {
    return `raw-${day}`;
  }
  getParsedDailyData(day: string) {
    return `parsed-${day}`;
  }
}

// Good: each method is clearly distinct
class Analytics {
  getDailyReport(day: string) {
    return `report-${day}`;
  }
  getDataForToday() {
    return "today-report";
  }
  getRawDailyData(day: string) {
    return `raw-${day}`;
  }
  getParsedDailyData(day: string) {
    return `parsed-${day}`;
  }
}

const analytics = new Analytics();
console.log(analytics.getDailyReport("2026-06-18"));
// report-2026-06-18
console.log(analytics.getDataForToday());
// today-report
```

### Naming Demo

```ts
// Bad: cryptic names, abbreviations, unclear intent
class Entity {
  title: string;
  desc: string;
  ymdhm: string;

  constructor(title: string, desc: string, ymdhm: string) {
    this.title = title;
    this.desc = desc;
    this.ymdhm = ymdhm;
  }
}

function output(item: Entity): void {
  console.log("Title: " + item.title);
  console.log("Description: " + item.desc);
  console.log("Published: " + item.ymdhm);
}

const summary = "Clean Code Is Great!";
const desc = "Actually, writing Clean Code can be pretty fun.";
const publish = new Date().toISOString().slice(0, 16).replace("T", " ");

const item = new Entity(summary, desc, publish);
output(item);
```

`Entity`, `desc`, `ymdhm`, `output`, and `item` are all vague. Let's improve:

```ts
// Clean: descriptive names, clear intent
class BlogPost {
  title: string;
  description: string;
  publishDate: string;

  constructor(title: string, description: string, publishDate: string) {
    this.title = title;
    this.description = description;
    this.publishDate = publishDate;
  }
}

function printBlogPost(post: BlogPost): void {
  console.log("Title: " + post.title);
  console.log("Description: " + post.description);
  console.log("Published: " + post.publishDate);
}

const title = "Clean Code Is Great!";
const description = "Actually, writing Clean Code can be pretty fun.";
const publishDate = new Date().toISOString().slice(0, 16).replace("T", " ");

const post = new BlogPost(title, description, publishDate);
printBlogPost(post);
// Title: Clean Code Is Great!
// Description: Actually, writing Clean Code can be pretty fun.
// Published: 2026-06-18 10:30
```

---

## Comments & Formatting

You might think comments help with code readability. In reality, the opposite is often the case. Proper code formatting (keeping lines short, adding blank lines, grouping related code) helps a lot more with reading and understanding code.

### Bad Comments

There are several kinds of bad comments you should avoid:

**Dividers & Markers**: redundant if your code uses proper names.

```ts
// Bad: dividers clutter the file and stop reading flow
// !!!!!!!!!
// CLASSES
// !!!!!!!!!
class User {
  save() {}
}
class Product {
  update() {}
}

// Good: clean names make dividers unnecessary
class User {
  save() {}
}

class Product {
  update() {}
}
```

**Redundant Information**: restating what the code already says.

```ts
// Bad: the comment adds nothing the name doesn't already say
function createUser() {
  // creating a new user
}

// Good: the function name speaks for itself
function createUser() {
  // ...
}
```

**Commented-Out Code**: delete it. Use source control (Git) to bring back old code if needed.

```ts
// Bad: clutters the file
function createUser() {
  // ...
}
// function createProduct() {
//   ...
// }

// Good: remove it, Git has the history
function createUser() {
  // ...
}
```

**Misleading Comments**: probably the worst kind. They confuse the reader.

```ts
// Bad: is this logging in or creating a user?
function login() {
  // create a new user
}
```

### Good Comments

While most comments are bad, a few types are acceptable:

**Legal Information:**

```ts
// (c) 2026 MyCompany Inc.
// Licensed under MIT
```

**Required Explanations**: when good naming isn't enough, especially for regex.

```ts
// Min. 8 characters, at least: one letter, one number, one special character
const passwordRegex =
  /^(?=.*[A-Za-z])(?=.*\d)(?=.*[@$!%*#?&])[A-Za-z\d@$!%*#?&]{8,}$/;
```

**Warnings:**

```ts
function fetchTestData(): void {
  // Warning: requires local dev server to be running on port 3000
}
```

**Todo Notes**: acceptable in moderation.

```ts
function login(email: string, password: string): void {
  // TODO: add password hashing before comparison
  if (email && password) {
    console.log("Logged in");
  }
}
```

**Docstrings**: useful for documenting public APIs, libraries, or complex types so IDEs can provide better autocompletion and hints.

```ts
interface CartItem {
  name: string;
  price: number;
  quantity: number;
}

/**
 * Calculates the total price of the items in the cart, applying any valid discounts.
 *
 * @param items - The array of items in the shopping cart.
 * @param discountCode - An optional discount code to apply.
 * @returns The final total price.
 */
function calculateTotal(items: CartItem[], discountCode?: string): number {
  let total = items.reduce((sum, item) => sum + item.price * item.quantity, 0);

  if (discountCode === "SAVE10") {
    total *= 0.9;
  }

  return total;
}

const cart = [
  { name: "Laptop", price: 1000, quantity: 1 },
  { name: "Mouse", price: 50, quantity: 2 },
];

console.log(calculateTotal(cart, "SAVE10"));
// 990
```

### Vertical Formatting

Code should be readable like an essay, top to bottom, without too many "jumps."

Add blank lines to separate distinct concepts. Keep related concepts close together.

```ts
class User {
  password: string;
  constructor(
    public email: string,
    password: string,
  ) {
    this.password = password;
  }
  saveToDatabase(): void {
    console.log("Saved user:", this.email);
  }
}

function findUserByEmail(email: string) {
  return { password: "hashed_secret", email };
}
function compareEncryptedPassword(stored: string, input: string): boolean {
  return stored === "hashed_secret";
}
function createSession(): void {
  console.log("Session created");
}

// Bad: no spacing, hard to see where one concept ends and another begins
function login(email: string, password: string): void {
  if (!email.includes("@") || password.length < 7) {
    throw new Error("Invalid input!");
  }
  const user = findUserByEmail(email);
  const passwordIsValid = compareEncryptedPassword(user.password, password);
  if (passwordIsValid) {
    createSession();
  } else {
    throw new Error("Invalid credentials!");
  }
}
function signup(email: string, password: string): void {
  if (!email.includes("@") || password.length < 7) {
    throw new Error("Invalid input!");
  }
  const user = new User(email, password);
  user.saveToDatabase();
}

login("max@test.com", "secret123");
// Session created
signup("alice@test.com", "password1");
// Saved user: alice@test.com
```

```ts
// Good: blank lines separate concepts, related code stays together
function login(email: string, password: string): void {
  if (!email.includes("@") || password.length < 7) {
    throw new Error("Invalid input!");
  }

  const user = findUserByEmail(email);
  const passwordIsValid = compareEncryptedPassword(user.password, password);

  if (passwordIsValid) {
    createSession();
  } else {
    throw new Error("Invalid credentials!");
  }
}

function signup(email: string, password: string): void {
  if (!email.includes("@") || password.length < 7) {
    throw new Error("Invalid input!");
  }

  const user = new User(email, password);
  user.saveToDatabase();
}

login("max@test.com", "secret123");
// Session created
signup("alice@test.com", "password1");
// Saved user: alice@test.com
```

The extra blank lines separate validation from the core logic and separate the two functions. Related lines (like creating a user and saving it) stay together.

**Stepdown Rule**: a function A called by function B should be placed closely below function B:

```ts
const database = {
  insert(table: string, data: any) {
    console.log(`Inserted into ${table}:`, data);
  },
};

// Good: reads top to bottom like a story
function processSignup(email: string, password: string): void {
  validateInput(email, password);
  saveUser(email, password);
}

function validateInput(email: string, password: string): void {
  if (!email.includes("@") || password.length < 7) {
    throw new Error("Invalid input!");
  }
}

function saveUser(email: string, password: string): void {
  const user = { email, password, id: generateId() };
  database.insert("users", user);
}

function generateId(): string {
  return Math.random().toString(36).substring(2, 10);
}

processSignup("max@test.com", "secret123");
// Inserted into users: { email: 'max@test.com', password: 'secret123', id: '...' }
```

### Horizontal Formatting

Lines should be readable without scrolling. Break long lines into multiple shorter ones.

```ts
function login(email: string, password: string) {
  console.log(`Logged in as ${email}`);
  return { email };
}
function getValidatedEmail() {
  return "default@test.com";
}
function getValidatedPassword() {
  return "default123";
}

// Bad: too much logic crammed into one line
let email: string | undefined = undefined;
let password: string | undefined = undefined;
const loggedInUser =
  email && password
    ? login(email, password)
    : login(getValidatedEmail(), getValidatedPassword());
// Logged in as default@test.com

// Good: same logic, but readable
let email2: string | undefined = "max@test.com";
let password2: string | undefined = "secret123";
if (!email2 || !password2) {
  email2 = getValidatedEmail();
  password2 = getValidatedPassword();
}
const loggedInUser2 = login(email2, password2);
// Logged in as max@test.com
```

Avoid overly long names that waste horizontal space:

```ts
function login(email: string, password: string) {
  console.log(`Logged in as ${email}`);
  return { email };
}

// Bad: unnecessarily verbose
const loggedInUserAuthenticatedByEmailAndPassword = login(
  "test@test.com",
  "secret",
);
// Logged in as test@test.com

// Good: concise but descriptive
const authenticatedUser = login("test@test.com", "secret");
// Logged in as test@test.com
```

---

## Functions & Methods

Functions are the meat of any code we write. All our code lives inside functions or methods. That's why it's extremely important to write clean functions. Functions are made up of three main parts: their name, their parameters, and their body.

### Minimize The Number Of Parameters

The fewer parameters a function has, the easier it is to read and call.

| Count | Example                  | Readability        | Recommendation       |
| ----- | ------------------------ | ------------------ | -------------------- |
| 0     | `user.save()`            | Easy to understand | Best possible option |
| 1     | `log(message)`           | Easy to understand | Very good option     |
| 2     | `login(email, password)` | Decent             | Use with caution     |
| 3     | `calc(5, 10, "add")`     | Challenging        | Avoid if possible    |
| > 3   | `coords(10, 3, 9, 12)`   | Difficult          | Always avoid         |

### Zero Parameters

When a function needs no external data, it's the easiest to read and call:

```ts
class User {
  email: string;
  password: string;
  id: string;

  constructor(email: string, password: string) {
    this.email = email;
    this.password = password;
    this.id = Math.random().toString(36).substring(2, 10);
  }

  save(): void {
    console.log(`Saving user ${this.email} to database...`);
  }
}

const user = new User("test@test.com", "testers");
user.save();
// Saving user test@test.com to database...
```

`user.save()` requires no arguments. It uses the object's own data.

### One Parameter

Functions with one parameter are straightforward:

```ts
function square(number: number): number {
  return number * number;
}

console.log(square(3));
// 9

function isValidEmail(email: string): boolean {
  return email.includes("@") && email.includes(".");
}

console.log(isValidEmail("max@test.com"));
// true

console.log(isValidEmail("invalid"));
// false
```

### Two Parameters

Two parameters can be okay when the order is obvious:

```ts
function login(email: string, password: string): void {
  console.log(`Logging in ${email}...`);
}

login("max@test.com", "testpassword");
// Logging in max@test.com...

class Point {
  x: number;
  y: number;

  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }
}

const point = new Point(10, 13);
console.log(point);
// Point { x: 10, y: 13 }
```

But avoid two-parameter functions where a boolean flag splits behavior. Split into two functions instead:

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

### Three or More Parameters: Use an Object

When too many parameters are required, group them into an object:

```ts
// Bad: hard to remember order, hard to read at call site
class User {
  name: string;
  age: number;
  email: string;

  constructor(name: string, age: number, email: string) {
    this.name = name;
    this.age = age;
    this.email = email;
  }
}

const user1 = new User("Max", 31, "max@test.com"); // which is age, which is email?
```

```ts
// Good: an object parameter makes the call site self-documenting
interface UserData {
  name: string;
  age: number;
  email: string;
}

class User {
  name: string;
  age: number;
  email: string;

  constructor(userData: UserData) {
    this.name = userData.name;
    this.age = userData.age;
    this.email = userData.email;
  }
}

const user = new User({ name: "Max", email: "max@test.com", age: 31 });
console.log(user);
// User { name: 'Max', age: 31, email: 'max@test.com' }
```

The object parameter lets you pass values in any order, and each value is labeled at the call site.

### Dynamic Number Of Parameters

Use rest parameters when a function should accept any number of values:

```ts
function sumUp(...numbers: number[]): number {
  let sum = 0;
  for (const number of numbers) {
    sum += number;
  }
  return sum;
}

console.log(sumUp(10, 19, -3, 22, 5, 100));
// 153
```

### Avoid Output Parameters

Output parameters (modifying an argument passed to a function) can be unexpected. Prefer methods on objects:

```ts
// Bad: user is modified as a side effect, not obvious from the call
function addId(user: { name: string; id?: string }): void {
  user.id = "u1";
}

const user = { name: "Max" };
addId(user);
console.log(user);
// { name: 'Max', id: 'u1' }

// Good: modification is obvious because it's on the object itself
class User {
  name: string;
  id?: string;

  constructor(name: string) {
    this.name = name;
  }

  addId(): void {
    this.id = "u" + Math.random().toString(36).substring(2, 6);
  }
}

const customer = new User("Max");
customer.addId();
console.log(customer);
// User { name: 'Max', id: 'u...' }
```

### Functions Should Be Small & Do One Thing

A function does "one thing" when all operations in the function body are on the same level of abstraction and one level below the function name.

```ts
// Bad: this function does too many things at mixed abstraction levels
function renderContent(renderInfo: {
  element: string;
  attributes: { name: string; value: string }[];
  content: string;
  root: { innerHTML: string };
}): void {
  const element = renderInfo.element;
  if (element === "script" || element === "SCRIPT") {
    throw new Error("Invalid element.");
  }

  let partialOpeningTag = "<" + element;
  const attributes = renderInfo.attributes;

  for (const attribute of attributes) {
    partialOpeningTag =
      partialOpeningTag + " " + attribute.name + '="' + attribute.value + '"';
  }

  const openingTag = partialOpeningTag + ">";
  const closingTag = "</" + element + ">";
  const template = openingTag + renderInfo.content + closingTag;
  renderInfo.root.innerHTML = template;
}

// Usage
const root = { innerHTML: "" };
renderContent({
  element: "p",
  attributes: [{ name: "class", value: "highlight" }],
  content: "Hello World",
  root,
});
console.log(root.innerHTML);
// <p class="highlight">Hello World</p>
```

```ts
// Good: each function does one thing at one level of abstraction
function renderContent(renderInfo: {
  element: string;
  attributes: { name: string; value: string }[];
  content: string;
  root: { innerHTML: string };
}): void {
  validateElement(renderInfo.element);
  const template = buildTemplate(renderInfo);
  renderInfo.root.innerHTML = template;
}

function validateElement(element: string): void {
  if (element.toLowerCase() === "script") {
    throw new Error("Invalid element.");
  }
}

function buildTemplate(renderInfo: {
  element: string;
  attributes: { name: string; value: string }[];
  content: string;
}): string {
  const attributeString = renderInfo.attributes
    .map((attr) => ` ${attr.name}="${attr.value}"`)
    .join("");
  const openingTag = `<${renderInfo.element}${attributeString}>`;
  const closingTag = `</${renderInfo.element}>`;
  return openingTag + renderInfo.content + closingTag;
}

// Usage
const root = { innerHTML: "" };
renderContent({
  element: "p",
  attributes: [{ name: "class", value: "highlight" }],
  content: "Hello World",
  root,
});
console.log(root.innerHTML);
// <p class="highlight">Hello World</p>
```

### Understanding Levels Of Abstraction

High-level operations abstract away details. Low-level operations deal with specifics. Don't mix them.

```ts
class Database {
  private uri: string;
  constructor(uri: string) {
    this.uri = uri;
  }
  connect(): void {
    console.log("Connected to:", this.uri);
  }
}

// Bad: mixed levels of abstraction
function connectToDatabaseBad(uri: string): void {
  if (uri === "") {
    console.log("Invalid URI!");
    return;
  }
  const db = new Database(uri);
  db.connect();
}

// Good: all operations at the same (high) level
function connectToDatabase(uri: string): void {
  validateUri(uri);
  const db = new Database(uri);
  db.connect();
}

function validateUri(uri: string): void {
  if (!uri) {
    throw new Error("An URI is required!");
  }
}

connectToDatabase("localhost:5432");
// Connected to: localhost:5432
```

### Rules Of Thumb For When To Split

There are two easy rules that help decide when to extract a function:

1. Extract code that works on the same functionality / is closely related
2. Extract code that requires more interpretation than the surrounding code

```ts
function validateUserData(data: {
  id: string;
  age: number;
  name: string;
}): void {
  if (!data.id || !data.name) throw new Error("Invalid data");
}

function findUserById(id: string) {
  return {
    setAge(age: number) {
      console.log("Set age:", age);
    },
    setName(name: string) {
      console.log("Set name:", name);
    },
    save() {
      console.log("User saved");
    },
  };
}

// Before: setAge and setName are related. they both update user data
function updateUserBefore(userData: {
  id: string;
  age: number;
  name: string;
}): void {
  validateUserData(userData);
  const user = findUserById(userData.id);
  user.setAge(userData.age);
  user.setName(userData.name);
  user.save();
}

// After: related operations extracted into a single function
function updateUser(userData: { id: string; age: number; name: string }): void {
  validateUserData(userData);
  applyUpdate(userData);
}

function applyUpdate(userData: {
  id: string;
  age: number;
  name: string;
}): void {
  const user = findUserById(userData.id);
  user.setAge(userData.age);
  user.setName(userData.name);
  user.save();
}

updateUser({ id: "u1", age: 31, name: "Max" });
// Set age: 31
// Set name: Max
// User saved
```

### Don't Overdo It: Avoid Useless Extractions

Splitting functions is important, but pointless extractions lead nowhere. Watch for three signals that an extraction doesn't make sense:

1. You're just renaming the operation
2. Finding the new function takes longer than reading the extracted code
3. You can't come up with a reasonable name that hasn't already been taken

```ts
// Over-extracted: throwError and buildUser just rename existing operations
function createUser(email: string, password: string): void {
  validateInput(email, password);
  saveUser(email, password);
}

function throwError(message: string): void {
  throw new Error(message); // just renaming "throw new Error"
}

function buildUser(
  email: string,
  password: string,
): { email: string; password: string } {
  return { email, password }; // just renaming object creation
}
```

```ts
// Reasonable: each function does meaningful work
function createUser(email: string, password: string): void {
  validateInput(email, password);
  saveUser(email, password);
}

function validateInput(email: string, password: string): void {
  if (!isEmail(email) || !isValidPassword(password)) {
    throw new Error("Invalid input");
  }
}

function isEmail(email: string): boolean {
  return !!email && email.includes("@");
}

function isValidPassword(password: string): boolean {
  return !!password && password.trim().length > 0;
}

function saveUser(email: string, password: string): void {
  const user = {
    email,
    password,
    id: Math.random().toString(36).substring(2, 10),
  };
  console.log("Saving user:", user);
}

createUser("max@test.com", "secret123");
// Saving user: { email: 'max@test.com', password: 'secret123', id: '...' }
```

### Stay DRY: Don't Repeat Yourself

Signs of code that is "not DRY": you find yourself copy-pasting code, or you need to apply the same change to multiple places.

```ts
// Not DRY: repeated validation logic across functions
function createUserBad(email: string, password: string): void {
  if (!email || !email.includes("@")) {
    console.log("Invalid email!");
    return;
  }
  if (!password || password.trim() === "") {
    console.log("Invalid password!");
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
function createUser(email: string, password: string): void {
  if (!inputIsValid(email, password)) {
    showErrorMessage("Invalid input!");
    return;
  }
  saveUser(email, password);
}

function createSupportChannel(email: string): void {
  if (!isEmail(email)) {
    showErrorMessage("Invalid email. Could not create channel");
    return;
  }
  console.log("Support channel created for:", email);
}

function inputIsValid(email: string, password: string): boolean {
  return isEmail(email) && isValidPassword(password);
}

function isEmail(email: string): boolean {
  return !!email && email.includes("@");
}

function isValidPassword(password: string): boolean {
  return !!password && password.trim() !== "";
}

function showErrorMessage(message: string): void {
  console.log(message);
}

function saveUser(email: string, password: string): void {
  const user = { email, password };
  console.log("User saved:", user);
}

createUser("max@test.com", "secret");
// User saved: { email: 'max@test.com', password: 'secret' }

createSupportChannel("invalid");
// Invalid email. Could not create channel

createSupportChannel("alice@company.com");
// Support channel created for: alice@company.com
```

`isEmail()` is reused by both `createUser()` (through `inputIsValid`) and `createSupportChannel()`. If the email validation rule changes, we only change it in one place.

### Avoid Unexpected Side Effects

A side effect is an operation which changes the state of the application (database writes, console output, modifying global variables). Side effects are normal, but they should never be unexpected.

```ts
function createSession(): void {
  console.log("Session created");
}

// Bad: unexpected side effect: createSession() inside validation
function validateUserInputBad(email: string, password: string): void {
  if (!email.includes("@") || password.length < 7) {
    throw new Error("Invalid input!");
  }
  createSession(); // <-- side effect hidden in a "validate" function!
}

// Good: side effects are in functions whose names imply them
function login(email: string, password: string): void {
  validateUserInput(email, password);
  createSession(); // <-- expected here, because "login" implies state change
}

function validateUserInput(email: string, password: string): void {
  if (!email.includes("@") || password.length < 7) {
    throw new Error("Invalid input!");
  }
}

login("max@test.com", "secret123");
// Session created
```

**Pure vs Impure Functions:**

```ts
// Pure: same input always gives the same output, no side effects
function generateId(userName: string): string {
  return "id_" + userName;
}

console.log(generateId("max"));
// id_max
console.log(generateId("max"));
// id_max (always the same)

// Impure: modifies external state
let lastAssignedId: string;

function generateIdWithSideEffect(userName: string): string {
  const id = "id_" + userName;
  lastAssignedId = id; // <-- side effect: changes external variable
  return id;
}

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

### Unit Testing Helps

If you can easily test a function in isolation, it's a sign that it's clean. If you can't, consider splitting it.

```ts
// Clean, testable helper functions
function isEmpty(value: string): boolean {
  return !value || value.trim() === "";
}

function hasMinValue(value: number, minValue: number): boolean {
  return value > minValue;
}

// Tests (conceptual, works with any testing framework)
console.log(isEmpty("") === true); // true
console.log(isEmpty("Test") === false); // true
console.log(hasMinValue(10, 8) === true); // true
console.log(hasMinValue(5, 8) === false); // true
```

---

## Control Structures & Errors

Control structures (`if`, `for`, `while`, `switch`) are essential for coordinating code flow, but they can also lead to bad or suboptimal code. There are three main areas of improvement: prefer positive checks, avoid deep nesting, and embrace errors.

### Prefer Positive Checks

Use positive wording in `if` checks when possible. It reduces mental overhead:

```ts
// Preferred: reads naturally, no negation needed
function isEmpty(value: string): boolean {
  return !value || value.trim() === "";
}

const blogContent = "";

if (isEmpty(blogContent)) {
  console.log("Error: No content provided!");
}
// Error: No content provided!

// Slightly worse: "!" requires extra mental parsing
function hasContent(value: string): boolean {
  return !!value && value.trim() !== "";
}

if (!hasContent(blogContent)) {
  console.log("Error: No content provided! (via hasContent)");
}
// Error: No content provided! (via hasContent)
```

But sometimes negative checks are simpler, especially when multiple states exist:

```ts
type TransactionStatus = "OPEN" | "CLOSED" | "UNKNOWN" | "PENDING";
const transaction = { status: "CLOSED" as TransactionStatus };

function isOpen(t: { status: string }): boolean {
  return t.status === "OPEN";
}
function isClosed(t: { status: string }): boolean {
  return t.status === "CLOSED";
}
function isUnknown(t: { status: string }): boolean {
  return t.status === "UNKNOWN";
}
function isPending(t: { status: string }): boolean {
  return t.status === "PENDING";
}

// Better with negation: one check instead of many
if (!isOpen(transaction)) {
  console.log("Error: Transaction is not open!");
}
// Error: Transaction is not open!

// Worse: must enumerate every non-open state
if (isClosed(transaction) || isUnknown(transaction) || isPending(transaction)) {
  console.log("Error: Transaction is not open! (via enumeration)");
}
// Error: Transaction is not open! (via enumeration)
```

### Avoid Deep Nesting

Deeply nested code is extremely hard to read and maintain:

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

### Use Guards & Fail Fast

Guards are `if` checks at the start of functions that exit early if conditions aren't met, flattening the nesting:

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

### Extract Control Structures Into Functions

Splitting nested control structures into separate functions reduces complexity:

```ts
class Database {
  connectionDetails = { host: "localhost", port: 5432 };
  fallbackConnection = true;
  fallbackConnectionDetails = { host: "fallback", port: 5433 };
  private uri: string;

  constructor(uri: string) {
    this.uri = uri;
  }

  connect(): boolean {
    console.log("Attempting connection to:", this.uri);
    return this.uri !== "bad-uri";
  }
}

// Bad: nested if/else in one function
function connectDatabaseBad(uri: string): any {
  if (!uri) {
    throw new Error("An URI is required!");
  }
  const db = new Database(uri);
  const success = db.connect();
  if (!success) {
    if (db.fallbackConnection) {
      return db.fallbackConnectionDetails;
    } else {
      throw new Error("Could not connect!");
    }
  }
  return db.connectionDetails;
}

// Good: each concern is in its own function
function connectDatabase(uri: string): any {
  validateUri(uri);
  const db = new Database(uri);
  const success = db.connect();

  if (success) {
    return db.connectionDetails;
  }
  return connectFallbackDatabase(db);
}

function validateUri(uri: string): void {
  if (!uri) {
    throw new Error("An URI is required!");
  }
}

function connectFallbackDatabase(db: any): any {
  if (db.fallbackConnection) {
    return db.fallbackConnectionDetails;
  }
  throw new Error("Could not connect!");
}

const details = connectDatabase("localhost:5432");
console.log("Connected:", details);
// Attempting connection to: localhost:5432
// Connected: { host: 'localhost', port: 5432 }
```

### Embrace Errors & Error Handling

Errors allow you to utilize mechanisms built into the language to handle problems. They remove the need for extra `if` checks and synthetic error codes.

```ts
// Bad: "synthetic" error with code checks
function validateInput(
  email: string,
  password: string,
): { code: number; message: string } | void {
  if (!email.includes("@") || password.length < 7) {
    return { code: 1, message: "Invalid input" };
  }
}

function createUser(email: string, password: string): void {
  const result = validateInput(email, password);
  if (result && (result.code === 1 || result.code === 2)) {
    console.log(result.message);
    return;
  }
  console.log("User created:", email);
}

createUser("invalid", "123");
// Invalid input
```

```ts
// Good: real errors with try/catch
function validateInput(email: string, password: string): void {
  if (!email.includes("@") || password.length < 7) {
    throw new Error("Invalid input!");
  }
}

function createUser(email: string, password: string): void {
  validateInput(email, password);
  console.log("User created:", email);
}

// Error handling is "one thing": keep it in a separate function
function handleSignupRequest(email: string, password: string): void {
  try {
    createUser(email, password);
  } catch (error: any) {
    console.log("Signup failed:", error.message);
  }
}

handleSignupRequest("max@test.com", "secret123");
// User created: max@test.com

handleSignupRequest("invalid", "123");
// Signup failed: Invalid input!
```

`throw` generates an error that bubbles up the call stack until it's handled by `try-catch`. This removes the need for returning error objects and checking codes.

### Factory Functions & Polymorphism

When you have duplicated `if` checks with slightly different logic inside, use factory functions to produce polymorphic objects:

```ts
// Bad: duplicated if checks for payment method
interface Transaction {
  type: string;
  status: string;
  method: string;
  amount: string;
}

function processTransaction(transaction: Transaction): void {
  if (transaction.type === "PAYMENT") {
    if (transaction.method === "CREDIT_CARD") {
      console.log("Processing credit card payment: $" + transaction.amount);
    } else if (transaction.method === "PAYPAL") {
      console.log("Processing PayPal payment: $" + transaction.amount);
    }
  } else if (transaction.type === "REFUND") {
    if (transaction.method === "CREDIT_CARD") {
      console.log("Processing credit card refund: $" + transaction.amount);
    } else if (transaction.method === "PAYPAL") {
      console.log("Processing PayPal refund: $" + transaction.amount);
    }
  }
}
```

```ts
// Good: factory function returns a polymorphic processor object
interface Transaction {
  type: string;
  status: string;
  method: string;
  amount: string;
}

interface TransactionProcessors {
  processPayment: (t: Transaction) => void;
  processRefund: (t: Transaction) => void;
}

function getTransactionProcessors(
  transaction: Transaction,
): TransactionProcessors {
  const processors: TransactionProcessors = {
    processPayment: () => {},
    processRefund: () => {},
  };

  if (transaction.method === "CREDIT_CARD") {
    processors.processPayment = (t) =>
      console.log("Processing credit card payment: $" + t.amount);
    processors.processRefund = (t) =>
      console.log("Processing credit card refund: $" + t.amount);
  } else if (transaction.method === "PAYPAL") {
    processors.processPayment = (t) =>
      console.log("Processing PayPal payment: $" + t.amount);
    processors.processRefund = (t) =>
      console.log("Processing PayPal refund: $" + t.amount);
  }

  return processors;
}

function processTransaction(transaction: Transaction): void {
  const processors = getTransactionProcessors(transaction);

  if (transaction.type === "PAYMENT") {
    processors.processPayment(transaction);
  } else {
    processors.processRefund(transaction);
  }
}

processTransaction({
  type: "PAYMENT",
  status: "OPEN",
  method: "CREDIT_CARD",
  amount: "23.99",
});
// Processing credit card payment: $23.99

processTransaction({
  type: "REFUND",
  status: "OPEN",
  method: "PAYPAL",
  amount: "10.99",
});
// Processing PayPal refund: $10.99
```

The factory function `getTransactionProcessors()` runs the method check once instead of once per type. The returned object is polymorphic. We always call `processPayment()` or `processRefund()`, but the actual logic depends on the method.

### Avoid Magic Numbers & Strings

Hard-coded values like `"PAYPAL"` or `"CREDIT_CARD"` scattered throughout code are error-prone (typos!) and hard to change. Use constants or enums:

```ts
const transaction = { method: "CREDIT_CARD", type: "PAYMENT" };

// Bad: magic strings everywhere
if (transaction.method === "CREDIT_CARD") {
  console.log("Credit card detected");
}
if (transaction.type === "PAYMENT") {
  console.log("Payment detected");
}
// Credit card detected
// Payment detected

// Good: constants defined once, reused everywhere
const PAYMENT_METHOD = {
  CREDIT_CARD: "CREDIT_CARD",
  PAYPAL: "PAYPAL",
  PLAN: "PLAN",
} as const;

const TRANSACTION_TYPE = {
  PAYMENT: "PAYMENT",
  REFUND: "REFUND",
} as const;

if (transaction.method === PAYMENT_METHOD.CREDIT_CARD) {
  console.log("Credit card (via constant)");
}
if (transaction.type === TRANSACTION_TYPE.PAYMENT) {
  console.log("Payment (via constant)");
}
// Credit card (via constant)
// Payment (via constant)
```

With TypeScript, you can also use `enum`:

```ts
const transaction = { method: "CREDIT_CARD", type: "PAYMENT" };

enum PaymentMethod {
  CreditCard = "CREDIT_CARD",
  PayPal = "PAYPAL",
  Plan = "PLAN",
}

enum TransactionType {
  Payment = "PAYMENT",
  Refund = "REFUND",
}

if (transaction.method === PaymentMethod.CreditCard) {
  console.log("Credit card (via enum)");
}
if (transaction.type === TransactionType.Payment) {
  console.log("Payment (via enum)");
}
// Credit card (via enum)
// Payment (via enum)
```

---

## Classes, Objects & Data Containers

Classes are blueprints for objects. When working with classes and objects, we can differentiate between "real objects" (with a public API of methods) and "data containers" (with public properties and almost no methods).

### Objects vs Data Containers

```ts
// Data Container: holds data, exposes properties publicly
class UserCredentials {
  email: string;
  password: string;

  constructor(email: string, password: string) {
    this.email = email;
    this.password = password;
  }
}

const credentials = new UserCredentials("max@test.com", "secret");
console.log(credentials.email);
// max@test.com
```

```ts
// Real Object: hides data, exposes a public API
class User {
  private name: string;
  private age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet(): void {
    console.log(`Hi! I'm ${this.name} and I'm ${this.age} years old.`);
  }
}

const user = new User("Max", 31);
user.greet();
// Hi! I'm Max and I'm 31 years old.
```

Both are valid types. The key is to use the right kind for the right job and avoid mixing the types.

### Classes & Polymorphism

Use inheritance and polymorphism to eliminate duplicated `if` statements. This is easier to maintain because adding a new type doesn't require modifying existing `if/else` logic.

#### Example

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

#### Example with Factory Pattern

```ts
// Bad: duplicate checks for deliveryType in every method
class Delivery {
  private purchase: any;

  constructor(purchase: any) {
    this.purchase = purchase;
  }

  deliverProduct(): void {
    if (this.purchase.deliveryType === "express") {
      console.log("Express delivery for:", this.purchase.product);
    } else if (this.purchase.deliveryType === "insured") {
      console.log("Insured delivery for:", this.purchase.product);
    } else {
      console.log("Standard delivery for:", this.purchase.product);
    }
  }

  trackProduct(): void {
    if (this.purchase.deliveryType === "express") {
      console.log("Tracking express delivery...");
    } else if (this.purchase.deliveryType === "insured") {
      console.log("Tracking insured delivery...");
    } else {
      console.log("Tracking standard delivery...");
    }
  }
}

const badDelivery = new Delivery({ deliveryType: "express", product: "Phone" });
badDelivery.deliverProduct();
// Express delivery for: Phone
badDelivery.trackProduct();
// Tracking express delivery...
```

```ts
// Good: polymorphism eliminates repeated checks
interface DeliveryHandler {
  deliverProduct(): void;
  trackProduct(): void;
}

class DeliveryBase {
  protected product: string;

  constructor(product: string) {
    this.product = product;
  }
}

class ExpressDelivery extends DeliveryBase implements DeliveryHandler {
  deliverProduct(): void {
    console.log("Express delivery for:", this.product);
  }

  trackProduct(): void {
    console.log("Tracking express delivery...");
  }
}

class InsuredDelivery extends DeliveryBase implements DeliveryHandler {
  deliverProduct(): void {
    console.log("Insured delivery for:", this.product);
  }

  trackProduct(): void {
    console.log("Tracking insured delivery...");
  }
}

class StandardDelivery extends DeliveryBase implements DeliveryHandler {
  deliverProduct(): void {
    console.log("Standard delivery for:", this.product);
  }

  trackProduct(): void {
    console.log("Tracking standard delivery...");
  }
}

// Factory function: the check happens only once
function createDelivery(purchase: {
  deliveryType: string;
  product: string;
}): DeliveryHandler {
  if (purchase.deliveryType === "express") {
    return new ExpressDelivery(purchase.product);
  } else if (purchase.deliveryType === "insured") {
    return new InsuredDelivery(purchase.product);
  }
  return new StandardDelivery(purchase.product);
}

const delivery = createDelivery({ deliveryType: "express", product: "Laptop" });
delivery.deliverProduct();
// Express delivery for: Laptop
delivery.trackProduct();
// Tracking express delivery...
```

### Classes Should Be Small

A class's size is defined by its number of responsibilities, not its line count. Clean classes should only have one responsibility.

```ts
// Bad: one class handles products, customers, orders, and inventory
class OnlineShop {
  addProduct(title: string, price: number): void {
    console.log("Added:", title);
  }
  updateProduct(productId: string, title: string, price: number): void {}
  removeProduct(productId: string): void {}
  getAvailableItems(productId: string): void {}
  restockProduct(productId: string): void {}
  createCustomer(email: string, password: string): void {}
  loginCustomer(email: string, password: string): void {}
  makePurchase(customerId: string, productId: string): void {}
  addOrder(customerId: string, productId: string, quantity: number): void {}
  refund(orderId: string): void {}
  updateCustomerProfile(customerId: string, name: string): void {}
}

const shop = new OnlineShop();
shop.addProduct("Book", 10.99);
// Added: Book
```

```ts
// Good: each class has a single responsibility
class Product {
  constructor(
    public title: string,
    public price: number,
  ) {}

  update(title: string, price: number): void {
    this.title = title;
    this.price = price;
  }
}

class Customer {
  private orders: Order[] = [];

  constructor(
    public email: string,
    private password: string,
  ) {}

  login(email: string, password: string): boolean {
    return this.email === email && this.password === password;
  }

  updateProfile(name: string): void {
    console.log("Profile updated:", name);
  }
}

class Order {
  constructor(
    public customerId: string,
    public productId: string,
  ) {}

  refund(): void {
    console.log("Order refunded:", this.productId);
  }
}

class Inventory {
  private products: Product[] = [];

  addProduct(product: Product): void {
    this.products.push(product);
  }

  getAvailableItems(): Product[] {
    return this.products;
  }

  restockProduct(productId: string): void {
    console.log("Restocking:", productId);
  }
}

const inventory = new Inventory();
inventory.addProduct(new Product("Clean Code Book", 29.99));
console.log("Inventory has", inventory.getAvailableItems().length, "items.");
// Inventory has 1 items.
```

### Cohesion

Cohesion describes how much a class's methods use the class's properties. High cohesion is good because it means the class is focused. Low cohesion signals that the class might be better split or turned into a data container.

```ts
// Low Cohesion: some methods do not use class properties at all
class UserDashboard {
  private username: string;

  constructor(username: string) {
    this.username = username;
  }

  // Uses class property
  showGreeting(): void {
    console.log(`Welcome, ${this.username}!`);
  }

  // Does not use any class properties - low cohesion!
  validateEmail(email: string): boolean {
    return email.includes("@");
  }

  // Does not use any class properties - low cohesion!
  formatDate(date: Date): string {
    return date.toISOString().split("T")[0];
  }
}

const dashboard = new UserDashboard("Max");
dashboard.showGreeting();
// Welcome, Max!

console.log(dashboard.validateEmail("max@test.com"));
// true

console.log(dashboard.formatDate(new Date("2026-06-19T10:00:00Z")));
// 2026-06-19
```

```ts
// High Cohesion: every method uses the class properties
class ShoppingCart {
  private items: { name: string; price: number }[] = [];

  addItem(name: string, price: number): void {
    this.items.push({ name, price });
  }

  removeItem(name: string): void {
    this.items = this.items.filter((item) => item.name !== name);
  }

  getTotal(): number {
    return this.items.reduce((sum, item) => sum + item.price, 0);
  }

  printReceipt(): void {
    this.items.forEach((item) => console.log(`${item.name}: $${item.price}`));
    console.log(`Total: $${this.getTotal()}`);
  }
}

const cart = new ShoppingCart();
cart.addItem("Book", 12.99);
cart.addItem("Pen", 1.49);
cart.printReceipt();
// Book: $12.99
// Pen: $1.49
// Total: $14.48
```

Every method in `ShoppingCart` interacts with the `items` property. That's high cohesion.

### The Law Of Demeter

Don't depend on the internals of "strangers." Code in a method should only access:

- The object it belongs to
- Objects stored in properties of that object
- Objects received as method parameters
- Objects created in the method

#### Example 1

A classic violation is "chaining" method calls or property accesses.

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

#### Example 2

```ts
// Bad: chaining through multiple objects violates the Law of Demeter
class Customer {
  lastPurchase = { date: "2026-06-18", product: "Laptop" };
}

class Warehouse {
  deliverPurchasesByDate(customer: Customer, date: string): void {
    console.log("Delivering purchases from:", date);
  }

  deliverPurchase(purchase: { date: string; product: string }): void {
    console.log("Delivering:", purchase.product);
  }
}

class DeliveryJob {
  private customer: Customer;
  private warehouse: Warehouse;

  constructor(customer: Customer, warehouse: Warehouse) {
    this.customer = customer;
    this.warehouse = warehouse;
  }

  // Bad: accessing customer.lastPurchase.date (chaining)
  deliverLastPurchaseBad(): void {
    const date = this.customer.lastPurchase.date; // Law of Demeter violation
    this.warehouse.deliverPurchasesByDate(this.customer, date);
  }

  // Good: "tell, don't ask"
  deliverLastPurchaseGood(): void {
    this.warehouse.deliverPurchase(this.customer.lastPurchase);
  }
}

const customer = new Customer();
const warehouse = new Warehouse();
const job = new DeliveryJob(customer, warehouse);
job.deliverLastPurchaseGood();
// Delivering: Laptop
```

Instead of reaching into `customer.lastPurchase.date`, we pass the entire `lastPurchase` object to the warehouse and let it handle the details.

---

## The SOLID Principles

SOLID is a set of five object-oriented design principles that lead to smaller, more focused, and more maintainable classes.

| Principle | Name                        | One-Liner                                                   |
| --------- | --------------------------- | ----------------------------------------------------------- |
| **S**     | Single-Responsibility (SRP) | A class should only have one reason to change               |
| **O**     | Open-Closed (OCP)           | Open for extension, closed for modification                 |
| **L**     | Liskov Substitution (LSP)   | Subtypes must be substitutable for their base types         |
| **I**     | Interface Segregation (ISP) | Many specific interfaces beat one general-purpose interface |
| **D**     | Dependency Inversion (DIP)  | Depend on abstractions, not concretions                     |

### S: Single-Responsibility Principle

A class should only change for one reason. If it needs to change for multiple reasons, it has too many responsibilities.

```ts
// Bad: two reasons to change: report generation AND PDF creation
class ReportDocument {
  generateReport(data: any): string {
    return `Report: ${JSON.stringify(data)}`;
  }

  createPDF(report: string): void {
    console.log("Creating PDF from:", report);
  }
}

// Good: each class has one responsibility
class Report {
  generate(data: any): string {
    return `Report: ${JSON.stringify(data)}`;
  }
}

class PDFPrinter {
  createPDF(content: string): void {
    console.log("Creating PDF from:", content);
  }
}

const report = new Report();
const content = report.generate({ sales: 1000, returns: 50 });
const printer = new PDFPrinter();
printer.createPDF(content);
// Creating PDF from: Report: {"sales":1000,"returns":50}
```

### O: Open-Closed Principle

A class should be open for extension (new subclasses) but closed for modification (existing code doesn't change for new features).

```ts
// Bad: adding a new document type requires modifying the class
class PrinterBad {
  printPDF(data: string): void {
    console.log("Printing PDF:", data);
  }
  printWebDocument(data: string): void {
    console.log("Printing Web:", data);
  }
  printPage(data: string): void {
    console.log("Printing Page:", data);
  }
  // Adding Word support? Must add a new method here...
}

const badPrinter = new PrinterBad();
badPrinter.printPDF("Hello");
// Printing PDF: Hello
```

```ts
// Good: extend with new classes, base stays closed
interface Printer {
  print(data: string): void;
}

class PDFPrinter implements Printer {
  print(data: string): void {
    console.log("Printing PDF:", data);
  }
}

class WebPrinter implements Printer {
  print(data: string): void {
    console.log("Printing Web:", data);
  }
}

// Adding Word support? Just add a new class, no existing code changes!
class WordPrinter implements Printer {
  print(data: string): void {
    console.log("Printing Word:", data);
  }
}

const printers: Printer[] = [
  new PDFPrinter(),
  new WebPrinter(),
  new WordPrinter(),
];
printers.forEach((p) => p.print("Hello World"));
// Printing PDF: Hello World
// Printing Web: Hello World
// Printing Word: Hello World
```

### L: Liskov Substitution Principle

Objects should be replaceable with instances of their subtypes without altering the behavior of the program.

```ts
// Bad: Penguin extends Bird but can't fly
class BirdBad {
  fly(): void {
    console.log("Flying...");
  }
}

class PenguinBad extends BirdBad {
  fly(): void {
    throw new Error("Penguins can't fly!"); // <-- breaks substitution
  }
}

const badPenguin = new PenguinBad();
try {
  badPenguin.fly();
} catch (e: any) {
  console.log("Error:", e.message);
}
// Error: Penguins can't fly!
```

```ts
// Good: separate hierarchy for flying vs non-flying birds
class Bird {
  eat(): void {
    console.log("Eating...");
  }
}

class FlyingBird extends Bird {
  fly(): void {
    console.log("Flying...");
  }
}

class Eagle extends FlyingBird {
  dive(): void {
    console.log("Diving...");
  }
}

class Penguin extends Bird {
  swim(): void {
    console.log("Swimming...");
  }
}

const eagle = new Eagle();
eagle.fly();
// Flying...
eagle.dive();
// Diving...

const penguin = new Penguin();
penguin.eat();
// Eating...
penguin.swim();
// Swimming...
```

Now, `Eagle` can substitute `FlyingBird`, and `Penguin` can substitute `Bird`, both without breaking behavior.

### I: Interface Segregation Principle

Many client-specific interfaces are better than one general-purpose interface. Don't force classes to implement methods they don't need.

```ts
// Bad: InMemoryDatabase is forced to implement connect()
interface DatabaseBad {
  connect(uri: string): void;
  storeData(data: any): void;
}

class InMemoryDatabaseBad implements DatabaseBad {
  connect(uri: string): void {
    // Has nothing to connect to, but forced by the interface!
    console.log("In-memory connects to nothing.");
  }

  storeData(data: any): void {
    console.log("Storing data in memory:", data);
  }
}

const memDbBad = new InMemoryDatabaseBad();
memDbBad.connect("none");
// In-memory connects to nothing.
memDbBad.storeData({ ok: true });
// Storing data in memory: { ok: true }
```

```ts
// Good: split into focused interfaces
interface Database {
  storeData(data: any): void;
}

interface RemoteDatabase {
  connect(uri: string): void;
}

class SQLDatabase implements Database, RemoteDatabase {
  connect(uri: string): void {
    console.log("Connecting to SQL database at:", uri);
  }

  storeData(data: any): void {
    console.log("Storing data in SQL:", data);
  }
}

class InMemoryDatabase implements Database {
  storeData(data: any): void {
    console.log("Storing data in memory:", data);
  }
}

const sqlDb = new SQLDatabase();
sqlDb.connect("localhost:5432");
sqlDb.storeData({ name: "Max" });
// Connecting to SQL database at: localhost:5432
// Storing data in SQL: { name: 'Max' }

const memDb = new InMemoryDatabase();
memDb.storeData({ name: "Alice" });
// Storing data in memory: { name: 'Alice' }
```

### D: Dependency Inversion Principle

Depend on abstractions, not concretions. Don't check for specific types inside consuming code. Let the caller handle specifics.

```ts
// Bad: NotificationService depends directly on concrete EmailService and SMSService classes
class EmailServiceBad {
  sendEmail(message: string): void {
    console.log("Sending email: " + message);
  }
}

class SMSServiceBad {
  sendSMS(message: string): void {
    console.log("Sending SMS: " + message);
  }
}

class NotificationServiceBad {
  private emailService: EmailServiceBad;
  private smsService: SMSServiceBad;

  constructor() {
    this.emailService = new EmailServiceBad();
    this.smsService = new SMSServiceBad();
  }

  notifyByEmail(msg: string): void {
    this.emailService.sendEmail(msg);
  }

  notifyBySMS(msg: string): void {
    this.smsService.sendSMS(msg);
  }
}

const badNotificationService = new NotificationServiceBad();
badNotificationService.notifyByEmail("Your order has been shipped");
badNotificationService.notifyBySMS("OTP 1234");
```

```ts
// Good: NotificationService depends on the NotificationChannel abstraction
interface NotificationChannel {
  send(msg: string): void;
}

class EmailService implements NotificationChannel {
  send(msg: string): void {
    console.log("Sending Email " + msg);
  }
}

class SMSService implements NotificationChannel {
  send(msg: string): void {
    console.log("Sending SMS " + msg);
  }
}

class NotificationService {
  private notificationChannel: NotificationChannel;

  constructor(channel: NotificationChannel) {
    this.notificationChannel = channel;
  }

  notify(msg: string): void {
    this.notificationChannel.send(msg);
  }
}

// The caller decides which implementation to inject
const emailNotification = new NotificationService(new EmailService());
emailNotification.notify("Your order has been shipped");
// Sending Email Your order has been shipped

const smsNotification = new NotificationService(new SMSService());
smsNotification.notify("OTP 1234");
// Sending SMS OTP 1234
```

The dependency is inverted: instead of `NotificationService` knowing how to create and manage specific channels, whoever creates the `NotificationService` is responsible for providing a fully configured channel.

---

## Clean Code Checklist

### Naming

- Use descriptive and meaningful names
- Variables & Properties: Nouns or short phrases with adjectives
- Functions & Methods: Verbs or short phrases with adjectives
- Classes: Nouns
- Be as specific as necessary and possible. Avoid redundant information, slang, abbreviations
- Avoid disinformation or misleading names
- Follow name casing
- Use yes/no "questions" for booleans (e.g. `isValid`)
- Be consistent with your names (e.g. stick to `get...` instead of `fetch...`)

### Comments & Formatting

- Most comments are bad, so avoid them!
- Acceptable comments are legal info, warnings, regex explanations, and todos
- Keep related concepts close together (vertical density)
- Add spacing between unrelated concepts (vertical distance)
- Write code top to bottom with called functions below calling functions
- Avoid long lines and break them into multiple lines (horizontal formatting)
- Use indentation to express scope

### Functions

- Limit the number of parameters because less is better. Use objects to group parameters
- Functions should be small and do one thing
- Keep operations one level of abstraction below the function name
- Avoid mixing levels of abstraction
- Avoid redundant splitting. Don't extract just for extraction's sake
- Stay DRY (Don't Repeat Yourself)
- Avoid unexpected side effects
- If you can easily test a function in isolation, it's a sign that it's clean

### Control Structures & Errors

- Prefer positive checks
- Avoid deep nesting by using guards and early returns
- Consider using polymorphism and factory functions
- Extract control structures into separate functions
- Use real errors (`throw`/`try-catch`) instead of synthetic error objects with `if` checks
- Avoid magic numbers and strings. Use constants or enums instead

### Objects & Classes

- Differentiate between real objects (public API) and data containers (public properties)
- Build small classes focused on a single responsibility
- Aim for high cohesion so that methods use class properties
- Follow the Law of Demeter and avoid chaining through strangers
- Follow the SOLID principles, especially SRP and OCP
