## Rest Operator(use with Function Parameters)
- With the rest parameter, you can create functions that take a variable number of arguments. These arguments are stored in an array that can be accessed later from inside the function.
- It is used inside of a list of function parameters in the definition of a function to indicate the "rest" of the arguments.
- you can think of it like the `"rest" of the arguments`.
```Javascript
function data(a,b,...c){
    console.log(a,b,c);
}

data(1,2,3,4,5); // 1, 2, [3,4,5]
```

- Using the rest operator is often a useful way to avoid dealing with the `arguments` array-like object.
- `arguments` can be a little tricky to deal with, since it's not actually an array and therefore doesn't have access to common array methods. However, when you use the rest operator, what you get access to inside of your function is a bona fide array.
- The rest parameter eliminates the need to check the args array and allows us to apply map(), filter() and reduce() on the parameters array.
```Javascript
function multiply() {
    let product = 1;
    for (let i = 0; i < arguments.length; i++) {
        product *= arguments[i];
    }
    return product;
}

function multiplyES2015(...nums) {
    return nums.reduce((product, num) => product * num, 1);
}

multiply(1, 2, 3, 4); // 24
multiplyES2015(1, 2, 3, 4);
```
