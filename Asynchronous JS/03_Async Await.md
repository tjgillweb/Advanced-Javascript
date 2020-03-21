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

**Example:**
#### Promises Version
```Javascript
function makeRequest(location) {
    return new Promise((resolve, reject) => {
        console.log(`Making request to ${location}`);
        if (location === "Google") {
            resolve("Google says hi");
        } else {
            reject("We can only talk to Google");
        }
    })
}

function processRequest(response) {
    return new Promise((resolve, reject) => {
        console.log('Processing response');
        resolve(`Extra information + ${response}`)
    })
}

makeRequest('Google').then(response => {
    console.log('Response Received');
    return processRequest(response)
}).then(processedResponse => {
    console.log(processedResponse);
}).catch(err => {
    console.log(err);
})
```
#### Async Version
```Javascript
function makeRequest(location) {
    return new Promise((resolve, reject) => {
        console.log(`Making request to ${location}`);
        if (location === "Google") {
            resolve("Google says hi");
        } else {
            reject("We can only talk to Google");
        }
    })
}

function processRequest(response) {
    return new Promise((resolve, reject) => {
        console.log('Processing response');
        resolve(`Extra information + ${response}`)
    })
}

async function doWork() {
    try {
        const response = await makeRequest('Google');
        console.log('Response Received');
        const processedResponse = await processRequest(response);
        console.log(processedResponse);
    }
    catch(err){
        console.log(err);
    }
}
doWork();
```
