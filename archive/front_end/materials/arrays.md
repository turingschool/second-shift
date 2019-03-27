# Arrays

Arrays hold collections of elements. Arrays can hold different type of values, and can hold either zero values, or as many values as can fit in memory.

## Contents

* Syntax
* Accessing values
* Common array operations
* Exercises

---

## Syntax

Arrays are denoted with bracket notation and are usually created just by writing `[]`. You can also use the prototype object `Array`.

```JavaScript
var array1 = [];
var array2 = Array();

array1 // []
array2 // []
```

Arrays can hold one or many elements.

```JavaScript
var array1 = ["Hello", "What's", "up"];

array1 // ["Hello", "What's", "up"]
```

They don't have to be of the same type.

```JavaScript
var array1 = ["Hello", 12345, function() { return "hey" } ];

array1 // ["Hello", 12345, [Function] ]
```

---

## Accessing values

Arrays are 0 indexed and values can be accessed using bracket notation.

```JavaScript
var array = ["Hello", 12345, true];

array[0] // "Hello"
array[2] // true
array[3] // undefined
```

If you don't know what index an element has, you can use the `indexOf` function.

```
var array = ["Hello", 12345, true];
var index = array.indexOf(12345);

index // 1
array[index] // 12345
```

---

## Common array operations

Read more about available array methods on [mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array). The methods are listed in the column to the left.

### Getting the length of the array

[Array.length](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/length)

```JavaScript
var array = ["h", "e", "l", "l", "o"];

array.length // 5
```

### Adding to the end of an array

[Array.prototype.push](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)

```JavaScript
var array = [1, 2, 3];

array.push(4);
array // [1, 2, 3, 4]
```

### Adding to the beginning of the array

[Array.prototype.unshift](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift)

```JavaScript
var array = [1, 2, 3];

array.unshift(4);
array // [4, 1, 2, 3]

array.unshift(6, 5);
array // [6, 5, 4, 1, 2, 3]

```

### Removing elements from the end an array

[Array.prototype.pop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)

```JavaScript
var array = [1, 2, 3];

array.pop();
array // [1, 2]
```

### Removing elements from the beginning of the array

[Array.prototype.pop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)

```JavaScript
var array = [1, 2, 3];

array.shift();
array // [2, 3]
```

### Ask if an array includes an element

[Array.prototype.includes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)

```JavaScript
var array = [1, 2, 3];

array.includes(2); // true
array.includes(0); // false
```

### Retrieve a portion of the array

[Array.prototype.slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) returns a shallow copy of the array. It takes two arguments - the first is the starting index and the second optional one is the end index.

```JavaScript
var array = [1, 2, 3, 4, 5];

var slice = array.slice(3);

array // [1, 2, 3, 4, 5]
slice // [4, 5]
```

### Remove a portion of the array

[Array.prototype.splice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) works similar to `slice` but actually removes the items.

```JavaScript
var array = [1, 2, 3, 4, 5];

var slice = array.splice(3);

array // [1, 2, 3]
slice // [4, 5]
```

---

## Exercises

1. 

Create an array and add some elements to it. Use the `.length` function to see how many elements are in the array.

2.

Create an empty array. Create a function called `add` which adds only string elements to that array. For example, if the function is invoked with `add(1)`, the value of the array should still be `[]`.

Expected results:

```JavaScript
var array = [];

add(4);
array // []

add("table");
array // ["table"]

add("yo");
array // ["table", "yo"]

add([]);
array // ["table", "yo"]
```

3.

Create an empty array. Create a function which adds the arguments passed to it to the array. However, the array can only hold three arguments, so when the fourth argument is added to the end of the array, the first element must be removed.

Expected results:

```
var array = [];

addToArray(1);
addToArray(2);
addToArray(3);

array // [1, 2, 3]

addToArray(4);
array // [2, 3, 4]

addToArray(5);
array // [3, 4, 5]
```

4.

Create an empty array. Create a function which takes two arguments, `item` and a string `direction` which is either `"front"` or `"back"`. If `direction` is equal to `"front"`, add the element to the beginning of the array. If the `direction` is `"back"`, add the element to the end of the array. If the `direction` argument is omitted, it adds it to the end of the array.

Expected results:

```JavaScript
var array = [];

addToFrontOrBack(1);
addToFrontOrBack(2, "front");

array // [2, 1];

addToFrontOrBack(2, "back");
array // [2, 1, 2];

addToFrontOrBack("front");
array // [2, 1, 2, "front"];
```

5.

Let's say that you have two bags, one for work and one for personal stuff. What should be in each array you keep track of using arrays.

```JavaScript
var personal = ["running shoes", "cinema coupon", "sun glasses", "bus pass"];
var work = ["laptop", "reading glasses", "pen", "TPS reports"];
```

Create a function which takes in one a string, `item`. If the `item` should go in your personal bag, print `"Personal!"` to the scren. If the `item` should go in your work bag, print `"Work!"` to the screen. If the item is not present in either of the arrays, print `"Unidentified object"`.

6.

Let's build a very simple queue. In a queue, items are added to the end and removed from the front. Start with an empty array and a function called `queue` which takes an argument `item`. When `queue` is called, it will add the `item` to the end of the empty array.

Create another function called `takeFromQueue`. When invoked, it will remove the first element of the array.

Expected results:

```JavaScript
var array = [];

queue(1);
queue(2);
queue(3);

array // [1, 2, 3]

takeFromQueue();

array // [2, 3]
```

Let `takeFromQueue` take an optional argument `size` which will be an integer specifying how many elements to remove from the front.

```JavaScript
array = [1, 2, 3, 4, 5, 6];

takeFromQueue(2);

array // [3, 4, 5, 6]

takeFromQueue(3);

array // [6]
```