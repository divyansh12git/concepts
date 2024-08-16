## 1. Lexical Environment

A lexical environment in JavaScript is essentially a snapshot of the variables and functions accessible at a particular point in your code. It's created when a function is called and destroyed when the function returns. Think of it as a container that holds information about the variables and functions available to the code within that container.

When JavaScript executes code, it creates a lexical environment for each scope. This includes global scope, function scopes, and block scopes (introduced in ES6 with let and const).

#### components:
- Environment Record: This is an object that stores the variables and functions declared within the current scope. It keeps track of their values and references.
- Outer Lexical Environment Reference: This is a link to the lexical environment of the enclosing scope. This allows for accessing variables from outer scopes, which is crucial for understanding closures.

#### Scope Chain:

The lexical environment forms a scope chain. This is a linked list of lexical environments, starting from the current scope and moving outwards to the global scope. When a variable is accessed, JavaScript searches for it in the current lexical environment. If not found, it moves up the scope chain to the outer environment until it finds the variable or reaches the global scope.

### Importance:
- Closures
- Scope Resolution
- Optimization
  
### Notes->
- Lexical environments are created at function creation time, not function execution time.
- Block-scoped variables (using let and const) create new lexical environments within blocks.
- Global variables are stored in the global object's lexical environment.



## 2. Hoisting 

Hoisting is a JavaScript mechanism where variable and function declarations are moved to the top of their scope before code execution. 
- applies to declarations, not assignments.

### Variable Hoisting:
- __var__ declared variables are hoisted to the top of their scope with an initial value of undefined
- **let** and **const** declared variables are also hoisted, but they are in a temporal dead zone until their declaration is reached.
  This means you cannot access them before their declaration, resulting in a ReferenceError
### Function Hoisting
- Function declarations are hoisted entirely, including their function body. This means you can call a function before its declaration.

### The Temporal Dead Zone (TDZ)
The TDZ is the period between the start of a block and the declaration of a let or const variable. Within this zone, trying to access the variable results in a ReferenceError.
```
if (true) {
  console.log(z); // ReferenceError
  let z = 15;
}
```
### Note:
- Hoisting doesn't physically move code. It's a conceptual understanding of how JavaScript handles declarations.
- Hoisting doesn't apply to variable assignments or function expressions. `const a=()=>{}`

## 3. Functions: 

### Immediately Invoked Function Expressions (IIFE)
An IIFE is a function expression that is called immediately after it's defined.
```
(function() {
  console.log("This is an IIFE");
})();
```

### Named Function Expression
```
let greet = function myGreet() {
  console.log("Hello from myGreet");
};

greet(); // Output: Hello from myGreet
```

### Callback
A callback function is essentially a function passed as an argument to another function. The purpose is to execute this callback function after the main function has finished its task or when a specific event occurs.

- Function as an Argument: A callback is simply a function passed as a parameter to another function.
- Asynchronous Operations: Callbacks are primarily used for handling asynchronous operations, where the result of an operation is not immediately available.
- Event Handling: Callbacks are also used extensively in event-driven programming, where code is executed in response to events like button clicks, mouse movements, etc.

```
function sayGoodbye() {
  console.log("Goodbye!");
}

greet("Alice", sayGoodbye);
```
```
function fetchData(url, callback) {
  // Simulate an asynchronous operation
  setTimeout(() => {
    const data = { name: "John Doe", age: 30 };
    callback(data);
  }, 2000);
}
```
#### Callback Hell
Overusing nested callbacks can lead to "callback hell", making code difficult to read and maintain. To address this, modern JavaScript introduced Promises and async/await.

## 4. Promises
A Promise in JavaScript represents the eventual completion (or failure) of an asynchronous operation and its resulting value. It's a way to handle asynchronous code in a more structured and cleaner way than traditional callbacks.
#### States->
- Pending
- Fulfilled
- Rejected
Resolve: A function that fulfills the Promise with a value.

Reject: A function that rejects the Promise with an error.

Thenable: An object that implements a then method.
- A thenable is simply an object that implements a then method. This then method accepts two optional arguments:
- ```
  const myThenable = {
  then(resolve, reject) {
    // Simulate asynchronous operation
    setTimeout(() => {
      const data = { message: "Hello from thenable" };
      resolve(data); // Or reject(new Error('Something went wrong'))
    }, 1000);
  }
  };
  myThenable.then(
    (data) => console.log(data),
    (error) => console.error(error)
  );
  ```
  Promise Example:
  ```
    const promise = new Promise((resolve, reject) => {
  // Asynchronous operation
  if (/* condition for success */) {
    resolve(value); // Fulfill the promise
  } else {
    reject(error); // Reject the promise
  }
  });
  ```
  Consuming A promise:
  ```
    promise.then(value => {
  // Handle successful result
  }, error => {
    // Handle error
  });
  ```
#### Chaining Promises
Promises can be chained using multiple then methods.
```
promise
  .then(value1 => {
    return value1 * 2; // Return a new promise
  })
  .then(value2 => {
    console.log(value2); // Handle the doubled value
  })
  .catch(error => {
    console.error(error);
  });
```
#### Methods :
- then()
- catch()
- finally()
- Promise.all([]) //accept array
- Promise.race() //array which one completes first...
  
## 5. Promisification
Promisification is the process of transforming a function that uses a callback to one that returns a Promise. This is particularly useful for dealing with older APIs or libraries that might not inherently support Promises.
```
function promisify(fn) {
  return function (...args) {
    return new Promise((resolve, reject) => {
      fn(...args, (err, result) => {
        if (err) {
          reject(err);
        } else {
          resolve(result);   

        }
      });
    });
  };
}
const promisifiedReadFile = promisify(readFile);

promisifiedReadFile('path/to/file')
  .then(data => console.log(data))
  .catch(err => console.error(err));
```
