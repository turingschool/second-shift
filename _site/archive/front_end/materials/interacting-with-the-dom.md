# Introduction to interacting with the DOM

## Adding JavaScript to an HTML page

All browsers support the [`<script></script>`](mdn.io/script-tag) tag which executes JavaScript.

It's fairly straightforward to embed JavaScript between the script tags on your page.

```JavaScript
<script>
  console.log("so snowy outside!");
</script>
```

Let's try to make the following things happen using JavaScript:

* Make an alert box pop up with any message when a user loads the page.
* Create an array with three different messages. Make the alert box display all three messages.
  * Change it so that only the first message displays.
  * Change it so that it selects one message to display at random.

---

Directly embedding JavaScript to an HTML page works great, but it isn't how developers usually do it. Mixing JavaScript code with your HTML is not necessarily a bad idea, but the more complex your JavaScript code becomes, it might be a bit difficult to organize and keep track of it all. That's why it's more common to require one of more JavaScript files - that way your JavaScript's footprint in our shared space, the HTML document, doesn't get messier the more JavaScript code we write.

To require a JavaScript file, we use the script tag but with a few added attributes.

The `src` attributes denotes where to find the resource - the resource being, in our case, a JavaScript file - and the `type` attribute sets the (MIME) type, identifying what we are about to import. If you don't specify a `type`, the browser will read the imported code as JavaScript by default.

```JavaScript
<script src="index.js" type="text/javascript"></script>
```

### Check-ins

* Create an HTML document, add a `script` tag and make some JavaScript happen in the browser
  * Wrap the code from the previous step in a function
  * Trigger the alert message every 10 seconds
* Now, create a file `script.js` and require it in the HTML file. Add some JavaScript in the file so you know it's required ok.
* What are blocking vs non-blocking operations?

---

## Interacting with the DOM

* [Lesson plan](http://frontend.turing.io/lessons/module-1/js-3-dom-manipulation.html)

### Check-ins - 1

* Create an HTML page with an `h1` tag.
* Add a button with the text "blue". Add a background color and text color to the button.
* Add an event listener to the button, so that when it's clicked, an alert box pops up with the text "blue".
* Change the event listener so that when the button is clicked, the color of the `h1` tag becomes blue.
* Add another button, with the text "red". When you click that button, the `h1` text becomes red.
* If the `h1` text already is blue when you click the `blue` button, display an alert box that says "Already blue!", and the text becomes magenta.
* If the `h1` text already is red when you click the `red` button, display an alert box that says "Already red!", and the text becomes magenta.

### Check-ins - 2

* Add `p` tag with some text in it.
* Add a button next to it which when clicked changes the text in the `p` tag to be all uppercase.
* Clicking the button again makes the text all lowercase.
* Add a button which changes the color of the `p` tag.
* Add another button which when clicked changes the background color of the page.

### Check-ins - 3

* Create an empty input field and a button with the text "display"
* Add an empty `h1` tag.
* Enter some text in the input field. When you click submit, log the text to the console.
* Update the event listener so that when you have entered text and click the button, the `h1` is updated to have the text entered in the input field.
* Empty the input field after the `display` button is clicked.

### Check-ins 4

* Add a box to the page with a 200px width and a 200px height.
* Add an input field under the box and a button with the text "set color".
* In the input field, enter the name of a color. When you click the button and have entered for example "green", the box should change to have green background color.
* Add same input fields/button pairs to also change the width and the height of the box.
* Empty the input field after the `display` button is clicked.
