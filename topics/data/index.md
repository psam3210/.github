<br><br>
<figure align="center">
  <img src="https://www.moma.org/media/W1siZiIsIjU0MDA3MCJdLFsicCIsImNvbnZlcnQiLCItcXVhbGl0eSA5MCAtcmVzaXplIDIwMDB4MjAwMFx1MDAzZSJdXQ.jpg?sha=9ebbad0b653e3c0f" height="400">
  <figcaption>IBM, Diagram of Dynamic Random-Access Memory Chip (DRAM), 1984. <a href="https://www.moma.org/collection/works/4288">↗</a></figcaption>
</figure>
<br><br>


# Data

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