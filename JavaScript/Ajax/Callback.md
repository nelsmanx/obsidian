`Callback` (колбэк, функция обратного вызова) — это функция, которая передается в качестве аргумента другой функции и выполняется после завершения некоторых операций.

A `callback` function is a function that is passed as an argument to another function and is executed after the completion of some operations.

This mechanism allows JavaScript to perform tasks like reading files, making HTTP requests, or waiting for user input without blocking the execution of the program. This helps ensure a smooth user experience.

### Why Use Callback Functions?

JavaScript runs in a single-threaded environment, meaning it can only execute one command at a time. Callback functions help manage asynchronous operations, ensuring that the code continues to run smoothly without waiting for tasks to complete. This approach is crucial for maintaining a responsive and efficient program.

```js
 const greet = (name, callback) => {
  console.log(`Hello, ${name}`);
  callback();
};

const sayGoodbye = () => console.log("Goodbye!");

greet('Alice', sayGoodbye);
```

Instead of a named ( pre-defined - переменная, хранящая функцию) function, you can pass an anonymous one:

```js
const greet = (name, callback) => {
  console.log(`Hello, ${name}`);
  callback();
};

greet('Alice', () => console.log("Goodbye!"));
```

In this code:
- `greet` is a function that takes a name and a callback function as arguments.
- After greeting the user, it calls the callback function.

## How Callbacks Work

1. **Passing the Function:** The function you want to run after some operation is passed as an argument to another function.
2. **Executing the Callback:** The main function executes the callback function at the appropriate time. This can be after a delay, once a task is complete, or when an event occurs.

Here’s a more detailed example with a simulated asynchronous operation using `setTimeout`:

```js
function fetchData(callback) {
  setTimeout(() => {
    const data = { id: 1, name: "Alice" };
    callback(data);
  }, 2000); // Simulating a delay of 2 seconds
}

fetchData((data) => {
  console.log("Data received:", data);
});
```

In this example:
- `fetchData` simulates fetching data after a 2-second delay.
- The callback function logs the data once it's received.

## How to Handle Errors with Callbacks

In real-world scenarios, you'll often need to handle errors. A common pattern is to pass an error as the first argument to the callback function:

```js
function readFile(filePath, callback) {
  const fs = require('fs');
  fs.readFile(filePath, 'utf8', (err, data) => {
    if (err) {
      callback(err, null);
    } else {
      callback(null, data);
    }
  });
}

readFile('example.txt', (err, data) => {
  if (err) {
    console.error("Error reading file:", err);
  } else {
    console.log("File content:", data);
  }
});
```

Here:
- The `readFile` function reads a file asynchronously.
- It calls the callback with an error (if any) or the file data.

## The Callback Hell Problem

As applications grow, using multiple nested callbacks can become complex and hard to manage, often referred to as "callback hell." Here’s an example of callback hell:

```js
function stepOne(callback) {
  setTimeout(() => callback('Step One Completed'), 1000);
}

function stepTwo(callback) {
  setTimeout(() => callback('Step Two Completed'), 1000);
}

function stepThree(callback) {
  setTimeout(() => callback('Step Three Completed'), 1000);
}

stepOne((message) => {
  console.log(message);
  stepTwo((message) => {
	console.log(message)
	stepThree((message) => {
	  console.log(message)
	})
  })
})
```

This code is difficult to read and maintain. To solve this, modern JavaScript provides `Promises` and `async/await` syntax, making code cleaner and easier to handle.

![[How to Use Callback Functions in JavaScript.mhtml]]