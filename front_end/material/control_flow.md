# Control flow

`If/else/` statements are used to control the flow of your code - if you need to do, or not do, something based on a certain condition, you can use `if/else` statements. For example, let's say that you only go to bed after 10pm. In code, you could write something that determines `If it's after 10pm, I will go to bed`. Or, you could write something like `If I oversleep, I will buy coffee`.

* if statement
* if/else statement
* if/else if/else statement
* exercises

## If statement

The if statement starts with the `if` keyword, a condition in parenthesis, and curly braces which encapsule the body.

If the `_condition_` returns a `true` or truthy value, the `_code_` in the body within the curly braces will execute.

```
if (_condition_) {
  _code_
}
```

## If/else statement

A single `if` statement is useful when you only want to execute some code under a certain condition. But what if you want to be able to do something else if the condition is not true? Do not despair, we can add an `else` clause.

With this, we can write things like `If I oversleep, I will buy coffee. If not, I will go to the gym`.

Here, we have added the `else` keyword on the same line as the closing brace for the `if` clause. The `else` is followed by an opening brace and a closing brace. In between the braces we put the code to run.

We will run `_some_other_code_` if the `_condition_` in the `if` clause returns false.

```
if (_condition_) {
  _code_
} else {
  _some_other_code_
}
```

## If/else if/else

But what if we want to say things like `If it's after 10pm, I will go to bed. If it's after 2pm and before 4pm, I will go to bed. Otherwise, I will stay awake`.

In the previous statement, we have two conditions (`it's after 10pm`, `after 2pm and before 4pm`) and one statement which represent all other outcomes (`Otherwise`).

```
if (_condition_) {
  _code_
} else if (_other_condition_) {
  _other_code_
} else {
  _some_other_code_
}
```

You can add as many `else if` clauses as you want, but you can only have one single `if` clause and one single `else` clause.

```
if (_condition_) {
  _code_
} else if (_other_condition_) {
  _other_code_
} else if (_other_condition_) {
  _other_code_
} else if (_other_condition_) {
  _other_code_
  .
  .
  .
} else {
  _some_other_code_
}
```

## Exercises

1. 
Start with the following code:

```
if () {

}
```

Add a condition which you know will return true, a boolean value is a good start. Then, add `console.log("Hello, World!")` in the body of the `if` statement.

Run the code. Do you get `Hello, World`?

2.
Create an `if` statement which will return the string `5 squared is equal to 25`, given that there is a variable `x` assigned to `5`.

Change the value of `x` to other integers and make sure your output changes according to `x`.

3.
Create an `if` statement which will return the string `100 is an even number`, given that there is a variable `y` assigned to `100`.

If `y` is not an even number (for example `101`), print the string `101 is not an even number` to the screen.

Change the value of `y` to other integers and make sure your output changes.

4.
It's raining outside and numbers are determining the sounds the drops make when they fall.

* If the number is divisible by 3, print "Drip"
* If the number is divisible by 5, print "Drop"
* If the number is divisible by both 3 and 5, print "DripDrop"

5.
Write code to determine what to wear given the temperature. 

* If the temperature is below 45 degrees F, print `Wear a jacket!` to the screen. 
* If the temperature is over 45 degrees but below 65 degrees F, print `Bring a sweater!` to the screen.
* If the temperature is over 65 degrees, print `Bring sunscreen!` to the screen.