# Objects

Objects are SUPER important in JavaScript. Everything is an object in JavaScript - from data types and data structures to elements in the browser such as the `document`. 

In your browser console,

* execute the command `typeof document`. What do you get?
* execute the command `typeof [1, 2, 3]`. What do you get?

## Contents

* Syntax and usage
* Exercises

---

## Syntax and usage

An object is a collection of key-value pairs. It's a JavaScript data type but also works as a collection, in the sense that it can hold many key-value pairs. Keys have to be unique, but the values do not have to be unique.

Let's create our first object. We are going to create a `person` object, and hopefully store information about the `person` in the object.

```JavaScript
var person = {}
```

Still in your web console,

* what is the outcome of `typeof person`?
* what is the outcome of `console.log(person)`?

---

Let's see about adding our first key to the `person` object.

There are two simple ways to add keys to a JavaScript object. Either we can use `bracket` notation, or `dot` notation.

```JavaScript
var person = {}

person.name = "Kelly"

person['age'] = 12

console.log(person) //=> { name: "Kelly", age: 12 }
```

In your web console,

* add a key/value pair to your object using dot notation
* add a key/value pair to your object using bracket notation
* add a key (not a value!) to your object using dot notation
* add a key (not a value!) to your object using bracket notation

---

Just as we can add keys and values to our objects using bracket and dot notation, we can also _access_ the values of the keys using bracket/dot notation. Since we added a few keys to our object in the previous section, let's see about accessing those.

```JavaScript
console.log(person); //=> { name: "Kelly", age: 12, hobbies: undefined, favoriteMovie: undefined }

person.name //=> "Kelly"
person.hobbies //=> undefined

person['age'] //=> 12
person['favoriteMovie'] //=> undefined
```

* in your console, access the values of the keys by using either bracket or dot notation.
* what is the difference between bracket and dot notation? Why?

---

Let's add a value to a key `hobbies`. `hobbies` is plural, indicating that the value might be a collection. Let's set the value to an empty array.

```JavaScript
person.hobbies = []
```

Let's add something to the person's hobbies. This person likes `"bob sleighing"`. Since the value is an array, we can use an array method to add elements to it.

```JavaScript
person.hobbies.push("bob sleighing");

person.hobbies //=> ["bob sleighing"]
```

* Add three hobbies to the `hobbies` key.
* Access each of the elements in the `hobbies` array by their index and bracket notation
* Write a for loop which iterates over the `hobbies` array and prints each element to the screen

---

Our objects can have any values - as long as they are valid JavaScript elements. Let's see about adding a function as a key to our `person` object.

```JavaScript
person.hello = function() { console.log("Hello, my name is " + this.name) }
```

* what does `person.hello` return? What does `person.hello()` return?

Let's make that function a bit more interactive. We can pass it another string, a `name` and have our person greet someone before saying their name.

```
person.hello = function(name) { console.log("Hello, nice to meet you", name, "! My name is", this.name, ".") }
```

* invoke the function, passing it an argument, so we get a complete greeting printed to the screen.

---

Objects can have nested keys. Take a look at the example below.

```
var countries = {
  Sweden: {
    population: 10000000,
    language: "Swedish"
  },
  UnitedStates: {
    population: 324000000,
    language: "English"
  },
  Norway: {
    population: 5000000,
    language: "Norwegian"
  }
}
```

* Given the object above, how would you access the `population` and `language` values for the countries listed?


Objects are used for A LOT of different things when programming in JavaScript, and they can easily become large and complex to navigate. If you have an idea of what data you want to store in your object from the get-go, you can save yourself future pain by setting your object up in a comprehensive way.

For example, our `person` object could easily bloat to a large object with just top-level keys. We can categorize the data we are adding to the `person` and group those keys under other keys.

Here, we have grouped the keys under the top-level keys `bio`, `actions`, and `personality`.

```JavaScript
var person = {
  bio: {
    name: "Kelly",
    age: 12,
    gender: "F"
  },
  actions: {
    hello: function(name) { console.log("Hello,", name, "! My name is", this.name, ".") }
  }
  personality: {
    hobbies: ["bob sleighing", "knitting", "hanging out"]
  }
}
```

* write a function which returns a new object like the one above - set all keys to `undefined` for now. 
* write the `hello` function (it can do whatever you want as long as it takes one argument and uses the `name` key in the object)
* modify the function to take some arguments: `name`, `age`, and `gender`. Create two new `person` objects with different values for `name`, `age`, and `gender`.
* verify that your two different person objects are returning different strings for the `hello` function.

---

## Exercises

### 1.

Create a `cat` object. The `cat` object should have three keys, `type`, `name` and `numberOfLegs`. `type` should be `cat`. Add the `cat` to an array called `animals`.

### 2.

Add two more objects to the `anmials` array. It should be the same as `cat` in shape (e.g., same keys and same types of values). Iterate over the `animals` array and print out all objects of `type = "cat"`.

### 3.

Create an empty array outside of the for loop called `cats`. In the for loop, add the object to the `cats` array if it is of `type = "cat"`. Note that this should not remove elements from the `animals` array!

### 4.

Create an empty object called `animalTypes`. We are going to populate this object with keys representing animal types, (for example `cat`, `spider`, or `snake`), and values representing how many objects of that `type` there are in the `animals` array. For example. if there is one `snake` and two `spider`s in the array, the `animalTypes` object should look like this: `{ spider: 2, snake: 1 }`. ]

In another for loop, iterate over the `animals` array and add keys to the `animalTypes` object. Set the values to `0`. We set it to 0 because that is our starting value for counting.

### 5.

In another for loop, iterate over the `animals` array and update the counts in the `animalTypes` object. Make sure that the object has the correct counts!

### 6.

Create another empty object called `numberOfLegs`. We are going to iterate over the `animals` object and update the `numberOfLegs` object as go. In the previous exercise, we updated the keys first, and then updated the values. We don't know what the numbers are before we iterate over the array, since it's not guaranteed that we are going to have animals with 0, 2, 4, 6 or 8 legs.

Keys need to be unique in an object, and we have to be careful that we don't override our keys. To check in an object if the key already exists, you can use the method [`Object.prototype.hasOwnProperty()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty). Play around with it in the console before to learn how to use it.

### 7.

We have a few operations happening here that we can extract into functions. Extract the logic of each operation to a function - make sure to pass it all necessary data it needs to operate. Below there are some suggestions on abstractions to make.

* A function which creates `animal` objects if you pass it values for `type`, `name`, and `numberOfLegs`.
* A function which retruns an array with just the `cat` objects from the array.
  * makes this function more dynamic by passing in a `type` argument. If the value `"snake"` is passed in, only return objects of `type = "snake"`.
* A function which returns an object representing the counts of each animal `type`.
* A function which returns an object representing the counts of the animals' `numberOfLegs`.

When you finish, try to have as much code as possible extracted into functions. Also remove and function invocations that you have. If you limit what you print to the console, it will be easier to follow.

### 8.

Now, take all your function names and add them to an object called `animalInformation`. The keys should be the function names, and the values should be the function _expressions_. When you print out the object `animalInformation.someObjectKey`, the function should not execute.

Write a for loop which iterates over the object and invokes each of the functions.