
## Hello to Javascript

Part one of a three-part introduction to D3 at NICAR 2019

## Math in Javascript

```js
5 + 4 // 9

Math.round(3.14) // 3
```

## Writing JS not in your console

Go to the `hello-javascript` folder and open up the file called `main.js` in your favorite text editor. Type the following into the file, hit save, then refresh your web page.

```js
console.log("Hello world!");
```
You should see `Hello world!` printed in your console.

### Cool, but aren't we here to make charts?

Yes! Let's talk about data.

## Variables

Variables store data. They can store numbers, strings, objects and the output of functions.

```js
var myBool = true;
var myNum = 3;
var myString = "cats";

console.log("I love my " + myString); // I love my cats

```

## Lists and objects

The two most common ways you will store data in javascript is with arrays and objects. 

An array is simply a list of things (strings, numbers, objects or even other arrays) between square brackets.

```js
var numbers = [1,2,3,4,5]
var animals = ["dogs","cats","ferrets","fish","hamsters"]
```

You can access the items inside an array by their index number. Indexes always start at zero

```js
animals[1]
// cats
```

A javascript object is defined by a key/value structure inside curly brackets.

```js
var person = {
    "name": "Michael",
    "job": "CEO",
    "num_children": 1,
    "quote": "I don't know what I expected."
}
```

You can access the values of an object by using the correct key, using either a dot or brackets and quotes

```js
person.name // Michael
// This is the same as person["name"]
```

## What the heck is JSON

JSON, pronounced jay-son, is a type of data file. Not unlike excel or CSV, it is a syntax for storing and exchanging data. It is made up of a combination of arrays and objects

Say you have a spreadsheet that looks like this:

| name    | job      | num_children | quote                                     |
|---------|----------|--------------|-------------------------------------------|
| Michael | CEO      | 1            | I don't know what I expected.             |
| GOB     | Magician | 1            | I've made a huge mistake.                 |
| Lindsay | Activist | 1            | It's vodka. It goes bad once it's opened. |
| Buster  | Army     | 0            | I'm a monster!                            |

The JSON equivalent of this spreadsheet would be:

```js
var bluths = [
    {
        "name": "Michael",
        "job": "CEO",
        "num_children": 1,
        "quote": "I don't know what I expected."
    },{
        "name": "GOB",
        "job": "Magician",
        "num_children": 1,
        "quote": "I've made a huge mistake."
    },{
        "name": "Lindsay",
        "job": "Activist",
        "num_children": 1,
        "quote": "It's vodka. It goes bad once it's opened."
    },{
        "name": "Buster",
        "job": "Army",
        "num_children": 0,
        "quote": "I'm a monster!"
    }
]
```

### So why use JSON when I can just use a spreadsheet? 

Because JSON supports "nested" data, which allows for more freedom when representing complex data.

```json
{
    "name": "GOB",
    "job": "Magician",
    "children": [
        {
            "name": "Steve",
            "job": "Pest control",
            "quote": "Steve Holt!"
        }
    ],
    "quotes": [
        "I've made a huge mistake.",
        "Come on!",
        "They're laughing with me!"
    ]
}
```

## For loops

You will use many `for` loops when working with data. For loops allow you to iterate through data in order to operate on it.

Copy the following and paste it into your `main.js`

```js
var bluths = [
    {
        "name": "Michael",
        "job": "CEO",
        "num_children": 1,
        "quote": "I don't know what I expected."
    },{
        "name": "GOB",
        "job": "Magician",
        "num_children": 1,
        "quote": "I've made a huge mistake."
    },{
        "name": "Lindsay",
        "job": "Activist",
        "num_children": 1,
        "quote": "It's vodka. It goes bad once it's opened."
    },{
        "name": "Buster",
        "job": "Army",
        "num_children": 0,
        "quote": "I'm a monster!"
    }
];

bluths.forEach(function(bluth) { 
    console.log(bluth.name); 
});
```

Save your file and hit refresh on your browser window. Check your console, and you should see the four names printed out on separate lines. 

## If statements

These are conditional statements. It means they will do different actions based on whether a statement is evaluated as true or false. 

```js

bluths.forEach(function(bluth) {
    if (bluth.num_children > 0) {
        console.log(bluth.name + " has children")
    } else {
        console.log(bluth.name + " does not have children")
    }
})

```

## Put it all together 

So far we've learned what JavaScript and the DOM are, variables, objects and loops. This is an extremely narrow snippet of what JavaScript can do, but these are basic concepts that you'll use all the time as you dive deeper into programming. 

To get you ready for the next couple of classes in this series, we're going to write a loop that cycles through our data, and arranges it into a table on the page.

In your `index.html` file, paste this code just underneath `<div id="hello"></div>`:

```html
<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Job</th>
            <th>Children</th>
            <th>Quote</th>
        </tr>
    </thead>
    <tbody id="bluth-table">
        <!-- Your table rows will go here -->
    </tbody>
</table>
```

Save and refresh your browser. You should see some table headers on your page.

Next, paste this code into your `main.js`:

```js
var table = document.getElementById('bluth-table');

bluths.forEach(function(bluth) { 
    var tableRow = table.insertRow(-1);
    for(i in bluth){
        var tableCell = tableRow.insertCell(-1);
        tableCell.innerHTML = bluth[i];
    }
});
```

This doen't look like a ton of code, but trust me, it's doing *a lot*. Save and refresh your page. You should see a table there with all of the bluths. Let's walk through what happened step-by-step.

First, we create a table variable. This is so we can talk to it later when we want to add our bluths to it.

```js
table = document.getElementById('bluth-table')
```

Next, we start our for loop. You'll notice that this looks different than the for loop we used earlier. There are probably 10 different ways to write a for loop in JavaScript and this is another one of my favorites because it's very explicit about what it's doing. For each object in our object list, perform this function or code block. We'll talk more on functions in a bit- for now just know that functions are blocks of code that perform a task.

```js
bluths.forEach(function(bluth) { 
```
Then we create another variable `tableRow`. This creates a new line for each person.

```js
var tableRow = table.insertRow(-1);
```

Geeze louise, another for loop *within* a for loop?! You betcha another for loop! This time we're looping through the attributes within our pet object. So the first time this loop runs it's grabbing the name, the second time it's grabbing the type, next the age and finally the color.

```js
for(i in bluth){
```

For all of the pet attributes we're looping through, we make another variable for the table cells where our pet data points will live. This line of code tells the DOM to find where we made our table row in the first loop a couple of steps back, and to insert a new table cell. 

That `(-1)` is just special to this particular `insertCell()` method and means that we want the table cell to be added to the end. This is something I'd never seen before and had to google. We're learning together!

```js
var tableCell = tableRow.insertCell(-1);
```

This is where we tell our table what exactly we want to add. First, we tell the DOM to grab the `tableCell` we created a second ago. Next, that `innerHTML` preps it for whatever text or HTML snippet we wnant to stick in there. In this case, we want to add the data in our object. Now for a little bit of JavaScript magic. We get this data by getting the object we're looping and telling it to grab the data inside with `[i]`. Square brackets next to an object is a JavaScript convention that indicates we want to break into that object and grab the data inside.

```js
tableCell.innerHTML = bluth[i];
```


### Bonus round! Functions

A function in JavaScript is a block of code that performs a task. A function won't happens unless we "call" it.

#### But that's weird, why wouldn't I just write the code I want to happen when I want it to happen?

There's a concept in programming called DRY which stands for Don't Repeat Yourself. Functions allow us to write reusable blocks of code that we can use later rather than writing them out over and over again. For instance, what if we had two data sets we needed to create tables for?

Copy and paste this new `plants` variable just underneath our `bluths` variable.

```js
var plants = [
    {
        "Name": "Cyclamen",
        "Type": "flower",
        "Description": "Pink, downward-facing flowers",
    },
    {
        "Name": "Jalepeno",
        "Type": "pepper",
        "Description": "Long, green peppers",
    },
    {
        "Name": "Haworthia",
        "Type": "succulent",
        "Description": "Dark green, spiky succulent with white stripes",
    },

]
```
First thing, let's get our `index.html` file ready for a new table. Paste this just below our first table:

```html
<table>
    <tr>
        <th>Name</th>
        <th>Type</th>
        <th>Description</th>
    </tr>
    <tbody id="plant-table">
    </tbody>
</table>
```

That sets up a new table with correct headers and a `<tbody>` with an ID of `plant-table` so our code knows what to look for.

#### Your first function

We're going to transform our table making code into something we can use for whatever data we have. Swap out this code that we wrote earlier in our `main.js`:

```js
table = document.getElementById('bluth-table')

bluths.forEach(function(obj) { 
    var tableRow = table.insertRow(-1);
    for(i in obj){
        var tableCell = tableRow.insertCell(-1);
        tableCell.innerHTML = obj[i];
    }
});
```

for this new code:

```js
function tableMaker(id, data) {
    table = document.getElementById(id)

    data.forEach(function(obj) { 
        var tableRow = table.insertRow(-1);
        for(i in obj){
            var tableCell = tableRow.insertCell(-1);
            tableCell.innerHTML = obj[i];
        }
    });
}
```

Then, at the very bottom, we'll call our function. First for bluths, then for plants

```js
tableMaker('bluth-table', bluths);
tableMaker('plant-table', plants);
```

Save, and look at your browser. Refresh your `index.html`. Did it work?

### Wait, what just happened?

Let's walk through line-by-line.

#### Functions, parentheses and parameters, oh my 😬:

```js
function tableMaker(id, data) {
```

This first step is probably the most important part. It establishes that we're about to write a function, names the function (in this case, `tableMaker`) and tells the function what information to expect. We call whatever we put in the parentheses parameters. Before, when our code looked like this:

```js
table = document.getElementById('bluth-table')

bluths.forEach(function(obj) { 
    var tableRow = table.insertRow(-1);
    for(i in obj){
        var tableCell = tableRow.insertCell(-1);
        tableCell.innerHTML = obj[i];
    }
});
```

We told it exactly what to expect. We told it to grab `bluth-table` and that we'd be looping through our `bluths` data. Now that our code looks like this:

```js
function tableMaker(id, data) {
    table = document.getElementById(id)

    data.forEach(function(obj) { 
        var tableRow = table.insertRow(-1);
        for(i in obj){
            var tableCell = tableRow.insertCell(-1);
            tableCell.innerHTML = obj[i];
        }
    });
}
```
We've swapped our `bluths` and `bluth-table` for more generic `id` and `data` parameters that we can change later when we call the function.

For the most part, the rest of this code block looks a lot like we just walked through when we built our first table. The only things that changed were the ID we target and the data we cycle through. See? Not so bad!

Last thing, we have to call our function! In order for functions to happen, something has to tell it to run which is what we do at the bottom of our `main.js` file with:

```js
tableMaker('bluth-table', bluths);
tableMaker('plant-table', plants);
```
Here, we tell our tableMaker to run and to fill in those `id` and `data` parameters with the ID we want to target and the data we want to use. 

One weird thing to note, you'll see that `bluth-table` and `plant-table` are wrapped in quotes. That's because they're strings, meaning they're just words we're looking for. `bluths` and `plants` are not wrapped in quotes because they're variables. They represent the big chunk of JSON we grabbed earlier.

### We did it!

We did a ton in this class. If you feel overwhelmed, that's totally okay. Here are some of the things we went over:

- What is JavaScript?
- The DOM
- The web console
- Arrays
- Variables
- JSON
- Objects
- For loops
- console.log()
- Functions

These are the fundamentals of JavaScript, and now that you have a little familiarity with them- you're ready to start adding on to your programming toolkit.

🎉

### Notes

- [Intro to JavaScript and jQuery from NICAR 2016](https://github.com/scottpham/JS2WorkshopNICAR2016)
- [w3schools intro to JavaScript](https://www.w3schools.com/js/js_intro.asp)
- [Mozilla's intro to JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/A_first_splash)
- [Another JavaScript tutorial](https://www.tutorialspoint.com/javascript/index.htm)
- [More about for loops](https://www.w3schools.com/js/js_loop_for.asp)