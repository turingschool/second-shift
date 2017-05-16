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
* Create an array with three different messages. Make the alert box select one at random to display.

Directly embedding JavaScript to an HTML page works great, but it isn't how developers usually do it. Mixing JavaScript code with your HTML is not necessarily a bad idea, but the more complex your JavaScript code becomes, it might be a bit difficult to organize and keep track of it all. That's why it's more common to require one of more JavaScript files - that way your JavaScript's footprint in our shared space, the HTML document, doesn't get messier the more JavaScript code we write.

To require a JavaScript file, we use the script tag but with a few added attributes. 

The `src` attributes denotes where to find the resource - the resource being, in our case, a JavaScript file - and the `type` attribute sets the MIME type, identifying what we are about to import. If you don't specify a `type`, the browser will read the imported code as JavaScript by default.

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

### Check-ins

* Create an HTML page with an `h1` tag. Change the color of the `h1` tag after 5 seconds.
* Add `p` tag with some text in it. Add a button next to it which when clicked changes the text in the `p` tag to be all uppercase. Clicking the button again makes the text all lowercase.
* Add another button which when clicked changes the background color of the page.
* Add the attribute `contenteditable` to the `p` tag. What happens now when you click the `p` element? 
  * Add a button which updates the content of the `p` tag when a button is clicked.
* Add a button which changes the color of all text.
  * Add three buttons which represent different colors. When clicked, the text of the page should change to that color.