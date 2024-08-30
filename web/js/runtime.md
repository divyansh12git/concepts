## 1. JavaScript Runtime:
JavaScript Runtime is a program that executes JavaScript code. It's the environment where JavaScript code runs, providing the necessary tools and mechanisms to interpret and execute the code.

- ### JavaScript Engine:
    - The core component responsible for parsing, interpreting, and executing JavaScript code.
    - Examples include V8 (used in Chrome and Node.js), SpiderMonkey (used in Firefox), and JavaScriptCore (used in Safari).   
    - Optimizes code execution through techniques like just-in-time (JIT) compilation.
- ### Call Stack:
    - A data structure used to keep track of function calls and their return addresses.
    - When a function is called, its context is pushed onto the stack. When the function returns, its context is popped off the stack.
- ### Heap:
    - A memory area used to store objects and their properties.
    - Objects are allocated on the heap when they are created.
    - The garbage collector is responsible for freeing memory that is no longer in use.
-  ### Web APIs:
    - Interfaces provided by the browser or Node.js environment that allow JavaScript code to interact with the outside world.
    - Examples include DOM APIs for interacting with web pages, network APIs for making HTTP requests, and file system APIs for working with files.

### Runtime->
1. ####  **Parsing:**
    - The JavaScript engine starts by parsing the JavaScript code. The code is parsed into an Abstract Syntax Tree (AST), which represents the structure of the code in a tree format.
    - The parser checks for syntax errors during this phase. If any errors are found, the engine stops execution and throws an error.
2. #### **Compilation:**
    - JavaScript engines, like Google's V8 (used in Chrome and Node.js), use Just-In-Time (JIT) compilation. This means the code is compiled to machine code at runtime, not before execution.
    - The engine optimizes the code during this phase. Frequently executed code paths are optimized further for better performance.
3. #### **Execution:**
The JavaScript code is executed in the execution context. The context has two main components:

- **Memory Component (Variable Environment):** Stores variables and function declarations.
- **Thread of Execution:** The actual execution of code line by line.

The JavaScript engine creates an Execution Context for each function invocation, consisting of:

- **Global Execution Context:** Created when the script first runs, containing global variables and functions.
- **Function Execution Context:** Created whenever a function is called, containing the function’s scope and variables.

4. #### **Call Stack:**
    - ***JavaScript is single-threaded***, meaning it can execute one command at a time. The Call Stack is a data structure used to manage the execution contexts.
    - When a function is called, it’s added to the top of the Call Stack. When the function completes, it’s removed from the stack, and control returns to the previous context.
5. #### **Event Loop and Callback Queue:**
    - The Event Loop is what allows JavaScript to handle asynchronous operations like API requests or timers.
    - When an asynchronous operation completes, the callback function is added to the Callback Queue.
    - The Event Loop continuously checks the Call Stack. If the stack is empty, it pushes the first function in the Callback Queue to the stack for execution.
    - This allows JavaScript to perform non-blocking operations, enabling it to remain responsive even during long-running tasks.
6. #### **Garbage Collection**
    - JavaScript engines perform automatic memory management through garbage collection. The engine periodically checks for and frees up memory that is no longer in use, preventing memory leaks.

## Execution

JavaScript starts by executing synchronous code line by line in the Call Stack.
If a function or operation is synchronous, it is pushed onto the Call Stack and executed immediately.
###  Handling Asynchronous Code:
- When JavaScript encounters an asynchronous operation (e.g., setTimeout, fetch, or a promise), it does the following:
    - Web APIs: The operation is handed off to a Web API provided by the browser or Node.js runtime. For instance, setTimeout triggers a timer in the Web API environment, while a fetch request starts an HTTP request in the Web API.
    - The original function or code is then removed from the Call Stack, allowing the JavaScript engine to continue executing other code.
- ####  Callback Queue (Task Queue):
    - Once an asynchronous operation is complete (e.g., a timer expires, or an HTTP request completes), its callback function is placed in the Callback Queue.
    - Examples include: Timers, I/O Operations.
- ####  Microtask Queue:
    - Microtasks (e.g., resolved promises, process.nextTick in Node.js) have higher priority than regular tasks.
    - When a promise is resolved or rejected, its .then or .catch handler is added to the Microtask Queue.
    - The Event Loop prioritizes the Microtask Queue over the Callback Queue, meaning all microtasks are processed before moving on to tasks in the Callback Queue.
- ### Event Loop Execution:
- The Event Loop continuously checks if the Call Stack is empty. If it is:
    - Step 1: The Event Loop checks the Microtask Queue. If there are any microtasks, it processes them first. This includes resolving promises or handling other high-priority asynchronous tasks.
    - Step 2: After clearing the Microtask Queue, the Event Loop then checks the Callback Queue. If there are tasks in the Callback Queue, the first task is moved to the Call Stack for execution. 
    - Step 3: This process repeats, allowing JavaScript to manage both synchronous and asynchronous tasks efficiently.

--- The Event Loop also allows time for garbage collection, where memory that is no longer needed is freed up.