# Test drive a Tyrannosaurus Rex

In this section, we are going to build a Tyrannosaurus Rex using test driven development. We are going to write one test, and then write code to make the test pass.

## Iteration 1 - setup

If you haven't done so already, clone the [javascript-foundations](git@github.com:turingschool-examples/javascript-foundations.git) repo.

`cd` into the `mythical-creatures` directory. In the `test` directory, create a new file called `dinosaur-test.js`. In the `exercises` directory, create a new file called `dinosaur.js`

At the top of the `dinosaur-test.js` file, add the lines below. The first line requires the `assert` object which we are going to use to make test assertions, and the second line is requiring the `Dinosaur` object from the `exercises/dinosaur.js` file (which we are yet to write).

```JavaScript
var assert = require('chai').assert;
var Dinosaur = require('../exercises/dinosaur');
```

In the console, run the command `mocha test/dinosaur-test.js`. Mocha should try to run the `dinosaur-test.js` file and you should get some errors in the console probably related to `Dinosaur` not being found.

## Iteration 2 - first test

It's time to write our first test. Let's start with a top-level `describe` block. `describe` and `it` blocks are used to organize tests and if you name them well, your test suite can read very well and sort of tell a story about the functionality of the thing being tested.

```JavaScript
describe("A dinosaur", function() {

});
```

Now that we have a top-level describe block, we can add our first `it` block. In this test, we are just asserting that when we instantiate a new `Dinosaur` its constructor is `Dinosaur`.

```JavaScript
describe("A dinosaur", function() {

  it("is a type of Dinosaur", function() {
    var dinosaur = new Dinosaur();
    assert.equal(dinosaur.constructor, Dinosaur);
  });

});
```

## Iteration 3 - more tests

Now let's add some more tests. Remember to write one test before continuing to the next one. Below I have listed the test cases. Try first to write the test yourself. If not, a suggested implementation for each test can be found after the numbered list.

1. The dinosaur function takes a `numberOfLegs` argument when instantiated. Assert that the value of `numberOfLegs` passed in, is the same as `dinosaur.numberOfLegs`.
2. The dinosaur function takes an argument called `diet` which can be set to `"carnivore"`, `"omnivore"`, or `"herbivore"`.
3. The dinosaur starts out as hungry. `dinosaur.hungry` should return true.
4. The dinosaur starts out as thirsty. `dinosaur.thirsty` should return true.
5. The dinosaur can tell you if it needs to eat or drink, using a prototype function called `status`. If the dinosaur is thirsty (aka if that value is true), the function should return "I am thirsty!". If the dinosaur is for example both thirsty and hungry`, the function should return "I am hungry! I am thirsty!". So when we call this function right after the function invokes it should return "I am hungry! I am thirsty!".
6. The dinosaur can eat. When calling the `eat` prototype function, the string "yuuuuum" should be returned.
7. The dinosaur can drink. When calling the `drink` prototype function, the string "sluuuuuurp" should be returned.
8. After eating three times, the dinosaur is not hungry anymore.
9. After drinking three times, the dinosaur is not thirsty anymore.


### Suggested test implementations

1.

```
it('is assigned a number of legs', function() {
  var dino = new Dinosaur(4);
  assert.equal(dino.numberOfLegs, 4);
});
```

2.

```
it('is assigned a diet', function() {
  var dino = new Dinosaur(4, "carnivore");
  assert.equal(dino.diet, "carnivore");
});
```

3.

```
it('starts out as hungry', function() {
  var dino = new Dinosaur(4, "carnivore");
  assert.equal(dino.hungry, true);
});
```

4.

```
it('starts out as thirsty', function() {
  var dino = new Dinosaur(4, "carnivore");
  assert.equal(dino.thirsty, true);
});
```

5.

```
it('can tell you it'\s status', function() {
  var dino = new Dinosaur(4, "carnivore");
  assert.equal(dino.status(), "I am hungry! I am thirsty!");
});
```

6.

```
it('can eat', function() {
  var dino = new Dinosaur(4, "carnivore");
  assert.equal(dino.eat(), "yuuuuuum");
});
```

7.

```
it('can drink', function() {
  var dino = new Dinosaur(4, "carnivore");
  assert.equal(dino.drink(), "sluuuuuurp");
});
```

8.

```
it('is not hungry after eating three times', function() {
  var dino = new Dinosaur(4, "carnivore");
  assert.equal(dino.status(), "I am hungry! I am thirsty!");

  dino.eat();
  dino.eat();
  dino.eat();

  assert.equal(dino.status(), "I am thirsty!");
});
```

9.

```
it('is not thirsty after drinking three times', function() {
  var dino = new Dinosaur(4, "carnivore");
  assert.equal(dino.status(), "I am hungry! I am thirsty!");

  dino.drink();
  dino.drink();
  dino.drink();

  assert.equal(dino.status(), "I am hungry!");
});

it('is not thirsty after drinking three times', function() {
  var dino = new Dinosaur(4, "carnivore");
  assert.equal(dino.status(), "I am hungry! I am thirsty!");

  dino.drink();
  dino.drink();
  dino.drink();

  dino.eat();
  dino.eat();
  dino.eat();

  assert.equal(dino.status(), "I am very happy.");
});
```

---

## Your turn

Test drive either your own dinosaur or some other animal - it can be completely made up. Before you start, write down a list of potential test cases that you can use.