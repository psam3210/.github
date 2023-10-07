<br><br>
<figure align="center">
  <img src="https://www.moma.org/media/W1siZiIsIjI5MjQ3NCJdLFsicCIsImNvbnZlcnQiLCItcXVhbGl0eSA5MCAtcmVzaXplIDIwMDB4MjAwMFx1MDAzZSJdXQ.jpg?sha=5dbbf9e4472deb7f" height="400">
  <figcaption>Untitled, Donald Judd, 1991. <a href="https://www.moma.org/collection/works/93425">↗</a></figcaption>
</figure>
<br><br>


# Modular JavaScript

So far, all of our JavaScript has been written in one file. If you were poking around some of the assignments, you may have noticed that I make use of `import` and `export` statements. These are all attempts to modularize my code. As we write bigger and more complex software, it makes sense to start breaking out our code into separate files.

## Scope

Recall that we spent a lot of time discussing scope earlier this class. As a rule of thumb, you only scope variables and function at the highest level necessary.

<br><br>
<figure align="center">
  <img src="global.jpg" height="800">
  <figcaption>Why global variables are bad — a meme.</figcaption>
</figure>
<br><br>

We want to avoid polluting the global namespace because at that level, you don’t know what is modifying your variables. This can be good if you explicitly have a shared object you want to be universally accesible, but in general should be avoided.

When writing your initial entrypoint into a JavaScript program, you should always wrap it in an event listener for [`DOMContentLoaded`](https://developer.mozilla.org/en-US/docs/Web/API/Document/DOMContentLoaded_event). This allows you to:

1. Scope variables and functions out of the global namespace.
2. Ensure that any JavaScript is run after the browser is aware of the DOM tree.
3. Create less brittle code which doesn’t have to be placed before or after any specific DOM elements.

```js
document.addEventListener('DOMContentLoaded', () => {
  let courseName = 'PSAM3210';
  console.log(courseName);
})

console.log(courseName); // This gives us an error, as expected!
```

`DOMContentLoaded` is an event which gets called by the browser once the HTML of a page has been parsed and all scripts with the `defer` attribute have been executed. It’s really great because it guarantees your script will be consistently executed at the same point each time the page loads.

### A Note on `async` and `defer`

You may notice that sometimes you’ll see script tags with the following attributes:

```html
<script src="script.js" async></script>
<script src="script.js" defer></script>
```

`async` tells the browser to fetch this script in parallel to its other p=operations (like rendering a page). Once the file has been fetched, the script will run immediately. By fetching the script `async` you prevent a large JS file from blocking key browser metrics like [First Paint](https://developer.mozilla.org/en-US/docs/Glossary/First_paint).

`defer` tells the browser to also asynchronously fetch a script, but also defer the execution of the script until after the HTML has been parsed. Any scripts with `defer` will be run immediately before `DOMContentLoaded` is called.


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