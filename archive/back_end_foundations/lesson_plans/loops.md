### Learning Goals

* apply the `times` method to repeat instructions
* use `while` and `until` to repeat instructions
* use `loop` and `break` to repeat instructions
* break out of an infinite loop in both IRB and regular Ruby

## Discussion

### Looping

A loop is a set of instructions that is executed repeatedly until some condition is met. This condition may be a certain number of times that the loop is executed, or it may be a question that returns a true/false answer.

Examples:

- While looking for a parking spot at a crowded sporting event, a car continues to drive up and down the rows until an empty spot is found. (Loop that executes until a question returns true or false)
- After baking cookies, you pull the cookie sheet out of the oven which holds 24 cookies. One by one, you remove each of the cookies from the sheet and place them on a cooling rack. (Set of instructions that executes 24 times)

*Try it*: What are some other examples of looping in the real world?

* `while`

```ruby
while condition (boolean)
  # code to execute
end
```

* `until`

```ruby
until condition (boolean)
  # code to execute
end
```

* `times`


```ruby
5.times do
  # code to execute
end
```

```ruby
5.times do |number|
  # code to execute
end
```
