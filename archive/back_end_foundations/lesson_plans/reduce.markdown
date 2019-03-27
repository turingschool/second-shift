## reduce/inject

### Preface
Reduce is the Cadillac of enumerables. The power of reduce is that if you really, really understand reduce, you don't have to remember how any other enumerable works. You can recreate its functionality with reduce. That's why reduce is really powerful.

But remember two important lessons I've repeated to you time and time again.

1. Don't be clever. No one likes a clever programmer. No one.
2. Clarity is King. Or Queen.

All this to say that _even if_ you can implement your desired functionality using a single complex reduce statement, opt instead for a couple simple enumerables chained together in a readable, decipherable way.

```
"The era of developers working alone is over.
 Welcome to the communication
age"

- Sarah Mei
(March 3, 2016 via Twitter)
Founder of Railsbridge,
Director of Ruby Central,
Chief Consultant of DevMynd Software.

```

If you EVER write clever code, you will feel a prickling at the back of your neck and that will be me, psychically judging you from afar.

### Using reduce

So let's talk about how to use it. Three pieces:
1) The starting value, which is the starting value of the second item,
2) The first block parameter and is accepted as an argument in reduce/inject, and
3) The third item, the second block parameter.

If you don't want to punch me in the face after that explanation you are a better person than I. Let's explain things by diagramming things out and talking about how this all works.

```
collection.inject(starting_value) { |running_total, variable| block }
```

Now, in practice.
```ruby
array = [1,2,3,4,5]
=> [1,2,3,4,5]

array.reduce(0) { |sum, num| sum += num }
=> 15
```

So, with this general case in mind, we should look at the example with the sum. We pass start to the inject method. Start is the initial value, and if we don't define start or pass anything to inject, its value is the first item over the array we are iterating over. Result is what gets passed when the enumerable is complete. Variable is the variable name we use each time we iterate through the collection, and block is whatever operation we need to perform each time we iterate through.

### Check for Understanding
Summing is easy, but we can also use `reduce` to build other things.

**Challenge #1:** Start with a array of 6 words, and write a `reduce` block that returns a string with the first letter of all the words in the array.

**Challenge #2:** Start with a string of one long word (your choice). Write a `reduce` block that returns a count of all the letters in the word.

**Challenge #3:** Given an array of the numbers 1 through ten, use inject to return an array of even numbers.
