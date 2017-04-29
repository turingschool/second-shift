# Front End Second Shift 2

* Warmup and review from last week
* JavaScript 2: Conditionals and Functions

---

## Warmup

LSAT problem.

## Review

* String
* Number
* Arithmetic operators
* Logical operators

---

## JavaScript 2: Conditionals and Functions

## Conditionals

So far our code has always executed in the same way, which doesn't make it very useful. One aspect of writing programs is to have it execute differently based on the  data we feed into the program.

One way of creating distinct code paths is to use control flow, or conditional statements. `if/else` statements are on of the more common ways of expressing control flow. 

Let's look at the anatomy of an `if/else` statement. In English, this would read "If the condition is true, do this - if not, do something else".

```JavaScript
if (condition) {
  statement;
} else {
  statement;
}
```

Based on what we just learned, what's going on below?

```JavaScript
condition = true;
x = 0;

if (condition) {
  x = x + 5;
} else {
  x = x + 10;
}

// What's the value of x? 
```

If you want to be able to handle multiple conditions, something that would read like "If this is true, do this - if not and this other condition is true, do this - if both of the previous conditions are false, do this".

```JavaScript
if (condition) {
  statement;
} else if (condition) {
  statement;
} else {
  statement;
}
```

Based on what we just learned, what's going on below?

```JavaScript
max = 10
x = 0;

if (max === 0) {
  x;
} else if (max % 2 === 0) {
  x = x + 2
} else {
  x = x + 1
}

// What's the value of x? 
```

### ATM Exercise

Let's practice this together by writing a program that emulates an interaction you might have with your friendly neighborhood ATM.

The features of our ATM are:

* Be able to make withdraw money
  * We can only withdraw the amount requested if we have enough money on our account
* Be able to deposit money and checks
* Be able to display the account balance

### Exercises

* Create an `if/else` statement which tells you if a student should get an `A`, `B`, `C`, or `D` based on their score.
  * A: 100 - 95
  * B: 94 - 85
  * C: 84 - 75
  * D: 74 or below
* Create an `if/else` statement which tells you if you should eat breakfast outside or inside based on the temperature.
  * If the `temperature` is equal to or more than 70 degrees, you eat outside
  * If the `temperature` is below 70 degrees, you eat inside

### Goals

* Understand how an `if/else if/else` statement works

## Functions

Functions are integral to writing good JavaScript and have a few special characteristis.

A function is a collection of instructions that execute together. For example, brushing your teeth might be a function since it comprises multiple actions; get your tooth brush, put tooth paste on your tooth brush, brush your teeth, rinse your tooth brush. 

Let's look at the anatomy of a JavaScript function. This function has a `function` keyword and it has a body (anything between the curly braces). Since this function does not have a name, it's referred to as an `anonymous function`. 

```JavaScript
function () { }
```

However, if we want to be able to use and talk about the function we create, we should give it a name. We can reference the function below using its name, `myFunctionName`.

```JavaScript
function myFunctionName () { }
```

### Check-in

* What is a function? Give an example of a daily task which could be abstracted into a function.
* In your console, create both an anonymous function and a named function.

--- 

Functions always return `undefined` by default if they aren't explicitly returning anything. Below, since our function doesn't have anything in its body, it returns undefined when they are invoked. Invoking a function is also called "calling a function" and it actually executes the instructions in the body.

Here, we first create a function, an operation which returns `undefined`. Then, we call it similar to how we would get the value of a variable we've created. In this case, it returns a `Function`. It's a so-called **function expression**. Then on the last line, we are calling the function - a so called **function invokation** - and we get its return value back. 

```JavaScript
> function myFunctionName() {}
undefined
> myFunctionName
[Function: myFunctionName]
> myFunctionName();
undefined
```

However, if we use the `return` keyword to return something in the function body, things immediately become more interesting.

```JavaScript
> function myFunctionName() {
... return "You did it!"
... }
undefined
> myFunctionName
[Function: myFunctionName]
> myFunctionName();
'You did it!'
```

Functions will exit out of the function when it gets to a `return` keyword. Any code defined after a `return` statement is classified as "unreachable".

```JavaScript
function myFunction() {
  return "This is the last you'll see of me";

  // unreachable code! :(
  var x = 4;
  x = x**x;
}
```

### Check-in

* Create a function called `myFunctionName`. What does `myFunctionName` return? What does `myFunctionName()` return?
* What does a function return by default? 
* Create a function which returns "You are the world's greatest" when invoked.

---

Functions get more flexible if they can use outside data. We could example wrap one of our `if/else` statements above and pass the values necessary as arguments.

```JavaScript
function greeting(name) {
  return "Hello, " + name + "!";
}

greeting("Dave");

// What will "greeting("Dave");" output?
// What will "greeting();" output?
```

The `Function Signature` defined what kinds of inputs the function take. JavaScript functions can take any number of arguments, and it's not necessary to pass arguments even though the function takes one or more of them.

```JavaScript
function greeting(name, age, homeTown) {
  return "Hello, " + name + " who is " + age + " years old and live in " + homeTown + "!";
}
```

Based on the function defined above, what will the following calls output?

* `greeting();`
* `greeting("Sara")`
* `greeting("Sara", "Minneapolis");`
* `greeting("Sara", 30, "Minneapolis");`
* `greeting("Minneapolis", "Sara", 30);`

Functions are first class citizens in JavaScript, which means that we can assign them to values and pass them around.

```JavaScript
var greeting = function(name) { return "Hello, " + name + "!"; };
```

Based on the expression defined above, what will the following calls output?

* `greeting`
* `greeting()`
* `greeting("Nick")`

### Check-in

* What part is the function signature of the following function: `function calculateChanges(oldValue, newValue) { };`.

----

### Exercises

* Answer the following questions with the person sitting next to you:
  * What does a function return by default?
  * What is the `return` keyword? What does it do?
  * Do you always have to give a function a name?
  * What is the difference between this: `myFunctionName` and `myFunctionName()`?
  * What is the function signature?
* Create a function which takes the arguments `name` and `jobTitle`. The function should return a string which is something like "Sam works as a Support Analyst".
* Create a function called `vacationPlanner`
  * it takes three arguments: `destination` (String), `budget` (Number), `totalCost` (Number)
  * if the `totalCost` exceeds the `budget`, the function should return: "Sorry, but you need X$ more to go to Y." where `X` is the amount you are short, and `Y` is the destination you wanted to go to.
  * if the `budget` is equal to or more than the `totalCost`, return: "Pack your bags! You are going to Y with an extra spending budget of X$!" where `Y` is the destination, and `X` is how much you're `budget` exceeds the `totalCost`
  * if passed no arguments, it returns "Are you sure you just want to stay home?"


![](https://media.giphy.com/media/8TqWUx2QusWre/giphy.gif)
