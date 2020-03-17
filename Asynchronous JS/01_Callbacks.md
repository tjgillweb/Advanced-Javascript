# Callback Functions

- A callback function is defined as a function that is passed into another function as a parameter and then invoked by that other function.
- A callback is passed as a parameter inside of a higher order function.

**Example:**
```Javascript
function callback(){
  console.log("Coming from callback");
}
function higherOrder(fn){
  console.log("About to call callback");
  fn(); // Callback function is invoked
  console.log("Callback function has been invoked");
}
higherOrder(callback);
```

## Higher Order Function
- A higher order function is a function that accepts a callback as a parameter.

```Javascript
function add (x,y) {
  return x + y
}
function higherOrderFunction (x, callback) {
  return callback(x, 5)
}
higherOrderFunction(10, add)
```

### Code Reuse with Callbacks
**Example:**
```Javascript
//Duplicate code without Callbacks
function sendMessageConsole(message){
    console.log(message);
}
function sendMessageAlert(message){
    alert(message);
}
function sendMessageConfirm(message){
    return confirm(message);
}
sendMessageAlert("Lots of duplication");
```

```Javascript
//Code Reuse with Callbacks
function sendMessage(message, callback){
    return callback(message);
}

sendMessage("Message for console", console.log);
sendMessage("Message for alert", alert);

var answer = sendMessage("Are you sure?", confirm);
```

### Callbacks with Function Declarations
```Javascript
function greet(name, formatter){
    return 'Hello, ' + formatter(name);
}
function upperCaseName(name){
    return name.toUpperCase();
}
greet('Tim', upperCaseName);
```

### Callbacks with Anonymous Functions
```Javascript
function greet(name, formatter) {
    console.log('Hello, ' + formatter(name));
}
greet('Tim', function (name) {
    return name.toUpperCase();
});
greet('Tim', function (name) {
    return name + '!!!!';
});
```

```
//OUTPUT:
Hello, TIM
Hello, Tim!!!!
```

### Callbacks Use Cases (What are callbacks used for?)
1. Advanced Array Methods
2. Browser Events (such as click, submit, DOMContentLoaded all accept callbacks which will be invoked when event takes place).
3. AJAX Requests
4. React Development

### Advanced Array Methods

```Javascript
[1,2,3].map((i) => i + 5)

_.filter([1,2,3,4], (n) => n % 2 === 0 );
```
- The .map and _.filter examples, is a nice abstraction over turning one value into another.   
We say “Hey, here’s an array and a function. Go ahead and get me a new value based on the function I gave you”.

**Implementation of `forEach` using Callback**
```Javascript
//Print array values doubled
// First, using normal for loop
let arr = [1,2,3,4,5,6];
function double(arr){
    for (let i = 0; i < arr.length; i++){
        console.log(arr[i] * 2);
    }
}
double(arr);

//Using forEach
let arr = [1,2,3,4,5,6];
forEach(arr, function(number) {
    console.log(number * 2);
});
```

**forEach Function Definition**
```Javascript
function forEach(array, callback){
  // to be implemented
}

//Callback signature
function callback(currentElement, currentIndex, array){
  // implemented by the caller of forEach
}
```
**forEach implementation using callback**
```Javascript
function forEach(arr, callback){
  for(let i = 0; i < arr.length; i++){
    callback(arr[i], i, arr);
  }
}
```

### Browser Events
```Javascript
$('#btn').on('click', () =>
  console.log('Callbacks are everywhere')
)
```
- Callbacks are used when we are delaying execution of a function until a particular time.   
"Hey, here’s this function. Go ahead and invoke it whenever the element with an id of btn is clicked."

### AJAX Requests
- Most of the apps we build don’t have all the data they need up front. Instead, they need to fetch external data as the user interacts with the app. 
- We’ve just seen how callbacks can be a great use case for this because, again, they allow you to “delay execution of a function until a particular time”.
- Instead of delaying execution of a function until a particular time, we can delay execution of a function until we have the data we need.

Here’s probably the most popular example of this, jQuery’s getJSON method.

```Javascript
// updateUI and showError are irrelevant.
// Pretend they do what they sound like.

const id = 'tjgillweb'

$.getJSON({
  url: `https://api.github.com/users/${id}`,
  success: updateUI,
  error: showError,
})
```
- We can’t update the UI of our app until we have the user’s data. 
- So what do we do? We say, “Hey, here’s an object. If the request succeeds, go ahead and call success passing it the user’s data. If it doesn’t, go ahead and call error passing it the error object. 

### Problems with Callbacks:
1. Callback Hell
2. Inversion of Control

### Callback Hell
```Javascript
// updateUI, showError, and getLocationURL are irrelevant.
// Pretend they do what they sound like.

const id = 'tylermcginnis'

$("#btn").on("click", () => {
  $.getJSON({
    url: `https://api.github.com/users/${id}`,
    success: (user) => {
      $.getJSON({
        url: getLocationURL(user.location.split(',')),
        success (weather) {
          updateUI({
            user,
            weather: weather.query.results
          })
        },
        error: showError,
      })
    },
    error: showError
  })
})
```
First we’re saying don’t run the initial AJAX request until the element with an id of btn is clicked. Once the button is clicked, we make the first request. If that request succeeds, we make a second request. If that request succeeds, we invoke the updateUI method passing it the data we got from both requests. Regardless of if you understood the code at first glance or not, objectively it’s much harder to read than the code before. This is referred to as **Callback Hell**.  

As humans, we naturally think sequentially. When you have nested callbacks inside of nested callbacks, it forces you out of your natural way of thinking. Bugs happen when there’s a disconnect between how your software is read and how you naturally think.  

**Solution:** Modularize the code
```Javascript
function getUser(id, onSuccess, onFailure) {
  $.getJSON({
    url: `https://api.github.com/users/${id}`,
    success: onSuccess,
    error: onFailure
  })
}

function getWeather(user, onSuccess, onFailure) {
  $.getJSON({
    url: getLocationURL(user.location.split(',')),
    success: onSuccess,
    error: onFailure,
  })
}

$("#btn").on("click", () => {
  getUser("tylermcginnis", (user) => {
    getWeather(user, (weather) => {
      updateUI({
        user,
        weather: weather.query.results
      })
    }, showError)
  }, showError)
})
```
So, here we’ve just put a band-aid over the readability issue of Callback Hell. The problem still exists that we naturally think sequentially and, even with the extra functions, nested callbacks break us out of that sequential way of thinking.

### Inversion of Control
- When you write a callback, you’re assuming that the program you’re giving the callback to is responsible and will call it when (and only when) it’s supposed to. You’re essentially inverting the control of your program over to another program.  
- When you’re dealing with libraries like jQuery, lodash, or even vanilla JavaScript, it’s safe to assume that the callback function will be invoked at the correct time with the correct arguments.
- However, for many third party libraries, callback functions are the interface for how you interact with them. It’s entirely plausible that a third party library could, whether on purpose or accidentally, break how they interact with your callback.
```Javascript
function criticalFunction () {
  // It's critical that this function
  // gets called and with the correct
  // arguments.
}
thirdPartyLib(criticalFunction)
```
Since you’re not the one calling `criticalFunction`, you have 0 control over when and with what argument it’s invoked. Most of the time this isn’t an issue, but when it is, it’s a big one.  

To solve these issues, we head over to `Promises`
