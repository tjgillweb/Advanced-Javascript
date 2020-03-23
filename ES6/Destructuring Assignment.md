## Destructuring Assignment
- The destructuring assignment makes it possible to extract data from arrays or objects into distinct variables. This can be useful if you want to assign multiple variables at once.

We can use Destructuring Assignment:
1. to Extract Values from Objects
2. to Assign Variables from Objects
3. to Assign Variables from Nested Objects
4. to Assign Variables from Arrays
5. with the Rest Parameter to Reassign Array Elements
6. to Pass an Object as a Function's Parameters

### Extract Values from Objects
```Javascript
const user = {
    name: 'John Doe',
    age: 28
};
//ES5 assignment
const name = user.name; // name = 'John Doe'
const age = user.age; // age = 34

// ES6 destructuring syntax
const {name, age} = user; // name = 'John Doe', age = 34
```

### Assign Variables from Objects
Destructuring allows you to assign a new variable name when extracting values. You can do this by putting the new name after a colon when assigning the value.
```Javascript
const user = { name: 'John Doe', age: 34 };

const { name: userName, age: userAge } = user;
// userName = 'John Doe', userAge = 34
```
You may read it as "get the value of user.name and assign it to a new variable named userName" 

### Assign Variables from Nested Objects
```Javascript
const user = {
  johnDoe: { 
    age: 34,
    email: 'johnDoe@freeCodeCamp.com'
  }
};
//extract the values of object properties and assign them to variables with the same name
const { johnDoe: { age, email }} = user;
```
```Javascript
//and here's how you can assign an object properties' values to variables with different names
const { johnDoe: { age: userAge, email: userEmail }} = user;
```
### Assign Variables from Arrays
- In the same way that we can destructure objects, we can also destructure arrays (which are really just a special type of an object).
- One key `difference between the spread operator and array destructuring` is that the spread operator unpacks all contents of an array into a comma-separated list. Consequently, you cannot pick or choose which elements you want to assign to variables.
- Destructuring an array lets us do exactly that:
```Javascript
const [a, b] = [1, 2, 3, 4, 5, 6];
console.log(a, b); // 1, 2
```
- The variable `a` is assigned the `first value of the array`, and `b` is assigned the `second value of the array`. We can also access the value at any index in an array with destructuring by using commas to reach the desired index:
```Javascript
const [a, b,,, c] = [1, 2, 3, 4, 5, 6];
console.log(a, b, c); // 1, 2, 5
```

#### Swapping Values
Array destructuring also allows us to swap the values in two variables succinctly.
```Javascript
const [a, b] = [1, 2];
a; // 1
b; // 2

[a, b] = [b, a];
a; // 2
b; // 1
```

### with the Rest Parameter to Reassign Array Elements
In some situations involving array destructuring, we might want to collect the rest of the elements into a separate array.  
The result is similar to `Array.prototype.slice()`, as shown below:
```Javascript
const [a, b, ...arr] = [1, 2, 3, 4, 5, 7];
console.log(a, b); // 1, 2
console.log(arr); // [3, 4, 5, 7]
```

### Pass an Object as a Function's Parameters
In some cases, you can destructure the object in a function argument itself.
```Javascript
const profileUpdate = (profileData) => {
  const { name, age, nationality, location } = profileData;
  // do something with these variables
}
```
This effectively destructures the object sent into the function. This can also be done in-place:
```Javascript
const profileUpdate = ({ name, age, nationality, location }) => {
  /* do something with these fields */
}
```
This removes some extra lines and makes our code look neat. This has the added benefit of not having to manipulate an entire object in a function — only the fields that are needed are copied inside the function.

Another example:
```Javascript
const stats = {
  max: 56.78,
  standard_deviation: 4.34,
  median: 34.54,
  mode: 23.87,
  min: -0.75,
  average: 35.85
};

const half = (stats) => (stats.max + stats.min) / 2.0; 
```
