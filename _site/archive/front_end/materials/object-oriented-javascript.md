# Object oriented JavaScript

Object oriented programming is a code pattern in which all entities of a program are represented by objects. All the logic that relates to that object is kept in the object. Code encapsulation is important so that the methods available to any object are just the ones that matters to it.

* Principlies of object oriented programming
* Object oriented JavaScript
* Exercises

---

## Principles of object oriented programming

There is a lot to object oriented programming, below are some of the more important ones to know when starting out.

### Single Responsibility

An object should have a single responsibility. For example, if we have `Car` and `Motorcycle` objects, they should only keep data that relates to them. A `Car` should not know anything about the `Motorcycle`, and vice versa.

### Open-Closed Principle

This means that objects cannot be _modified_, but the can be _extended_. For example, the `Car` and the `Motorcycle` object might have a lot in common. Both of them have an engine, break, some number of wheels, and some sort of steering instrument. That shared logic could be held in a `Vehicle` object, which both the `Car` and `Motorcycle` extends. They cannot change any of the `Vehicle`'s implementaion, only add to it.

---

## Object oriented JavaScript


### Creating a constructor function

Constructor functions are "blueprints" for new objects of that type. We pass data to differentiate between the different instances of that object, but all objects of the same type share a same set of properties and behavior. For example, we can have many instances of the `Animal` object, but they have slightly different properties - like a `dog`, `cat`, `parrot`, or `snake`.

The convention is to have function names with capital letter when creating constructor functions. We can ask the `cat` object we just instantiated what its constructor is, and it will say it is of type `Animal`. We also say that the `cat` object's _prototype_ is `Animal`.

The `Animal` constructor takes two arguments which we tie to the object by doing `this.type = type`. In JavaScript, `this` represents the context at any given moment. The context can be set in a few different ways, but usually it is set to the object which called it. So, for example, inside the objects that are of type `Animal`, the `this` context will be equal to those objects. Look at the return values for `{dog,cat}.whoAmI()`.


```JavaScript
function Animal(type, name) {
  this.type = type
  this.name = name
}

var cat = new Animal("cat", "kelly")
cat.constructor //=> [Function: Animal]
cat.name //=> "kelly"
```

### Extending an object using prototype functions

All entities in JavaScript have prototypes, objects from which they inherit prototype functions that are available on them. When we create a new array and ask it for its `constructor` we get `Array`.

```JavaScript
var array = [1,2,3]
array.constructor //=> [Function: Array]
```

We can extend the prototype and add functions or properties that will become avilable for all objects with the same property.

```JavaScript
var array = [1,2,3]
array.constructor //=> [Function: Array]

array.lol //=> undefined

Array.prototype.lol = "lol what's up"
array.lol //=> "lol what's up"

var newArray = ["hey"]
newArray.lol //=> "lol what's up"
```

### Object context: `this`

All functions have a context, and it is usually set by the object that called the function. Here, we have added a prototype function to the `Animal` object called `whoAmI`. When we call it, we will log the current value of `this`.

```JavaScript
function Animal(type, name) {
  this.type = type
  this.name = name
}

Animal.prototype.whoAmI = function() {
  console.log(this)
}

cat.whoAmI() //=> Animal { type: 'cat', name: 'kelly', whoAmI: [Function] }
```

We get a representation of the object that called the `whoAmI` function, holding all available properties on it. All functions stem from `Function`, so there are in fact [a few more available methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function).

Most of the time, the value of `this` is not an issue, but if you're getting the error `undefined is not a function` and you are mostly sure that it is a function, log the value of `this` to make sure that the current execution context is what you think it is.

### Full Animal example

```JavaScript
function Animal(type, name) {
  this.type = type
  this.name = name
  this.whoAmI = function() {
      console.log (this)
  }
}

var cat = new Animal("cat", "kelly")

cat.type //=> "cat"
cat.type //=> "kelly"
cat.constructor //=> [Function: Animal]
cat.whoAmI() //=> Animal { type: 'cat', name: 'kelly', whoAmI: [Function] }

var dog = new Animal("dog", "burt")

dog.type //=> "dog"
dog.type //=> "burt"
dog.constructor //=> [Function: Animal]
dog.whoAmI() //=> Animal { type: 'dog', name: 'burt', whoAmI: [Function] }
```

### Example: Twitter

For a site like Twitter, there might be one object called `Tweet` which holds all logic related to tweets, one object called `User` which holds logic related to users. The `Tweet` object might have a property called `timeStamp`, `content`, or an array called `replies` which holds other `Tweet` objects. The `User` object might have properties like `name`, `username`, `bio`, or an array called `tweets` which holds multiple `Tweet` objects.

```JavaScript
function Tweet(timeStamp, content) {
  this.timeStamp = timeStamp
  this.content = content
  this.replies = []
}

function User(name, username, bio) {
  this.name = name
  this.username = username
  this.bio = bio
  this.tweets = []
  this.following = []
  this.followers = []
}
```

---

## Exercises

On pen and paper, draw out the different objects that might be in place for Facebook, Yelp, or another web app of your choice.

---

In this section we are going to create a `animalDex` to keep track of animals that we explore and study.

Required features of the `Dex` and `Entry` objects are below.

* It should have a property `entries` which starts out as an empty array.
* It should have a function which allows me to add an animal. When adding an entry in the `animalDex`, you are adding an instance of the `entry` object to the `entries` array. More info on the `Entry` object below.
* It should have a function which allows me to find an animal by `name`.
* It should have a function which allows me to update data in an `entry`.

**Dex**

The `Dex` object is what we use to instantiate the `animalDex`. The `Dex` object takes a `topic` argument - if the topic is Pokemon, the `topic` should be set to `pokemon`. When instantiated, the `Dex` also has the properties `entries` which is set to an empty array.

**Entry**

The `Entry` object is representing entries in a `Dex`. The `Entry` object can have any keys, and only the `name` attribute is required when instantiated. We are going to set up some rules about property names so that the `Entry` objects share some standards and make it easier to write programs around.

All `Entry` objects should have a unique `id`.

### Iterations

#### 1.

Create the `Dex` object.

```JavaScript
var animalDex = new Dex("animals")

animalDex.topic //=> "animals"
animalDex.entries //=> []
```

#### 2.

Create the `Entry` object.

```JavaScript
var testEntry = new Entry("test")

testEntry.name //=> "test"
```

#### 3.

Let's decide that we are going to have some standard fields for the `Entry`. When instantiating a new `Entry`, let's pass it an object which has all the standard properties so that we can ensure that all the `Entry` objects are of the same shape.

Below is an example of what fields to use - use at least these but feel free to add additional ones.

```JavaScript
var defaultData = {
  canBeFoundIn: [],
  climate: [],
  diet: [],
  lifeExpectancyInYears: 0
}

var testEntry = new Entry("name", defaultData)

testEntry.name //=> "name"
testEntry.data //=> {
  canBeFoundIn: [],
  climate: [],
  diet: [],
  lifeExpectancyInYears: 0
}
```

#### 4.

Add a prototype function to the `Dex` called `addEntry`. To the `addEntry` function we need to pass the `"name"` of the new entry. When you create the `Entry`, pass the `"name"` and an object that looks something like `defaultData` in the previous section.

#### 3.

Add a prototype function to `Dex` which allows me to find an animal by `name`.

#### 4.

Add a prototype function to the `Dex` called `addInfoToEntry`. The function takes three arguments: `id`, `field`, `value`. The `id` we use to find the `Entry` to update, the `field` is the field in the `entry` we are going to update, and the `value` is what we are going to set the value to.

You can either populate one of the fields on the given entry, but you can also add new ones.

#### 6.

Add a prototype function which finds the entry associated with the lowest `lifeExpectancy`. If there is one, return the name of the species. If there are 
multiple, return all of them.

#### 7.

Add a prototype function which finds the entry associated with the highest `lifeExpectancy`. If there is one, return the name of the species. If there are 
multiple, return all of them.
