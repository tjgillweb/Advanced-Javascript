# Async/Await

- Async/Await is just syntactical sugar wrapped around making Promises easier to work with.
- These two keywords allow us to write code that "looks" synchronous, but is actually asynchronous. 
- We prefix our functions with the `async` keyword to denote that they are actually asynchronous and we invoke those functions with the keyword `await` to ensure that they have completed before their values are stored.

> Note that when you make a funciton an async function, it automatically returns a promise to you. Very commonly you'll create the promise yourself, but even if you don't, the value you return will be wrapped in a promise:

```Javascript
async function asyncExample() {
  return 1;
}
asyncExample(); // Promise {<resolved>: 1}
```

