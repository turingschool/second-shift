# Number Guesser

For this project, you are going to build a number guesser game. The program will set a secret number in the beginning of the round, and the user will enter guesses through a text field. If the guess is either above or below the secret number, we will print "Too high!" or "Too low!" to the screen. If the user guesses the secret number, we will let the user know they won and start a new round.

---

## Iteration 1 - create the project

Set up your project. You need an HTML document with `html`, `head`, and `body` section. The `head` should have a `title`. Add an `h1` tag to the `body` section with the text `Number Guesser`.

Add [jQuery](https://code.jquery.com/) to your project as well as a `script` section. Within the `script` tags, make sure that `console.log($)` prints out a jQuery object.

## Iteration 2 - set a secret number

First, we want to be able to set a secret number that the user can guess.

Within the script tags, create a variable called `secretNumber` which gets set to a new random number every time the page loads. Print out the variable `secretNumber` using `console.log()` and make sure that the number changes when you refresh the page.

## Iteration 3 - print the secret number when you click a button

Add a button to the page. Add an `id` selector to it, and set the value to `"show-secret-number"`. This number is going to be our "cheat button" so that we don't have to guess a million times before beating the game.

Within the `script` tags, add an event listener which prints out the `secretNumber` everytime you press the button.

Once you have been able to print the number to the console, change the event listener so that it appends a new `p` element to the dom with the text: "The secret number is: XX", where `XX` is the secret number.

## Iteration 4 - add an input field

Add an input field to the page with an `id` selector set to `guess-input-field`. After having entered some value in the input field, play around in the console and try to fetch the current value of the input field.

## Iteration 5 - add a submit button

Add a button to the page with `type="Submit"` and an `id` selector set to `"submit-button"`. Add an event listener so that when you click the button, something gets printed to the console.

Once you have gotten that working, change the event listener so that when you click the button, the current value of the input field is printed to the console.

## Iteration 6 - compare the guess to the secret number

In this iteration we are going to be putting it all together. We are going to take the number we entered in the input field and compare it to the `secretNumber`.

If the number is higher than the `secretNumber`, we are going to print an `h2` tag to the screen with the text: "Too high!". If the number is lower than the `secretNumber`, we are going to print an `h2` tag to the screen with the text "Too high!". If the `secretNumber` is equal to the number we entered, print the text "You guessed right!" to the screen.

## Iteration 7 - start a new round

Now that we have a way of beating the game, we need to have a way to start a new game.

Add a button to the page with the text "Start new game" which simply resets the value of `secretNumber`.