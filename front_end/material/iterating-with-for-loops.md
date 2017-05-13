# Iterating with for loops

When we have a collection, for example an array with strings representing different names, it's common that  we want to go through each element in the array to do something with it. By being able to iterate over a collection and run the same code for every element int he collection, we can reduce duplicate code. 

For example, given an array of names, we want to print `Hello, ...` for each name.

```JavaScript
var names = ["Solange", "Kendrick", "Future", "Abra"];

for every name that is in the names collection {
  return "Hello, " + name + "!";
}

//=> "Hello, Solange!"
//=> "Hello, Kendrick!"
//=> "Hello, Future!"
//=> "Hello, Abra!"
```

* Syntax and example usage: for loop
* Syntax and example usage: for in loop
* Exercises: for loop
* Exercises: for in loop

---

## Syntax and exmaple usage: for loop

Above we have pseudo code, that code won't run.

First, we have the `for` keyword, then a parenthesis where options for the loop go, and then an opening and closing brace. Like `if/else` statements and like functions, code between the curly braces is what will be executed if we drop in to the body.

```JavaScript
for (_options_) {
  _code_
}
```

This is what it would look like with the options added.

* `var i = 0` initializes the counter. This variable we can increase as we iterate to keep track of where we are, and we can create conditions using it.
* `i < 10` is a condition that needs to return true for the loop to do another iteration.
* `i++` changes the value of the counter, in our case `i`.

```JavaScript
for (declaring counter variable; condition; modify counter variable) {
  _code_
}

for (var i = 0; i < 10; i++) {
  _code_
}
```

In the above code, we will iterate until `i < 10`, meaning until `i` is equal to `10`. Let's see if we can experiment with the options to understand them better. Try out the following code snippets in your console. Make small changes to the options and see if you can change the way it runs.

```JavaScript
for (var i = 0; i < 10; i++) {
  console.log(i);
}

for (var i = 0; i <= 10; i++) {
  console.log(i);
}

for (var i = 0; i <= 10; i = i + 2) {
  console.log(i);
}

var letters = ["h", "e", "l", "l", "o"];
for (var i = 0; i < letters.length; i = i + 2) {
  console.log('i:', i);
  console.log('element:', letters[i]);
}
```

Now, we can actually write the first example using actual code. Notice how we have set up the condition. Since we want to iterate as many times as there are elements in the array, we can set the condition up to have the upper bound be the length of the array.

```JavaScript
var names = ["Solange", "Kendrick", "Future", "Abra"];

for (var i = 0; i < names.length; i++) {
  console.log("Hello, " + names[i] + "!");
}

// "Hello, Solange!"
// "Hello, Kendrick!"
// "Hello, Future!"
// "Hello, Abra!"
```

---

## Syntax and exmaple usage: for in loop

For loops work great for arrays, but if we want to iterate over objects, we can use the `for..in` loop.

```JavaScript
var object = { 1: "1", 2: "2", 3: "3" }

for (var key in object) {
  console.log("key:", key, "value", object[key]);
}
```

Objects are key-value pairs, and when we use `for...in` loops, we iterate over the keys. Given the object `var people = { friends: ["bob", "sarah"], colleagues: ["phil", "jaime"], teamMates: ["oscar", "victoria"] }` we can iterate over the object and print out the names of the people in our lives.

```JavaScript
for (var group in people) {

  var result = ''

  result = result + " My " + group + "are "


  for (var i = 0; i < people[group].length; i++) {
    result = result + people[group][i] + ", "
  }

  console.log(result)
}
```

Try to recreate the above loop with different data. For example, you can have an object with keys representing music genres, and values as arrays holding names of songs. Or, you can have an object with keys like `breakfast`, `lunch`, `dinner`, `snack`, and have arrays as values holding names of different foods.

---

## Exercises: for loop

1.

Write a for loop that iterates 15 times and prints out the number of the iteration. For example, for the first iteration it should print: `"Iteration 1"`.

2.

Write a for loop that iterates 10 times and prints out the square of the number being iterated over. For example, if you start at 0, the first string output should be: `"Square of 0 is 0"`, and the second should be: `"Square of 1 is 1"`, `"Square of 2 is 4"` etc...

3.

Write a for loop which iterates over an array of at least 5 strings. For each element, only print out the first letter of each string. For example, if the first element of the array is `"car"`, only `"c"` should be printed.

4.

Write a for loop which iterates over an array of 5 empty arrays: `[[], [], [], [], []]`. For each empty array, add the counter to it. The end result should be an array with five arrays, all with one number in them.

5.

Write a for loop which iterates 100 times and prints the value of the counter variable in each iteration. The expected result is seeing all integers 0-99 printed to your screen.

If the value of your counter variable is divisible by 3, print "one, two, three". If the counter variable is divisible by 5, print "one, two, three, four ,five". Else, print the number. The expected result is 99 values printed to the screen, some should be integers, some should be either string.

6.

Write a for loop which iterates over an array of arrays. Each array hold three strings; `name`, `hobby`, `dream`. For example: `[['Dave', 'Playing soccer', 'Travel the world'], ['Kenny', 'Knitting', 'Become a chef']]`.

Iterate over each array, and print out a string to the screen using all three elements in the sub array.

7.

Write a for loop which prints out all even numbers from 0-99.

Write a for loop which prints out all odd numbers from 0-99.

For both of the above, there are (at least) two different ways of solving it. Try to get both!

8.

Write a for loop which iterates over an array of numbers. For each element, print out the distance to `100`. For example, given the element `60`, the string `"Add 40 to get to 100"`.

Example array: `[45, 67, 92, 72, 10, 86, 99, 16, 86, 39, 68]`.

9.

Create a function which takes in an array of numbers and an `upperBound`. Your job is to recreate the behavior of the previous exercise, iterating over the array that's passed in and use the `upperBound` instead of the hardcoded `100`.

---

## Exercises: for-in loop

1.

Write a for loop that iterates 100 times over an array. Print out the number to the console so you know you're iterating correctly. Populate an object with two keys: `even` and `odd` - the values should be arrays holding either all even numbers or all odd numbers.

Iterate over the object with the odd and even numbers, in the loop, iterate over the value array and sum all the numbers. You can create a new object outside of the `for...in` loop where you can keep the sums of the value arrays.

