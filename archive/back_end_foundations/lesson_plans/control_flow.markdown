---
title: Control Flow With Conditionals
---

### Learning Goals

* Be able to use gets to receive user input from the keyboard.
* Be able to successfully use a single branch conditional.
* Be able to successfully use a multiple branch conditional.

### Interactivity

So far, we've written some programs that just do a thing, based on the
information that we've given it. That's fine and all, but BOOOOORING.


Imagine if you bought a game for your phone from the App Store or the Google
Play store, and how the game works is if you start it and it just tells you
if you've won or lost. Would this not be the worst game ever? Personally,
   if I had paid money for this app, I would be on the phone with my credit card
   company in order to get a chargeback. People wouldn't download this app if it
   was FREE.


   The point that I'm trying to drive here is that right now, we only really
   know how to write programs in Ruby that don't really have any sort of
   interactivity. And here's the thing - interactivity is what makes things really
   great. Well mostly great. If you go down into the comments section of any web
   site you just start debating the usefulness of the internet and the goodness
   of humanity - but still - comments sections make people come back.


   So the next thing we are going to learn is just how we can add some
   interactivity to our ruby programs. We are going to see now how we can
   prompt the user to enter something via the keyboard.


   ```ruby
   puts "Please enter your name!"
   name = gets.chomp
   puts "Nice to meet you, #{name}"
   ```

   So what can we infer is happing on the second line here? It pauses execution
   and waits for the user to enter something and then it will save whatever
   the user types as a string inside the variable `name`. It's very important
   to remember that it saves it to a string. So even if you type in 11023, it
   will regard it as a string and not a Fixnum.


### Control Flow

   Now that we've got that out of the way, let's watch this fantastic video.


   [Control Flow](https://www.youtube.com/watch?v=UgI8CP6vpCg)


   Even though the code in the music video was in Python, the principles remain
   pretty relevant. Computer programs need a way to go along two different paths
   if they need to.


   This is something that also happens in every day life. Every time we buy
   something using a debit card or a credit card, a message gets sent to the
   financial company with an amount of how much you want to spend. If you have
   enough money it's approved, if you don't, it's declined.


   Conditionals can have an unlimited number of branches. We can have something
   as simple as one branch: "If a student has a GPA of 3.8 or higher, they can
   go to the Honor Roll ceremony."


   They can also be multiple branches. "If you want to have a fancy dinner.
   Otherwise, cook dinner at home."


   We'll look at the single branch first. If a student has a GPA of 3.8 or higher,
   they go to the Honor Roll Ceremony.


   The basic syntax of a single branch conditional statement in ruby.

   ```ruby
    if conditional
      action
    end
   ```

   Knowing what the basic syntax is, let's apply it to the first example above.

   ```ruby
    student_gpa = 3.7

    if student_gpa > 3.8
      puts "GOING TO THE CEREMONY"
    end
  ```

Here, the student only goes to the ceremony if the GPA is higher than 3.8


What happens if it is less than 3.8?


What happens if it is 3.8 on the nose?


Now, you try on your own. Let's combine the two ideas that we've learned so
far. Ask the user to type in a kind of cookie. If the user types in,
"Oatmeal Raisin" the computer is to reply, "That's not a cookie."


That's a single branch. Let's talk about how we would handle a second
possibility? Let's look at the syntax.

```ruby
  if condition
    do thing
  elsif condition
    do something else
  end
```

So let's go back the scenario of eating out.

```ruby
  money = 100
  if money > 200
    puts "TIME TO EAT OUT!"
  else
    puts "Eating at home.  : ("
  end
```

This is a little different from the previous scenario, because earlier,
we only wanted a reaction if the condition we wanted was true. Now, we want
some other action to take place if it isn't true.


There are also situations where we may want a ton more branches, and a ton of
additional situations. To handle that, we would use the `elsif`.


Let's look at a situation where we would have three differents choices when
eating out.

```ruby
  money = 20
  if money > 200
    puts "TIME TO EAT OUT!"
  elsif money < 25
    puts "Instant Ramen."
  end
```

What elsif does is that it adds in another situation that we may need to
account for here.

But we have a hole here. There's a situation that we just aren't covering.
What happens if we have 100 money? Right. Nothing happens. This is why when
you are writing code, you should make sure that you're covering for all
possible cases. And this is where else really comes into play. I would
rewrite the above statement like this:

```ruby
  money = 20
  if money > 200
    puts "TIME TO EAT OUT"
  elsif money > 25
    puts "Frozen pizza."
  else
    puts "Instant ramen."
  end
```

So order matters here. What happens if we put the `money > 25` part first?

Now, it's time for you to try something on your own. write a program that asks
the user to enter a number. Now, based on what they input, print out to the
screen if the number is even, odd, or neither. (Let's just count zero as being
neither.)


Some things to make a note of slash hints:
* Anything that comes in from `gets` will be a string.
* How do you think we can tell if a number is even or odd?
* Is there another way that doesn't make use of a method?
