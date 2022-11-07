<br><br>
<figure align="center">
  <img src="https://www.moma.org/media/W1siZiIsIjI5MjQ3NCJdLFsicCIsImNvbnZlcnQiLCItcXVhbGl0eSA5MCAtcmVzaXplIDIwMDB4MjAwMFx1MDAzZSJdXQ.jpg?sha=5dbbf9e4472deb7f" height="400">
  <figcaption>Untitled, Donald Judd, 1991. <a href="https://www.moma.org/collection/works/93425">↗</a></figcaption>
</figure>
<br><br>


# Modular JavaScript, and Data

So far, all of our JavaScript has been written in one file. If you were poking around some of the assignments, you may have noticed that I make use of `import` and `export` statements. These are all attempts to modularize my code. As we write bigger and more complex software, it makes sense to start breaking out our code into separate files. We’ll also take a look at where to store data beyond other JavaScript files.

## Modules

Traditionally, we’ve always stored our scripts in JavaScript files that are then pulled into our web pages like this:

```html
<script src="script.js"></script>
```

This works pretty well, but what if we actually have a ton of different scripts, doing many different things. Our script imports might look something more like this:

```html
<script src="lodash.js"></script>
<script src="p5.js"></script>
<script src="site.js"></script>
<script src="home.js"></script>
<script src="nav.js"></script>
<script src="buttons.js"></script>
```

This quickly becomes unwieldy. What happens if we want to access a variable like `navColor` that is declared in `nav.js` in a previously pulled in file, such as `site.js`? Well, to do so, we’d have to move the import of `nav.js` before `site.js`. This is pretty inefficient!

Introducing [JS Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules). Modules let us segment our code into files of like code. For example, we can store classes in their own files, and have a file for utilities as well. By default, these files are self contained — you can’t access their variables or functions in other files. To do so, you use the `import` and `export` keywords.

Our syntax also changes. We need to let the browser know that we are organizing our code using modules:

```html
<script src="site.js" type="module"></script>
```

## Export

The first thing you do to get access to module features is export them. This is done using the [`export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) statement.

Since variables and clases are not publicly accessible by other files, we need to indicate which ones should be exported. This lets us keep certain internal helper code within our file without polluting the `window` namespace.

```js
const navColor = '#0000ff';
const defaultColor = '#ff00ff';

const sum = (x, y) => {
  return x + y;
}

// We export navColor and sum, but not defaultColor
export { navColor, sum };
```

Sometimes if we only have one thing in our file, such as a class, we can have a default export:

```js
export default class Nav {
  constructor();
}
```

## Import

The import declaration is used to [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) exports from other modules. Lets take a look at the following setup.

```
index.html
site.js
nav.js
utils.js
```

```js
// nav.js
export default class Nav {
  constructor();
}
```

```js
// utils.js
const navColor = '#0000ff';
const defaultColor = '#ff00ff';

const sum = (x, y) => {
  return x + y;
}

export { navColor, sum };
```

At this point in time, we’ve set up two files that contain exported code. We then set up `site.js` to tie it all together.

```js
// site.js
// This imports the default export of nav.js and names it Nav.
import Nav from './nav.js';

// This imports navColor only from utils.js, and we can use it like any other variable.
import { navColor } from './utils.js';

// This imports all exports from utils.js and puts it under the utils object.
import * as utils from './utils.js';

document.addEventListener('DOMContentLoaded', () => {
  let n = new Nav();

  document.querySelector('nav').style.backgroundColor = navColor;
  document.querySelector('nav').style.backgroundColor = utils.navColor;
  
})
```

## Structuring a larger web app

In general, it is a good idea to keep your JS files relatively lean. Their naming should suggest their purpose, and their purpose should be precise enough so that one can understand exactly what the file contains without much guessing. 

You can think of each JavaScript file as mapping on to a component of your website. For example, you might have `gallery.js` which handles your image galleries, `nav.js` to handle your navigation, and `links.js` to handle any fancy link behavior you have. Then perhaps a `utils.js` file that has utility functions. Finally there is `site.js` to tie it all together.

In a directory, you would have
```
index.html
scripts/
  site.js
  gallery.js
  nav.js
  links.js
  utils.js
```

This sort of structure makes it incredibly easy to section your code into more digestible chunks.

## Package Managers

Modules can be packaged up and turned into frameworks! We’ve talked a bit now about how frameworks are just fancy wrappers for vanilla JavaScript. These packages are then distributed widely using a platform called Node Package Manager ([npm](https://www.npmjs.com/)). There are other [package managers](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Understanding_client-side_tools/Package_management) out there as well, such as Yarn.

As some of you may have seen, we use `npm` and `node` to run the course assignment tests, which is a JavaScript library called [Jest](https://jestjs.io/). 

If we want to pull in external frameworks, we’ll often make use of something called [Webpack](https://webpack.js.org/) to bundle the node modules in with our JavaScript. Think of this tool as something that merges all our code into one big JavaScript file that a browser can use.

While we won’t get more into specifics, I would encourage you to check out this [tutorial](https://webpack.js.org/guides/getting-started/) to learn more.

---
## Data

Data is a collection of discrete values that convey information. The entire internet is founded on the flow of information and data. We’ve encountered data before, using Objects and Arrays to store values into data structures that our programs can manipulate.

At its core, data is stored in some abstraction that supports [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations. CRUD stands for:

- **Create** a new piece of data
- **Read** an existing set of data
- **Update** an existing piece of data
- **Delete** an existing piece of data

Today, we’ll discuss how data can be managed and stored using JavaScript.

## Local Storage

[`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) allows us to persist data in the browser. It is associated with web addresses, and lets us keep track of data without having to use a database. 

`localStorage` encapsulates a [`Storage`](https://developer.mozilla.org/en-US/docs/Web/API/Storage) object. There is also [`sessionStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage) which persists data until the session ends (however long a person spends on a website).

```js
// Create
localStorage.setItem('myCat', 'Tom');

// Read
const cat = localStorage.getItem('myCat');

// Update
localStorage.setItem('myCat', 'Neko');

// Delete
localStorage.removeItem('myCat');
```

## APIs

An [Application Programming Interface](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Introduction) (API) is are constructs made available in programming languages to allow developers to create complex functionality more easily. 

APIs abstract more complex code away, providing some easier syntax to use in its place. An API has two parts: it has a specification and an implementation. The implementation is expected to follow the rules of the specification. 

We’ve already used APIs before, such as `document.querySelector()` or `Math.random()`. The W3C, a group of industry professionals, creates an API specification. Then, web browsers are expected to implement this specification. 

These days, APIs are used to communicate back and forth with external data sources. Web APIs exist as specific URLs that you construct to get the data you want. Here are two examples:

- [Weather](https://open-meteo.com/en/docs#api-documentation)
- [Are.na](https://dev.are.na/documentation)

These above are links to the documentation or specification of the API. They delinate instructions for using the API (sending queries), as well as how you should expect the data to be returned back to you.

For our purposes, we can expect all API response to be returned back to us in JavaScript Object Notation (JSON).

We’ve also used JSON before! To set the style of an element `myEl`, we do:

```js
myEl.style.backgroundColor = 'green';
myEl["style"]["backgroundColor"] = 'green';
```

In this case, `myEl` is an object, `style` is a property of `myEl`, but is also an object, and `backgroundColor` is a property of `style`. We can reconstruct this abstraction in JSON as such:

```js
myEl = {
  'style': {
    'backgroundColor': 'green',
    'fontSize': '14px',
    'marginTop': '36px',
  }
};

```

## `fetch()`

To request data from another URL, we use the JavaScript [`fetch()`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) function. When `fetch()` is called, it returns an asynchronous [promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) which will resolve when the data is returned back. Promises and asynchronous functions are out of the scope of this course.

```js
// A simple example of using fetch with arrow functions.
fetch('http://example.com/movies.json')
  .then((response) => response.json())
  .then((data) => console.log(data));
```

## Fetching from Are.na

Let’s take a look Meg Miler’s Are.na channel, [Good Sign-Offs](https://www.are.na/meg-miller/good-sign-offs). It has a unique channel slug `good-sign-offs`. We can look at Are.na’s [API](https://dev.are.na/documentation/channels#Block43472) and determine that if you visit the URL `https://api.are.na/v2/channels/[slug]`, you can get a JSON representation of the channel

<br><br>
<figure align="center">
  <img src="api-response.png" height="400">
  <figcaption>Response from Are.na for the below code.</figcaption>
</figure>
<br><br>

```js
let slug = 'good-sign-offs';
let url = `https://api.are.na/v2/channels/${slug}?per=100`;

fetch(url)
  .then((response) => response.json())
  .then((data) => {
    // Print out all the data
    console.log(data);

    // Print out the title property of the data
    console.log(data.title);

    // Gets the blocks
    let blocks = data.contents;
    blocks.forEach((block) => {
      // Prints out the content of the blocks
      console.log(block.content)
    })
  });

```