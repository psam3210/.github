<br><br>
<figure align="center">
  <img src="https://whitneymedia.org/assets/artwork/57720/2018_55_cropped.jpeg">
  <figcaption>Rafaël Rozendal, <em>Abstract Browsing 17 03 05 (Google)</em>, 2017. <a href="https://whitney.org/collection/works/57720">↗</a></figcaption>
</figure>
<br><br>


# DOM and Event Listeners

One of the great things about JavaScript is that it gives us the ability to interact with the webpage. This happens because the language authors have designed *abstractions* for HTML and CSS so that we can interact with the webpage. This abstraction is also referred to as the [Document Object Model](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction) (DOM).

### Accessing and Manipulating DOM

Let’s say we have some HTML and we want to access certain elements. JavaScript has a `document` object, which represents the HTML code of our website that we can use to access certain elements within it. 

```html
<body>
  <h1 id="heading">This is a heading.</h1>
  <p>This is a paragraph.</p>
  <p>This is another paragraph.</p>
  <ul class="list">
    <li class="list_item">List item 1</li>
    <li class="list_item">List item 2</li>
  </ul>
</body>
```

```js
  // Matches the first element with an id="heading" attribute
  let h1 = document.getElementById('heading'); // Returns <h1>

  // Selects the first element which matches the given CSS selector
  let p = document.querySelector('p'); // Returns <p>
  let div = document.querySelector('div'); // Returns null

  // Selects the first element which matches the given CSS selector
  let ps = document.querySelectorAll('p'); // Returns [<p>, <p>]
  let divs = document.querySelectorAll('div'); // Returns []

  // How do we access all list items?
  let liEls= document.querySelectorAll('li');
  let liEls = document.querySelectorAll('.list_item');
  let liEls = document.querySelectorAll('.list > .list_item');
```

These functions all return objects which are JS representations of the HTML element itself. We can do things like…

```html
<body>
  <h1 id="heading"><a href="/">This is a heading.</a></h1>
</body>
```

```js
  let h1 = document.getElementById('heading'); // Returns <h1>
  h1.innerText; // Gets the rendered text contained within an element, This is a heading.
  h1.innerHTML; // Gets the inner HTML of an element, <a href="/">This is a heading.</a>

  h1.innerText = 'New heading'; // Sets the value of innerText of our h1 to 'New heading'
  h1.innerHTML = '<a href="/">New heading</a>'; // Same here, but can use HTML elements

  h1.style; // Gets the inline styles of an element.
  window.getComputedStyle(h1); // Get all computed styles of the element. Window is used because it represents the visual, not the Document.

  h1.style.color = 'red'; // Sets the color of the element to 'red'

  // Sets the background color. You can either use CSS property names as in the first line, or camelCase the property as in the second line.
  h1.style['background-color'] = 'green';
  h1.style.backgroundColor = 'green';

  // Use classList to add, remove, or toggle the red class on an element.
  h1.classList.add('red');
  h1.classList.remove('red');
  h1.classList.toggle('red');
```

We can even manipulate the DOM by creating new elements in JavaScript. 

```js
  // Create a new div element.
  const newDiv = document.createElement('div');
  // Give it some copy.
  newDiv.innerText = 'Hi, I’m a newly created element!';

  // Add the element into the DOM.
  document.body.appendChild(newDiv);
```

There is no way we can cover everything you can do — I would suggest querying Google with your questions, and seeing what StackOverflow, or MDN give you. 

## Manipulating DOM

Last lecture we discussed how we can use JavaScript to access and manipulate the DOM of a webpage. Today, we’ll talk about how to handle user input, both via forms and via event listeners.

## Form Inputs

Let’s say we have an HTML form that a user has at their disposal, maybe something like…

```html
<form>
  <div>
    <label for="name">Enter your name: </label>
    <input type="text" name="name" id="name" required>
  </div>
  <div>
    <label for="email">Enter your email: </label>
    <input type="email" name="email" id="email" required>
  </div>
  <div>
    <input type="submit" value="Click me!">
  </div>
</form>
```

How would we access the current value of an input element? Well, we can do that by first selecting the element, and then getting it’s value. This approach applies to all input types, though most commonly via a textfield input.

```js
// We can use query selector and use CSS based attribute targeting to grab our input element.
let nameInput = document.querySelector('input[name="name"]');
let name = nameInput.value;

// The same for email.
let emailInput = document.querySelector('input[name="email"]');
let email = emailInput.value;

console.log(name, email);
```

> Let’s say we wanted to transform the name input into uppercase. What should we do before transforming the data? We need to check if there is anything in the input field first! This is because we have no guarantee the input field is filled.

But let’s say we want to run this when the user clicks on the submit button. Well, we can use event listeners for this!

## Event Listeners

An [event listener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) is a method which sets up a predefined function to be called back to when a specific event is triggered on the original element. We use the following syntax:

```js
let buttonEl = document.querySelector('input[type="submit"]');

buttonEl.addEventListener('click', () => {
  console.log('I’ve been clicked!');
});
```

This adds an event listener to our button element, specifically on the `'click'` event. When that event occurs, we’ll print out the statement `I’ve been clicked!`. Breaking this down, we have:

- `buttonEl` the target element
- `.addEventListener()` the function to add an event listener, which takes two arguments.
  - `'click'` the first argument defines the event to listen to. In this case, it’s the `'click'` event.
  - `() =>{}` or `functionName` the second argument is a function which will get called when that event occurs. You can also declare a function elsewhere and pass that name here. Note that you don’t execute it here, so you don’t include the `()` that we usually use to call a function.

There are other events you may use, such as `'input'`, `'mouseover'`,  `'mouseout'`, `'mousemove'`, `'scroll'`, `'keydown'`. 

```js
let emailInput = document.querySelector('input[name="email"]');
let printEmail = () => {
  console.log(emailInput.value);
}

// What will this do? It’ll print the value of the email field every time a user inputs something.
emailInput.addEventListener('input', printEmail);
```

Using named functions for handling events is very useful in the case that we want to further modify that listener, or if we want to have multiple events point to the same handler.

```js
let nameInput = document.querySelector('input[name="name"]');
let emailInput = document.querySelector('input[name="email"]');

let handleInput = () => {
  console.log(nameInput.value, emailInput.value);
}

// This will call the same function, but on different input events.
nameInput.addEventListener('input', handleInput);
emailInput.addEventListener('input', handleInput);

```

Let’s say you want to remove an event listener after you’ve created it. This might be because you only want behavior to work when an element is showing. We can use `.removeEventListener()` to do this. However, this’ll only work with named functions — anonymous functions won’t work!

```js
window.addEventListener('scroll', () => {
  console.log('I am scrolling');
});

// This won’t work, because the anonymous function is pointing to a DIFFERENT function than the original event listener.
window.removeEventListener('scroll', () => {
  console.log('I am scrolling');
});

// This will work.
const scrollHandler = () => {
  console.log('I am scrolling');
}

window.addEventListener('scroll', scrollHandler);
window.removeEventListener('scroll', scrollHandler);
```

Sometimes events can also pass in event objects to their handlers. This object represents certain attributes specific to that event. We can write handlers to handle that as well. A very common one is the keydown event, which handles keyboard input.

```js
document.addEventListener('keydown', (evt) => {
  
  // Logs out the key event.
  console.log(evt);

  // Checks which key was pressed.
  if (evt.key === 'Escape') {
    window.close();
  }
});
```

Between event listeners and DOM manipulation, we can do some pretty powerful things. In your next assignment, you’ll be asked to create a digital drawing program using only HTML elements. All of this can be handled via setting DOM properties using style attributes, and having event listeners handle user input.

> You may have seen other ways to add event listeners to DOM elements, such as `buttonEl.onclick = () => {}`. We generally avoid this practice because it means we can never remove event listeners once we’ve added them. This also only lets us add one handler per event, when in fact we can have as many as we want.
