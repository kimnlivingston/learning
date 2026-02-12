**Learn JavaScript: Write Modern Code with JavaScript ESNext**
===

https://www.linkedin.com/learning/learn-javascript-write-modern-code-with-javascript-esnext

# What is JS?

## How It's Used

- Front-End: React, Angular, Vue, Svelte, jQuery
- Back-End: made possible by Node.js: Allows JS to be run outside of the browser (i.e. a server)
  - NPM: Node Package Manager: Allows developers to public and share code
  - MongoDB: A non-relational database that store JavaScript objects
- Mobile: React Native, Native Script (allows for a single codebase for mulitple platforms)
- Desktop: Electron (takes front-end apps written in JS and builds them so they can be run as desktop apps)

## Language Features

- Interpreted language
- Dynamically and Weakly typed
- "Mostly" Object-Oriented (but slightly different than normal): it can both use objects to modal a problem and can do so in a reusable way
  - Does not support private variables
  - Functional Programming will get the most out of JavaScript
- Single Threaded: only executing a single operation at a time

## Pros

- popular (active developer ecosystem)
- relatively easy to learn and use
- can be used to a very wide range of applications
- client-side execution

## Cons

- have to worry about browser support
- have to be more security-conscious
  - the client has a copy of our source code
  - third-party code can make our site more vulnerable
- certain parts of the language can behave strangely
- not the best language for "OOP purists"
- is evolving *very* quickly

## Dialects and Browser Compatibility

### Versioning

Old ECMAScript versions was named by numbers (ES1, ES2...). From 2016, versions are named by year (ES2016, ES2017...). Most browsers support up to ES6, which is the same as ES2016.

- ECMAScript Specification
  - defines different sets of language features that a runtime must support
  - provides a "guarantee" of what features a given JS runtime will support
- Babel: converts ES6+ code into code with better browser support

### Dialects

There are several "dialects" that transpile (automatically convert) to JS before they are run

- TypeScript: add static typing to JS
    ``` js
    // JavaScript
    let x = 5;
    ```
    ``` js
    // TypeScript
    let x: number = 5;
    ```
- CoffeeScript: offers "syntactically improved" version of JS
    ``` js
    // JavaScript
    let person = {
      name: "John",
      age: 40,
      hairColor: "brown"
    };
    ```
    ``` js
    // CoffeeScript
    person =
      name: "John"
      age: 40
      hairColor: "brown"
    ```
- elm: pure functions and data visualization
- logicJS: logic programming
- dart: improved OOP features

# Data Types

- `var`: function-scoped (referenced only inside the scope of the innermost **function** that they were defined in)
- `let`: block scoped (referenced only inside the scope of the innermost **block** that they were defined in)
- `const`: constant
- no variable type is a GLOBAL variable

``` js
if (true) {
  var x = "function scoped"
  let y = "block scoped"

  console.log(x)              // outputs "function scoped"
  console.log(y)              // outputs "block scoped"
}

console.log(x)                // outputs "function scoped"
console.log(y)                // outputs error
```

## Numbers

- scientic notation: `let sixtyMillion = 6e7;`
- Not a Number (`NaN`): `let answer - 10 * "oops";`
- `Infinity`: `let answer = 9 / 0;` or if the number is too big
- `Math` object: has many methods for calculation

## Strings

- `\"`
- `\'`
- `\n`
- `\\`

## Booleans

The 7 "Falsy" Values:

1. `""`
2. `0`
3. `NaN`
4. `0n`
5. `null`
6. `undefined`
7. `false`

## Objects

Other languages refer to objects as hashes or key value maps.
``` js
let name = "Dana";
let age = 22;

let person = {
  name,
  age,
};
```

The following are technically objects accordings to jS:
- `null`
- arrays

## Functions

Functions definded with `function` are hoisted (can be used anywhere in the code).

## Undefined

The difference between `undefined` and `null`:
- `undefined` means that we either
  - haven't declared a variable or
  - we have defined a variable and have not assigned it a variable
  - `typeof undefined;` => `undefined`
- `null` means that we
  - have defined a variable and assigned `null` to it
  - `typeof null` => `object`

## BigInts

- no decimals
- add `n` at the end of the number
- `10n + 2` => `Error`
- `10n + BigInt(2)` => `12n`

## Symbols

Exist to ensure uniqueness.

``` js
let mySymbol = Symbol("myFirstSymbol")
```

- `typeof mySymbol;` => `symbol`
- no two symbols are ever the same, even if they have the same description
  - the only way is to compare the same symbol or create a reference to that symbol
- Used to avoid "value clashes"
  ``` js
  let shirt = {
    // ...
    size: "Medium",
  }

  let sizeSymbol = Symbol("size");
  shirt[sizeSymbol] = 11;
  ```

# Basic Control Flow

## Equality

- `==` : does not check type
  - `0 == "0"` => `true`
  - `0 == 0n` => `true`
  - `0 == ""` => `true`
  - `0 == []` => `true`
  - `0 == null` => `false`
  - `0 == undefined` => `false`
  - `"true" == true` => `false`
  - `"false" == false` => `false`
- `===` : checks type
  - `5 == Number("5")` => `true`

## For Loop

Used when we know exactly how many iterations are needed.

``` js
// loop though an Object
for (item of arr) {
  console.log(item.name);
}
// or
arr.forEach(
  console.log(item.name)
)
```

``` js
// loop though an Object
for (key in obj) {
  console.log(`${key}: ${onj[key]}`)
}
```

## While and Do-While Loops

Used when we **DO NOT** know exactly how many iterations are needed.

- while checks before execute the loop
- do/while checks after executing the loop

## Handle and Throw Errors

Use a try-catch block and throw and error.

``` js
try {
  // code that might fail
  throw "Error!"
} catch (err) {
  console.error(err)      // "Error!"
  // error handling logic
} finally {
  // optional
  // runs everytime
  // clean-up logic
}
```

## Switch-Case Statement

``` js
switch (userAnswer) {
  case "a":
    // ...
    break;
  case "b":
    // ...
    break;
  default:
    // ...
}
```

## Ternary Operator

An "abbreviated version" of the if statement.

``` js
let greeting = isBeforeNoon
  ? "Good morning"
  : "Good afternoon";
```

# Work with Arrays and Objects

## Built-In Object Functions

``` js
let myObj = {
  a: 1,
  b: 2,
  c: 3,
}
```

### Object.keys()

``` js
Object.keys(myObj); // ["a", "b", "c"]

// Example use
for (let person of people) {
  let keys = Object.keys(persion);
  for (let key of keys) {
    console.log(key + ": " + persion[key]);
  }
}
// as opposed to
for (let person of people) {
  for (let key in person) {
    console.log(key + ": " + persion[key]);
  }
}
// to allow for keys to be used later on the in function
// example use
Object.keys(obj).filter(...);
Object.keys(obj).sort();
```

### Object.values()

``` js
Object.values(myObj); // [1, 2, 3]
```

### Object.entries()

``` js
Object.entries(myObj); // [["a", 1], ["b", 2], ["c", 3]]

// Example use (instead of using the square bracket notation)
let entries = Object.entries(myObj);

for (let entryPair of entries) {
  console.log(
    entryPair[0] + ": " + entryPair[1]
  );
}
```

### Object.assign()

``` js
ley obj1 = { a: 1, b: 2 }
ley obj2 = { c: 3, d: 4 }

Object.assign(obj1, obj2);

obj1 // { a: 1, b: 2, c: 3, d: 4 }

//example
let persion = {
  name: "Betty",
  age: 48,
}

let medicalData = {
  name: "Betsy",
  medication: null,
}

Object.assign(person, medicalData); // { name: "Betsy", age: 48, medication: null }

// use to create copy
let myObject = {
  a: 1,
  b: 2,
}
let myOtherObject = Object.assign({}, myObject);
myOtherObject.a = 100;
myObject.a // still is 1
```

## Built-In Array Functions

``` js
numbers = [1, 2, 3, 4, 5];
```

### Push

``` js
numbers.push(6);            // [1, 2, 3, 4, 5, 6]
```

### Pop

``` js
let last = numbers.pop();   // 6
numbers;                    // [1, 2, 3, 4, 5]
```

### Splice

``` js
//  array.splice (
//    startingIndex,
//    removeHowMany,
//    ... elementsToAdd,
//  );
numbers.splice(2, 1);               // [3]
numbers                             // [1, 2, 4]
numbers.splice(2, 0, 100);          // []
numbers                             // [1, 2, 100, 4]
numbers.splice(0, 2, "one", "two"); // [1, 2]
numbers                             // ["one", "two", 100, 4]
```

# Learn JavaScript ES6+ Syntax

- **Arrow function**: best outside of objects and classes
- **Function keyword**: best inside objects and classes

## Default Arguments

only works for undefined, NOT `null`, `0`, `false`

```js
let myFunc = (x = "default!", y = 100) => {
  // ...
}
```

## Spread Operator

- Combining objects and arrays (Instead of using `Object.assign()`)
  ``` js
  let combined = {
    ...obj1,
    ...obj2,
  };
  ```
- Getting function arguments as an array
  ``` js
  let myFunction = (...args) => {...}
  ```
- Passing array elements as arguments
  ``` js
  someOtherFunction(...myArray);
  ```

## Object Destructuring

```js
let {
  name: personName = "Unknown", // renames the key and assigns new value
  age,
  bodyMeasurements: { height }, // pulls out nested property and its variables
  eyeColor = "unknown", // adds a new key and assigns new value
} = person;
```

# Work with Asynchronous Code

instead of "Callback Hell

``` js
readfile('someFile.text', contents => {
  // ...
  postRequest('www.someapi.com', contents, response => {
    //...
    updateDataOnServer(data => {
      //...
      getUpdatedData(data => {
        // ...
      })
    })
  })
})
```

do

``` js
readFile('someFile.text')
.then(contents => {
  // send data to server
})
.then(response => {
  // update data on server
})
.then(data => {
  // get updated server data
})
.then(data => {
  // use the data for whatever you need to do
});
```

## Promises

``` js
let myPromise = newPromise((resolve, reject) => {
  setTimeout(() => {
    resolve("Success!");
    // use reject for error
  }, 1000);
});

myPromise.then(message => {
  console.log(message);
})
.catch(error => {
  console.log(error);
})
.finally(() => {
  // executes no matter what
});
```

## Async / Await

- Added to use instead of callbacks and promises.
- `await` must be used inside a function
  - any function that contains `await` must be labelled with `async`

Instead of:

``` js
fs.readFile('someFile.txt', contents => {
  // ...
});

// or

readFile('someFile.txt')
.then(contents => {
  // ...
});
```

We can do:

``` js
let contents = await readFile('someFile.txt');
```

#### Example

``` js
try {
  let reponse = await fetch('www.someapi.com/data');
  let data = await reponse.json();
  console.log(data);
} catch (error) {
  console.log("An error occurred!");
} finally {
  // do something
}
```

``` js
let someAsyncFunction = async () => {
  // fetch data
  return response;
}

async function execute() {
  try {
    let reponse = await someAsyncFunction();
    let data = await reponse.json();
    console.log(data);
  } catch (error) {
    console.log("An error occurred!");
  } finally {
    console.log("I'm done!");
  }
}

execute()

```
