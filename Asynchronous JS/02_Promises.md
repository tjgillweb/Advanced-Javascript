# Promises
- A promise is an object that represents a task that will be completed in the future.
- Promises are almost always preferred over callbacks when managing asynchronous code.  
- Promises make use of callback functions, but help avoid deep nesting of callbacks and improve readability. 
- Another advantage of promises is that they are immutable, so once a promise is done, you can not accidentally call it again (something you can do accidentally with callbacks).
- Think of a promise as a **one-time guarantee of future value**.

**Example:**
Have you ever been to a busy restaurant without a reservation? When this happens, the restaurant needs a way to get back in contact with you when a table opens up.  
1. One solution instead of taking your name, is that they take your number and text you once a table opened up. This allowed them to target your phone with ads whenever they wanted. It’s a metaphor for `callbacks`! Giving your number to a restaurant is just like giving a callback function to a third party service. You expect the restaurant to text you when a table opens up, just like you expect the third party service to invoke your function when and how they said they would. Once your number or callback function is in their hands though, you’ve lost all control. So, this leads to the problem of **Inversion of Control** as discussed above.
2. The other soluttion is that little buzzer thing they give you. When the device starts buzzing and glowing, your table is ready. You can still do whatever you’d like as you’re waiting for your table to open up, but now you don’t have to give up anything. In fact, it’s the exact opposite. They have to give you something. `There is no inversion of control`.

The buzzer will always be in one of three different states - `pending`, `fulfilled`, or `rejected`
1. `pending` is the default, initial state. When they give you the buzzer, it’s in this state.
2. `fulfilled` is the state the buzzer is in when it’s flashing and your table is ready.
3. `rejected` is the state the buzzer is in when something goes wrong. Maybe the restaurant is about to close or they forgot someone rented out the restaurant for the night.

> If giving the restaurant your number is like giving them a callback function, receiving the little buzzy thing is like receiving what’s called a “Promise”.

Exactly like the buzzer, a Promise can be in one of three states, pending, fulfilled or rejected, where they represent the status of an asynchronous request.
1. If the async request is still ongoing, the Promise will have a status of `pending`
2. If the async request was successfully completed, the Promise will change to a status of `fulfilled`. In other words `resolved`.
3. If the async request failed, the Promise will change to a status of `rejected`

Now that you understand why Promises exist and the different states they can be in, there are three more questions we need to answer.
1. **How do you create a Promise?**
2. **How do you change the status of a promise?**
3. **How do you listen for when the status of a promise changes?**

### How to create a Promise?
```Javascript
const promise = new Promise((resolve, reject) => {
setTimeout(() => {
    resolve() // Change status to 'fulfilled'
  }, 2000)
});
```
The Promise constructor function takes in a single argument, a (callback) function. This function is going to be passed two arguments, `resolve` and `reject`.

### How to change the status of a promise?

`resolve` - a function that allows you to change the status of the promise to fulfilled

`reject` - a function that allows you to change the status of the promise to rejected.

In the code above, we use setTimeout to wait 2 seconds and then invoke resolve. This will change the status of the promise to fulfilled.

We can see this change in action by logging the promise right after we create it and then again roughly 2 seconds later after resolve has been called. Notice the promise goes from `<pending>` to `<resolved>`.  
The promise is created first before the setTimeout() finishes and resolves.

![](images/resolve.gif)

### How to listen for when the status of a promise changes?
- When you create a new Promise, you’re really just creating a plain old JavaScript object. This object can invoke two methods, `then`, and `catch`.
- When the status of the promise changes to `resolved`, the function that was passed to `.then` will get invoked. 
- When the status of a promise changes to `rejected`, the function that was passed to `.catch` will be invoked. 
- Once you create a promise, you’ll pass the function you want to run if the async request is successful to .then. You’ll pass the function you want to run if the async request fails to .catch.
```Javascript
function onSuccess () {
  console.log('Success!')
}

function onError () {
  console.log('💩')
}

const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve()
  }, 2000)
})

promise.then(onSuccess)
promise.catch(onError)
```
**Another example:**  
We're randomly generating a time between 0 and 5 seconds, and storing it in a variable called timeToResolve. This is basically mimicking what happens when we make an AJAX request: we have no idea how long it will take for that request to complete.
```Javascript
function secondPromise() {
  return new Promise(function(resolve, reject) {
    var timeToResolve = Math.random() * 5000;
    var maxTime = 3000;
    if (timeToResolve < maxTime) {
      setTimeout(function() {
        resolve(
          `Hooray! I completed your request after ${timeToResolve} milliseconds.`
        );
      }, timeToResolve);
    } else {
      setTimeout(function() {
        reject(
          `Sorry, this is taking too long. Stopping after ${maxTime} milliseconds. Please try again.`
        );
      }, maxTime);
    }
  });
}

secondPromise()
  .then(function(data) {
    console.log(data);
  })
  .catch(function(error) {
    console.log(error);
  });
```
--------------------------------------------------------------------------------------------------------------------------------------
### Promises in jQuery
jQuery has built-in support for promises! This means that when you use methods like $.get, $.getJSON, or $.ajax, you can chain .then and .catch methods to the end of them.
```Javascript
function getJokesAbout(term) {
  return $.getJSON(`https://icanhazdadjoke.com/search?term=${term}`);
}

getJokesAbout("spider")
  .then(function(data) {
    console.log("Here is our joke data!", data);
  })
  .catch(function(err) {
    console.log("Oops, something went wrong", err);
  });
```

### Promise.all
 - We can wait for multiple promises to resolve with the `Promise.all` function.
 - Promise.all accepts an array of promises, and returns a new promise to you that will resolve only after each promise in the array has resolved. 
 - If any one of the promises in the array passed to Promoise.all gets rejected, the promise returned by Promise.all will be rejected too.
 - Promise.all is helpful if you want to fire off a lot of requests that don't have anything to do with one another. 
```Javascript
 function getJokesAbout(term) {
  return $.getJSON(`https://icanhazdadjoke.com/search?term=${term}`);
}

Promise.all([
  getJokesAbout("spider"),
  getJokesAbout("ghost"),
  getJokesAbout("pizza")
])
  .then(function(data) {
    console.log("Woah check out all this data", data);
  })
  .catch(function(err) {
    console.log("Oops, something went wrong!");
  });
```

### Promise Chaining

Sometimes you'll want to chain promises together sequentially, for instance, if you need the response from one resolved promise in order to create another promise.  

**Example of Promise Chaining:**  
In the example below, we call getPromise which returns us a promise that will resolve in at least 2000 milliseconds. From there, because .then will return a promise, we can continue to chain our .thens together until we throw a new Error which is caught by the .catch method.
```Javascript
function getPromise () {
  return new Promise((resolve) => {
    setTimeout(resolve, 2000)
  })
}

function logA () {
  console.log('A')
}

function logB () {
  console.log('B')
}

function logCAndThrow () {
  console.log('C')

  throw new Error()
}

function catchError () {
  console.log('Error!')
}

getPromise()
  .then(logA) // A
  .then(logB) // B
  .then(logCAndThrow) // C
  .catch(catchError) // Error!
```

### Escaping Callback Hell using Promise Chaining
- Fortunately, we can return the value of promise to another. This is another way for us to escape out of `callback hell`. Rather than deeply nesting callbacks, we can chain together multiple promises using .then  

**Example of `Callback hell`**
```Javascript
$.getJSON(
  "https://icanhazdadjoke.com/search?term=spider",
  function(data) {
    console.log("Here's some data on spider jokes: ", data);
    $.getJSON(
      "https://icanhazdadjoke.com/search?term=ghost",
      function(data) {
        console.log("Here's some data on ghost jokes: ", data);
        $.getJSON(
          "https://icanhazdadjoke.com/search?term=pizza",
          function(data) {
            console.log("Here's some data on pizza jokes: ", data);
          },
          function(error) {
            console.log("Oops something went wrong!", error);
          }
        );
      },
      function(error) {
        console.log("Oops something went wrong!", error);
      }
    );
  },
  function(error) {
    console.log("Oops something went wrong!", error);
  }
);
```

**Refactored code using Promise Chaining**
```Javascript
$.getJSON("https://icanhazdadjoke.com/search?term=spider")
  .then(function(data) {
    console.log("Here's some data on spider jokes: ", data);
    return $.getJSON("https://icanhazdadjoke.com/search?term=ghost");
  })
  .then(function(data) {
    console.log("Here's some data on ghost jokes: ", data);
    return $.getJSON("https://icanhazdadjoke.com/search?term=pizza");
  })
  .then(function(data) {
    console.log("Here's some data on pizza jokes: ", data);
  })
  .catch(function(error) {
    console.log("Oops something went wrong!", error);
  });
```
