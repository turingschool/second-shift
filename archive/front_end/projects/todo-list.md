# Project: TodoList

Customer problem statement:

_It's almost guaranteed that I will forget things I have to do if I don't write them down. I'm also having difficulties to organize my todo list and never seem to be able to complete things in time._

In this project we are going to build out a smarter TodoList for our customer. The base requirements are:

* being able to add new todo items
* being able to remove todo items
* being able to set a deadline

The additional requirements are:

* being able to set an estimated duration
* being able to set one of three labels: `non-urgent`, `important`, `urgent`
* being able to mark todos as complete

We are going to be building this iteratively, writing just as much code needed to fulfill the requirement in order to get an MVP out the door for feedback.

### Iteration 1: being able to add new items

We should first create a basic HTML document. Make sure to include the following tags:  `html`, `head`, `body`, `footer`, and `header`. We don't have a lot of data right now to put on the page, so add some FPO data as you see appropriate.

If we are adding new items, we need to have an input field where the user can enter the title of their new todo, as well as a button they can click once they wish to add the todo to the page. When they click the button, we need to take the text that they have entered in the input field. Next, we need to display that data, actually printing it to the screen. We can keep all todos in a simple unordered list for now.

* Add an input field to the page
* Add a button to the page with text `Submit`. When the button is clicked, `console.log` the value of the input field.
* Now, change the button to create a new list item with the value of the input field as text and print it to the screen when clicked.
* Make sure that you can add multiple list items to the screen.

### Iteration 2: being able to remove items

You probably have more items than you want on your page now since there's no way to delete them! Our check list for deleting items looks something like this:

* Add a button or link to every list item that says `Delete` or `Remove`. 
* When clicked, that button should remove the list item it was clicked for.

### Iteration 3: being able to set a deadline

Our list is becoming a bit overloaded as we are about to add the third separate element for a given list tag. It needs to hold the `title`, `Delete` button, and now also a date indicating when it's due. Since each row will have mutliple different pieces of information, let's convert the list to an HTML table.

* Convert unordered list to an HTML table
* Add a second input field where the user can set or pick a date
* When the user clicks `Submit`, the date should also be printed to the screen
* The table should have three columns for each row: a `title`, a `due date`, and a button which when clicked removes an item

---

### Iteration 4: being able to set an estimated duration

Our user needs help better organizing thier time. Let's help out by allowing them to set an estimated duration for each task in minutes. By default we slot 30 min to each todo.

* Add an additional input field which takes an estimated duration in minutes
* The table should have an additional column `duration` with the estimated times in
* The user should be able to edit the estimated time in the table

### Iteration 5: being able to set one of three labels: `non-urgent`, `important`, `urgent`

To further help them organize when to do which task, let's add a feature to add different labels to the todo items. By default we set the label to `non-urgent`. 

* Add a dropdown from which the user can choose `non-urgent`, `important`, or `urgent`
* When an option is selected and the user clicks `Submit`, the option should display on the table
* The user should be able to change the value in the table

### Iteration 6: being able to mark todos as complete

To avoid doing the same work twice, let's add a checkbox to each row in the table to indicate whether or not the item has been completed. By default the todos should not be completed.

* Add a column `status` in the table
* Have the value for all the items in the table be `Incomplete` by default
* When the field is clicked, change the text to `Complete`