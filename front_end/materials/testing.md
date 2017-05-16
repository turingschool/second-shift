# Introduction to testing

To verify that your code is performing the way you expect, you can add tests which exercise your code in a specified way.

## What is testing?

Below is a small example on how you might do it. We have a function called `hello` which takes one argument and then prints out a string using that argument passed in. Nothing special.

However, in the **test** block, we have some new things. We have an `it` block which is a function that takes two arguments - one string which describes in text what the test is doing, and a callback function which holds the code to be executed when running the test.

**implementation**
```JavaScript
function hello(name) {
  return "Hello, " + name + "!"
}
```

We use the `assert.equal` function from the `chai` library. `assert.equal` takes two arguments, and in order for the test to pass, those need to be euqal to each other.

**test**
```JavaScript
it('says hello', function() {
  assert.equal(hello("Lovisa"), "Hello, Lovisa!");
});
```

---

## How do I run tests?

There are many different libraries you can use to test your code, but we are going to use [mocha](https://mochajs.org/) as our test runner, and [chai](http://chaijs.com/) for our assertion library.

To run a test file, you use the command `mocha` and then pass it the path to the test to run. For example, if you want to run the `car-test.js` file in the `test` directory, you run `mocha test/car-test.js`.

## How do I write tests?

When you write a test, you want to name it the same as the file whose code you're exercising. The `car-test.js` file will import code from the `car.js` file. 

You also need to import the `assert` object from chai.

```JavaScript
var Car = require('car');
var assert = require('chai').assert;
```

Once you've done that, it's time to start writing the actual tests. When writing tests, there are two "blocks" you need to be aware of. There are `describe` and `it` blocks. `describe` blocks usually describe a scenario, and `it` blocks describe specific assertions.

See the example below. If we are careful about how we organize the `describe` and `it` blocks, the test suite can read very well and make it easy for us to understand what we are testing. We can also use it as a way to DRY up our code and make it more concise.

```JavaScript
describe("A vehicle", function() {

  it("is of type Vehicle", function() {
    ....
  });

  describe("with two wheels", function() {

    var vehicle = new Vehicle(2);

    it("has a pedal to kickstart it", function() {
      // use vehicle with two wheels here
      ....
    });

  });

  describe("with four wheels", function() {

    var vehicle = new Vehicle(4);

    it("has a steering wheel", function() {
      // use vehicle with four wheels here
      ....
    });

  });

});
```

## How do I get started with testing?

Create a new workspace by cloning the [javascript-foundations](https://github.com/turingschool-examples/javascript-foundations) repo.

Let's run and implement the `Centaur` in the mythical-creatures repo together.