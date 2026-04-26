# Node.js Interview Questions & Answers

## What is Node.js?
Node.js is a JavaScript runtime built on Chrome's V8 engine that allows JavaScript to run on the server side. It uses an event-driven, non-blocking I/O model that makes it efficient and suitable for real-time applications.

## What is the difference between Node.js and JavaScript?
JavaScript is a programming language that runs in browsers. Node.js is a runtime environment that executes JavaScript outside the browser, enabling server-side programming.

## What is npm?
npm (Node Package Manager) is the default package manager for Node.js. It allows developers to install, share, and manage dependencies in their projects.

## What is the Event Loop in Node.js?
The Event Loop is a mechanism that handles asynchronous operations in Node.js. It continuously checks for pending callbacks and executes them, allowing non-blocking I/O operations.

## What is the difference between blocking and non-blocking I/O?
- **Blocking I/O**: Operations that pause execution until the operation completes
- **Non-blocking I/O**: Operations that allow the program to continue executing while waiting for I/O to complete

## What are Promises?
Promises are objects representing the eventual completion or failure of an asynchronous operation. They have three states: pending, fulfilled, or rejected.

## What is async/await?
async/await is syntactic sugar built on top of Promises that makes asynchronous code look and behave more like synchronous code.

## What is Express.js?
Express.js is a minimal and flexible Node.js web application framework that provides robust features for web and mobile applications.

## What is middleware in Express?
Middleware functions are functions that have access to the request and response objects. They can execute code, modify request/response, end the cycle, or call the next middleware.

## What is the difference between process.nextTick() and setImmediate()?
- `process.nextTick()`: Adds callback to the next tick queue, executes before any other I/O events
- `setImmediate()`: Executes callbacks in the check phase of the event loop, after I/O callbacks

## What is the purpose of the Buffer class in Node.js?
Buffer is used to handle binary data directly. It's particularly useful when working with streams, file I/O, and network communications.

## What is a stream in Node.js?
Streams are objects that let you read or write data continuously. There are four types: Readable, Writable, Duplex, and Transform streams.

## How do you handle errors in Node.js?
Errors can be handled using try/catch blocks for synchronous code, and `.catch()` or error-first callbacks for asynchronous operations.

## What is the Cluster module in Node.js?
The Cluster module allows a Node.js application to run in multiple child processes that share the same server port, enabling load balancing.

## What is the difference between require() and import?
- `require()`: CommonJS module syntax, synchronous, can be used anywhere
- `import()`: ES6 module syntax, asynchronous, must be at the top of the file (or use dynamic imports)

## What is the purpose of module.exports?
module.exports is used to export functions, objects, or values from a Node.js module so they can be required in other files.

## What is the EventEmitter class?
EventEmitter is a module that allows objects to emit events and register listeners. It's the backbone of Node.js event-driven architecture.

## How does Node.js handle child processes?
Node.js can create child processes using the `child_process` module with methods like `spawn()`, `exec()`, `execFile()`, and `fork()`.

## What is REPL in Node.js?
REPL (Read-Eval-Print Loop) is an interactive shell that reads, evaluates, and prints results of JavaScript code. Access it by running `node` in terminal.

## What are callbacks in Node.js?
Callbacks are functions passed as arguments to asynchronous functions that are executed once the operation completes.

## How do you prevent callback hell?
Use Promises, async/await, or modularize code into smaller functions to avoid deeply nested callbacks.

## What is the purpose of the path module?
The path module provides utilities for working with file and directory paths, like joining, resolving, and parsing paths.

## What is the difference between fs.readFile() and fs.createReadStream()?
- `fs.readFile()`: Reads entire file into memory before processing
- `fs.createReadStream()`: Reads file in chunks, better for large files and memory efficiency

## What is the purpose of the crypto module in Node.js?
The crypto module provides cryptographic functionality including encryption, decryption, hashing, and digital signatures. It wraps OpenSSL functions for hashing (sha256, md5), HMAC, ciphers (AES, DES), and creating certificates.

## What is the difference between Array.from() and Array.of()?
- `Array.from()`: Creates an array from array-like or iterable objects (e.g., arguments, NodeList)
- `Array.of()`: Creates an array from a variable number of arguments, regardless of count

## What are Worker Threads in Node.js?
Worker Threads enable parallel execution of JavaScript code. They are useful for CPU-intensive tasks as they run in isolation with their own event loop and V8 instance, avoiding blocking the main thread.

## What is the purpose of the util module in Node.js?
The util module provides utility functions like `util.inspect()` for debugging, `util.inherits()` for prototype inheritance, and `util.promisify()` to convert callback-based functions to Promise-based ones.

## What is the difference between setTimeout() and setImmediate()?
- `setTimeout()`: Schedules callback to execute after a specified delay in the timer phase
- `setImmediate()`: Executes callback in the check phase immediately after I/O callbacks, not tied to a delay

## How does Node.js handle database connections?
Node.js connects to databases using driver libraries (e.g., mysql2, pg, mongodb). These can use callback-based, Promise-based, or async/await patterns. Connection pooling is commonly used to manage multiple connections efficiently.

## What is the purpose of the net module in Node.js?
The net module provides asynchronous network API for creating TCP servers and clients. It handles streaming data and connection-based communication at the transport layer.

## What is the purpose of the os module in Node.js?
The os module provides operating system-related utility methods and properties. It gives information about CPU architecture, free/used memory, network interfaces, and platform details.

## What is the difference between Node.js timers and browser timers?
Node.js timers (`setTimeout`, `setInterval`) behave similarly to browser timers but are implemented differently. Node.js timers are part of the `timers` module and integrate with the libuv event loop, while browser timers are part of the HTML spec and use different timing mechanisms.

## What is the purpose of the zlib module in Node.js?
The zlib module provides compression and decompression functionality using Gzip, Deflate, and Brotli algorithms. It's commonly used for compressing HTTP responses and file streams.

## What is the purpose of the DNS module in Node.js?
The dns module enables name resolution (converting domain names to IP addresses) and DNS lookups. It offers both callback-based and Promise-based methods like `lookup()`, `resolve()`, and `reverse()`.

## What are the key differences between CommonJS and ES modules in Node.js?
CommonJS (`require/module.exports`) loads synchronously at runtime, while ES modules (`import/export`) are statically analyzed at parse time. ES modules support dynamic imports, top-level await, and better tree-shaking, but require the `"type": "module"` field in package.json.

## What are the phases of the Node.js Event Loop?
The Event Loop has six phases: timers (executes callbacks from setTimeout/setInterval), pending callbacks (I/O callbacks deferred), idle/prepare (internal use), poll (retrieves new I/O events, executes I/O-related callbacks), check (setImmediate callbacks), and close callbacks (socket.on('close')). Each phase has a FIFO queue of callbacks, and the loop continues until all phases are empty or process.exit() is called.