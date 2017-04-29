# Front End Second Shift 4

* Warmup and review from last class
* JavaScript 3: Objects and Arrays
* JavaScript 4: Iterating through collections

---

## Warmup

LSAT problem.

---

## JavaScript 3: Objects and Arrays

So far we've learned about strings, integers, operators, conditionals, and functions. In this class we are going to look at two more data structures: objects and arrays. Both of them are types of collections, but the use cases for them are slightly different.

The curly braces denote the limits of an object, and when we ask JavaScript what type an object is, we get `'object'`.

```JavaScript
> {}
{}
> typeof {}
'object'
```

An object holds data in key value pairs. Both the key and the value are pieces of information that you decide yourself, so an object is very powerful in that regard. For example, let's say that we want to keep track of countries' population. A number like `10,456,789` might make much sense by itself, but if we are careful about how we construct the data, we can get more information about the number.

```JavaScript
> var population = { Whoville: 10234567 }
undefined
> population
{ Whoville: 10234567 }
> population.Whoville
10234567
```

The keys of an object are usually strings or integers.

```JavaScript
> var object = {}
undefined
> var object = { "HELLO": 2, 4: "key", newKeyOverHere: { d: 4 } }
undefined
```

If you know the keys of an object, you can access their values with either so-called dot notation or bracket notation.

```JavaScript
> object = { turing: "school" }
{ turing: 'school' }
> object.turing
'school'
> object['turing']
'school'
```

Given a string key, what's the difference between the dot notation and bracket notation in terms?

```JavaScript
> object.'turing'
object.'turing'
       ^^^^^^^^
SyntaxError: Unexpected string

> object[turing]
ReferenceError: turing is not defined
    at repl:1:8
```

It's fairly common that you want to add more information to your object. Similarly to accessing values, you can add key/value pairs using either bracket notation or dot notation.

```JavaScript
> b = {}
{}
> b.new = 2
2
> b['alsoNew'] = 3
3
> b
{ new: 2, alsoNew: 3 }
```

### Check-ins

* Create an object and ask it for it's type:  `typeof`
* Create an object with a string key and an integer key. The values can be whatever.
* Access the values with both dot notation and bracket notation.
* Create an empty object and add key/value pairs using both array notation and bracket notation.
* Create an object with two of the same keys. What happens?
  * `{ "yo": 1, "yo": 2 }`
* Create an object with two of the same values. What happens?
  * `{ 1: "yo", 2: "yo" }`

---

An object is a type of collection as it can hold many keys, but we also have arrays which works more as a list. Arrays don't have any keys, insted they hold elements in a comma separated list.

Note that JavaScript will tell us that `[]` is a `typeof` object. That's because arrays inherit, or stem from, objects.

```JavaScript
> a = []
[]
> typeof a
'object'
```

Let's create our first array! We can start easy and create one that holds 5 integers. 

```JavaScript
> var array = [1,2,3,4,5]
undefined
```

Arrays are 0-indexed and every element in the array occupies one position, one index, in the array. We can ask the array for elements at various positions in the array.

As you can see, the element at index `0` is the first number (1), and the last element has index `4`. If we ask for the element at index 5 - which doesn't exist currently - you get `undefined` back. 

```JavaScript
> array[0]
1
> array[1]
2
> array[4]
5
> array[5]
undefined
```

If you want to find out how many elements there are in an array, you can use the `length` method.

```JavaScript
> array.length
5
```

Let's say you don't know what the last element in the array is but you want to access it, you can easily access the last element with some simple arithmetic.

```JavaScript
> array[array.length -1]
5
```

The size of an array is dynamic and it grows or shrinks depending on what you decide to do with it. JavaScript provides a nice method to add more elements at the end of the array called `push`.

Note the difference between the method `length` and the method `push`. Length does not need parentheses (`()`) when called whereas we need to invoke the `push` method with parentheses. The `length` is an object attribute, and `push` is a prototype function. We are going to go more into detail on the difference later in the course.

```JavaScript
> array.push()
5
> array.push(6)
6
> array
[ 1, 2, 3, 4, 5, 6 ]
```

Sometimes we also want to remove elements from the array.

```JavaScript
> array.pop()
6
> array
[ 1, 2, 3, 4, 5 ]
```

Arrays can contain any data types, data structures, or expressions - not just integers. It could be strings, objects, other arrays, or functions.

```JavaScript
var array = [1, 2, ["hello"], ["you!", { turing: 1, school: 1}] [ ["what's"], ["up"] ], { a: 4, b: 5 }, function hey() { console.log("hey, friend") }];
```

### Check-ins

* Create an array containing a few different elements.
* Add the string "Christmas" to the end of the array.
* Add a few integers to the end of the array.
* Use the `pop` function to remove some of the values.
* Using bracket notation, what's the last element of the array?
* What's the difference in return value between `push` and `pop`?

Given the array: 

* `var array = [1, 2, ["hello"], ["you!", { turing: 1, school: 1}] [ ["what's"], ["up"] ], { a: 4, b: 5 }, function hey() { console.log("hey, friend") }];`, 

try to access each of the elements in the array so it prints by itself.

For example, accessing the three first elements would go like this:

```JavaScript
> array[0]
1
> array[1]
2
> array[2]
[ 'hello' ]
> array[2][0]
'hello'
```

---

## JavaScript 4: Iterating through collections

Now that we have enough knowledge about collections, we can start looking at how to iterate through collections. Iterating through collections is an extremely common operation in programming.

One way of iterating through a collection is using a "for loop".

If we don't do anything in the body of the for loop, we get back `undefined`. But if we log the variable we can kind of see what's going on.

```JavaScript
> for (var i = 0; i < 10; i++) {
... }
undefined
> for (var i = 0; i < 10; i++) {
... console.log(i);
... }
0
1
2
3
4
5
6
7
8
9
undefined
```

[Lesson plan](http://frontend.turing.io/lessons/module-1/js-2.html);

### Check-ins

* Create a for loop that iterates 10 times over a variable `i`, printing out the value of `i` each iteration.
* Given the array `furniture = ["sofa", "kitchen table", "book shelf"];`, create a for loop that prints out that you're selling each of your furniture pieces.
  * The furniture array could also keep the prices of the furniture; `furniture = ["sofa", "kitchen table", "book shelf"];`. Update the for loop to also print out what you are selling each piece for.
* Create a for loop that prints out all the even numbers from 1 up to a variable `var limit = 10;`.
  * Wrap the for loop in a function that takes `limit` as a variable. Try tro print all even numbers up to 100, 1000, 10000, 1000000... 
  * have the function take another argument - `lowerLimit` - which sets the lower limit for the range. Bonus if you prevent (or help correct) your users from creating invalid ranges!
* We know how to iterate over arrays, but how do we iterate over objects? `[Object](mdn.io/object)`

---

![](https://media.giphy.com/media/3o6YgjB8zBvvERC6v6/giphy.gif)
