# Front End Second Shift 1

* Introduction
* Warmup
* How computers work
* Introduction to JavaScript
* Preparing your development environment
* JavaScript 1: Strings, Integers, Operators

---

## Introduction (20 min)

* introductions
* program outline
* careers in software

## Warmup (10 min)

LSAT problem.
https://drive.google.com/drive/u/0/folders/0Bx6JZxtPBe_FblNwRWdMNEwzOFU

## How computers work (25 min + 5 min break)

Our computer is just a machine, also referred to as **hardware**. It receives **instructions** either entered by us directly from the keyboard, or generated from applications running on the computer. These applications are called **software**, instructions programmed and designed to make common and more complex tasks easier on a computer. Regardless, there needs to be some kind of interaction in order for the computer to perform - that's why instructions are usually referred to as commands. Commands are computed and the result is communicated back to us, usually via a display.

The so-called brain of the computer is the **CPU**, the central processing unit, or simply just the processor. It consists of three units; **the control unit** which interprets and schedules instructions to be carried out, **the arithmetic logical unit** (ALU) which carries out logical/arithmetic operations, and the **cache memory** which is a short term memory storage.

Memory is an interesting topic and for a given consumer computer there are a few different kinds of memory. Some data you might only want to store temporarily, others you might want to store permanently. Of the data you store permanently, some data you may want to access frequently whereas others you won't need very often.

Data that you want to store temporarily goes to the **RAM**, or random access memory. Chrome is an application that uses a lot of your RAM, there are a lot of pieces of data Chrome need to store temporarily, for example buffering a video on youtube.

Data that you want to store permamently goes to either the **SSD** (solid state drive) or the **HDD** (hard disk drive). The SSD supports faster read/writes, but are not as large as HDD's. HDD's are not as fast as SSD's but will get you more storage for less.

Almost all computers have an **operating system** which defines how to interact with the underlying system. Operating systems need to comply with **POSIX** standards which is a protocol for maintaining compatability between operating systems. Most operating systems have two interfaces, one **GUI** (graphical user interface) and one **CLI** (command line interface). Most users only use the GUI, however the CLI is necessary for performing more lower level tasks. Using the CLI for common tasks can greatly speed up your workflow as well as allow for automation of some tasks.

More resources:

* [How computers work](https://www.youtube.com/watch?v=_QElVrqBwnk)
* [How computer memory works](https://www.youtube.com/watch?v=p3q5zWCw8J4)

### Goals

* Be familiar with how a computer works
* Understand the following terms:
  * hardware
  * software
  * central processing unit (CPU)
  * graphical user interface (gui)
  * command line interface (cli)

---

## Introduction to JavaScript (25 min + 5 min break)

For a long time, JavaScript could only be executed in the browser. Every programming language needs to be **parsed and translated** (compiled) to lower level languages so that they can be executed (read) by the CPU. The thing that compiles JavaScript down to bytecode is called the **JavaScript engine**. Every browser has a JavaScript engine - Chrome uses V8, Firefox uses SpiderMonkey, Safari uses Nitro. Even though the engines are different, they all perform the same task: they execute JavaScript code.

In addition to a engine, there is also the **runtime**. A runtime is essentially a wrapper which provides some additional objects to interact with. In the browser, we can interact with the DOM, document object model, and the window object. These parts are not part of JavaScript itself but enable interactivity between JavaScript and the native elements. For example, if you want to use JavaScript to make a website to be interactive, you need a way to access specific elements on your page and do something with them.

In 2009, the **Node.js** project was initially released. It's a runtime which can be used outside of the browser, meaning that JavaScript could be used to write server side code! Node.js uses the V8 engine to execute JavaScript. The Node.js runtime provides some really cool functions you can use to interact with the computer. There is an `fs` module to access the filesystem, you can work with `buffers` and `streams` to perform async tasks, and the `crypto` module provides cryptographic capabilities.

To get to the browser console, right click in the browser and select `Inspect element`.
To get to the Node.js console, navigate to a terminal and enter the command `node` - make sure it's installed it if it doesn't work.

### Goals

* Be familiar with the following terms:
  * JavaScript Engine
  * JavaScript runtime
* Know how to use the browser console to execute JavaScript

--- 

## Preparing your development environment (30 min)

Having a reliable development environment is really important and can make a huge difference in your general productivity. As you become more experienced you will also become more opinionated about how you want things to be. Good news are that you can completely customize your development environment to make programming as frictionless as possible. As a starter, we are going to use [cloud9](c9.io), a web based development environment.

If you don't have a [GitHub](www.github.com/signup) account, go and create one!

### Terminal 101

Bash stands for "Bourne Again Shell" and it's a command line interface for executing Unix commands. In a nutshell, it provides another way of performing common tasks on your computer and it's essential to working as a software developer. To start, we are just going to learn how to do some common tasks in the terminal. We need to know how to **create** files and directories, **delete** files and directories, and know how to move around.

* touch
* mkdir
* rm, rm -rf
* cd, cd ..
* ls

##### Exercises

* go to your root
* create a directory called `test-directory`
* create a directory called `cool-directory`
* without changing directories, create a file in the `test-directory` called `test-file.txt`
* without changing directories, create a file in the `cool-directory` called `cool-file.txt`
* change directories to the `test-directory` and add some text to the file you recently created
* go up one directory
* change directories to the `cool-directory` and add some text to the file you recently created
* go up one directory
* remove the `test-directory` and its contents
* list what you have in your root
* remove the `cool-directory` and its contents
* list what you have in your root and verify that both directories you created are gone

### Goals

* Know how to navigate a workspace in cloud9
* Be familiar with basic terminal commands: touch/mkdir/cd/ls
* Have a GitHub account
* Know how to use the Node console to execute JavaScript

---

## JavaScript 1: Strings, Integers, Operators (60 min)

In this section we are going to start working with JavaScript - FINALLY!

Areas of focus:

* [`String`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)
* [`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)
* [`Boolean`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)
* [`Variable`](https://developer.mozilla.org/en-US/docs/Glossary/Variable)
* Operators
  * [`Arithmetic operators`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators#Arithmetic_operators)
  * [`Logical operators`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators#Binary_logical_operators)

### String

A `String` is a very common data type used in programming. Strings are flexible and can either hold no characters, and there's in theory no limit to how many they can hold. A String is annotated using quotes, either single or double quotes.

```JavaScript
> ""
''
> ''
''
> typeof ""
'string'
```

Strings are indexed and you can access specific characters using bracket notation.

```JavaScript
> 'hello'[0]
'h'
> 'hello'[1]
'e'
> 'hello'[5]
undefined
```

Strings have access to many prototype methods which makes them easier to work with. For example, if you're only interested in the first two characters of the string:

```JavaScript
> 'hello'[0] + 'hello'[1]
'he'
> 'hello'.slice(0,2)
'he'
```

Or if you want to find characters between two indices:

```JavaScript
> 'hello'[3] + 'hello'[4]
'lo'
> 'hello'.substring(3,5)
'lo'
```

Or repeating a sequence of characters a given number of times: 

```JavaScript
> 'yo ' + 'yo ' + 'yo ' + 'yo ' + 'yo '
'yo yo yo yo yo '
> 'yo '.repeat(10)
'yo yo yo yo yo yo yo yo yo yo '
```

#### Exercises

* Go to the [docs](mdn.io/string) and find how to use the String prototype functions `concat`,  `toLowerCase`, `toUpperCase`, and `split`.

### Number

Numbers are used to represent numerical values. In JavaScript, large numbers are written without commas.

```JavaScript
> 0
0
> 12
12
> 123456
123456
> 123,456
456
```

If a value cannot be parsed as a number, it will return `NaN` (not a number). In this case, the string `"5"` when parsed as a number becomes the numeric value `5`, and can be multiplied by `4`. The string "car" cannot be casted to a number and therefore we return `NaN`. 

```JavaScript
> 4 * 5
20
> 4 * "5"
20
> 4 * "car"
NaN
```

The `parseInt` function is used to parse strings to integers.

```JavaScript
> parseInt(123)
123
> parseInt("123")
123
> parseInt("123,456")
123
> parseInt("four")
NaN
```

#### Check-ins

* What's the difference between the function `parseInt` and `parseFloat`?
* What's the difference between the function `isNaN` and `isInteger`?

### Arithmetic Operators

Explore the below arithmetic operators using what you just learned about Strings and Numbers.

* addition: `+`
* subtraction: `-`
* division: `/`
* multiplication: `*`
* remainder (modulo): `%`

Answer the following questions:

* Which operators work on Strings? 
* Which operators work on Numbers?
* Can you use operators with one String and one Number?
* What is the return value of the modulo operator?

### Logical operators Operators

There are two binary logical operators and they usually return a `boolean` value - either `true` or `false`.

The `&&` (and) operator evaluates to true if the expressions on both side of it returns true.

```JavaScript
> false && true
false
> true && false
false
> false && false
false
> true && true
true
```

The `||` (or) operator evaluates to true if an expression on either side of it is true.

```JavaScript
> true || true
true
> false || false
false
> true || false
true
> false || true
true
```

### Check-ins

* Write down two statements using the `||` operator and two using the `&&` operator. Verifying the return value of the statements in the Node console first, let the person next to you guess whether the four statements will evaluate to `true` or `false`. 
* String, Number, Variable, and Arithmetic operators
  * Greeting
    * Create three variables - `name`, `age`, and, `homeTown` - and assign them values.
    * Print `My name is X and I am Y years old. I live in Z.`, where `X`, `Y`, `Z` represents one of your three variables.
  * Creating a sentence
    * Assign the value `tomatoes` to a variable `expression`
    * Add ` in my pants` to the end of the variable
    * Add `a lot of ` to the beginning of the variable
    * What's the value of the variable `expression`? Desired is: `a lot of tomatoes in my pants`
* Logical operators
  * What does `"hello" || false` evaluate to? Why?
  * What does `"hello" && undefined` evaluate to? Why?
  * What does `"hello" && "it's me"` evaluate to? Why?
  * What does `"hello" || "it's me"` evaluate to? Why?

### Goals

* Understand the difference between and how to use the following JavaScript data types:
  * string
  * number
  * true
  * false
  * undefined
* understand what a variable is and how to assign a value to it
* be familiar with some common prototype methods for string and number
* know how to use arithmetic operators
* know how to use logical operators

![](https://media.giphy.com/media/JJluJeEqQL4Pe/giphy.gif)