# Functions

Functions keep collections of instructions that are executed together. If we are always brushing our teeth, taking a shower, and eating breakfast in the morning, we can put those instructions in a function and just call that function whenever we are ready to do our morning routine.

It would look something like this:

```JavaScript
function morningRoutine() {
  brush_teeth
  take_shower
  eat_breakfast
}

morningRoutine();
```

## Contents

* Syntax
* Return value
* Arguments
* First-class objects
* Exercises

---

## Syntax

Let's break down the syntax. The bare minimum of a function is the `function` keyword, parentheses `()`, and a opening and closing brace between which we'll put the code to run.

```JavaScript
function () {

}
```

A function without a name is called an anonymous function. Anonymous functions are really useful for a lot of things, but the downside is that there is no way for us to reference anonymous functions. We need to give it a name (or assign to a variable, more on that below) so that we can talk about it.

We declare a function like so:

```JavaScript
function myFunctionName() {
  console.log('my function');
}
```

And then invoke the function like this:

```JavaScript
myFunctionName();
```

---

## Return value

Functions will always return `undefined` by default.

```JavaScript
function lol() {

}

lol(); //=> undefined
```

Even when we have some code in the function body, it returns `undefined` by default. In the example below, note that the string get printed to the screen, but our return value, the last value, is `undefined`.

```JavaScript
function lol() {
  console.log('this is coming from the function')
}

lol();
//=> 'this is coming from the function'
//=> undefined
```

We almost always care about the return value of a function, and we can use the `return` keyword to override the default return value to have the function return whatever we want.

```JavaScript
function hello() {
  return 'Hello, World!'
}

hello(); //=> "Hello, World!"
```

---

## Arguments

Our code becomes more interesting when we can pass data to it and make it behave in different ways. What if we wanted to switch up `"Hello, World"`, and say hi to different people?

In the function declaration we can specify arguments that our function will accept. These will be assigned data that is passed in when we invoke the function.

```JavaScript
function hello(name) {
  return 'Hello, ' + name + "!";
}

hello("Rey"); //=> "Hello, Rey!"
```

A function can take multiple arguments, but it is not necessary to pass all arguments in when invoking the function. Arguments are assigned to values by position, so if there's not enough value, some will simply equal `undefined`.

```JavaScript
function hello(name, question) {
  return 'Hello, ' + name + "! " + question;
}

hello("Rey"); //=> "Hello, Rey! undefined"

hello("Rey", "Where are you heading?"); //=> "Hello, Rey! Where are you heading?"
```

---

## First-class objects

Functions are so called first-class object in JavaScript. It makes them really powerful - they can be assigned to variables, passed to other functions as arguments, stored in data structures, and returned from functions.

* functions can be assigned to variables

```JavaScript
var hello = function () {
  return "Hello, World!"
}

hello //=> [Function]
hello(); //=> "Hello, World!"
```

* functions can be passed to other functions as arguments

**Note:** JavaScript functions that are passed to other functions are typically called _callbacks_

```JavaScript
var hello = function () {
  return "Hello, World!"
}

function runCode(callback) {
  return callback();
}

runCode(hello);
```

* functions can be stored in data structures

```JavaScript
var hello = function () {
  return "Hello, World!"
}

var goToWork = function () {
  return "Hello, World!"
}

var actions = [hello, goToWork];

actions[1] //=> [Function: goToWork]
actions[1]();
```

* functions can be returned from other functions

```JavaScript
function sayHelloLater(name) {
  return function () {
    "Hello, " + name + "!"
  }
}

var helloLater = sayHelloLater("Kylo Ren");

helloLater //=> [Function]
helloLater() //=> "Hello, Kylo Ren!";
```

---

## Exercises

1.

Create a function called `dinnerIsReady`. The function should return the string `"Dinner is ready!"` when invoked.

2.

Create a function called `foodIsReady`. The function should take one argument, `meal`, which specifies which meal is ready. For example, if the function is invoked with `"Lunch"`, it should print `"Lunch is ready!"`.

3.

Create a function called `introduceMyself`. The function takes four arguments, `name`, `age`, `homeTown`, and `profession`. When invoked and passed all four arguments, it should return: `"Hello! My name is NAME and I am AGE years old. I live in HOMETOWN. I work as a PROFESSION".`.

If the function is not passed the last argument, `profession`, omit that sentence from the statement.

Expected results:

```JavaScript
introducyMyself("Nick", "36", "Kansas City", "Waiter"); // "Hello! My name is Nick and I am 36 years old. I live in Kansas City. I work as a Waiter."

introducyMyself("Bella", "24", "Houston"); // "Hello! My name is Bella and I am 24 years old. I live in Houston. "
```

4.

Create a function called `computeSquare`. It should take one argument, a `number`. When invoked, it should return the square of that number.

Expected results:

```JavaScript
computeSquare(10); //=> 100
computeSquare(5); //=> 25
computeSquare(12); //=> 144
```

When the number is even, compute the square. When the number is odd, mulitply it by `10` and return the product.

Expected results:

```JavaScript
computeSquare(10); //=> 100
computeSquare(5); //=> 50
computeSquare(11); //=> 110
```

5.

Create a function called `firstAndLast`. It takes one argument, a `word` which is a string. When invoked, the function should return the first and the last letter of the `word`.

Expected results:

```JavaScript
firstAndLast("turing"); //=> "tg"
firstAndLast("dinosaur"); //=> "dr"
firstAndLast("table"); //=> "te"
```


6.

Create a function called `food()`. The function should take one argument, `time`, which is an argument which can be any number between 1-24. The number represents the hour of the day, where `1` is 1am and `24` is midnight.

* If the time is later than 5 but earlier than 9, print "Breakfast is ready!"
* If the time is later than 11 but earlier than 14, print "Lunch is ready!"
* If the time is later than 18 but earlier than 22, print "Dinner is ready!"
* Else, print "Wait until next meal."

Test your code with the following invocations:

```JavaScript
food(3); // "Wait until next meal."
food(6); // "Breakfast is ready!"
food(10); // "Wait until next meal."
food(13); // "Lunch is ready!"
food(16); // "Wait until next meal."
food(19); // "Dinner is ready!"
```
