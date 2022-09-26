<br><br>
<figure align="center">
  <img src="https://s3-us-west-2.amazonaws.com/collections.lacma.org-images/remote_images/ma-150089-WEB.jpg?2M5gpOjk6YunoEGDIm8UI_PcjswOPnBv">
  <figcaption>René Magritte, <em>The Treachery of Images</em>, 1929. <a href="https://collections.lacma.org/node/239578">↗</a></figcaption>
</figure>
<br><br>


# Event Listeners and Abstract Data Types

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

---

## Abstract Data Types

A **data type** is a set of values of a set of operations on those values. We’ve used these before, mainly the primitive data types of `Number`, `String`, and `Boolean`. These primitive data types map on to direct operations within the computer.

But as our programs become more complex, we may want to be able to represent other types of data. Enter the **abstract data type**, a data type whose representation is hidden to the client.

We often used Object Oriented Programming (OOP) to implement our abstract data types. OOP provides us with objects that we can manipulate and modify. For example, we can represent an abstract representation of a Color, a Picture, or as we’ve used before in class, an Apple.

### Using data types

```js
let a = new Array(10);
a.fill(5);
```

To use a data type, you need to know:
- Its name
- How to construct new objects
- How to apply operations to the given object

To construct a new object, we need to use the `new` keyword to invoke a constructor, and the name of the data type. In the above, we are constructing a new `Array` object, and passing in a set of parameters used to construct the object.

To apply an operation to an object, we can either modify it’s values directly or invoke methods to modify it. To do so, we need to know:
- The name of the object we are referring to (it is an instance of our data type).
- The dot operator `.` which indicates that we are applying a method or accessing a variable associated with that object.
- The method name or property we are trying to modify.

In the above, we’ve called the `.fill()` method on our instance of `Array`, `a`, which fills it with the passed in arguments.

Many built in objects have a robust documentation in MDN on which properties and methods are available to us to use. The MDN for an [`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) object contains things we’ve discussed so far, as well as other instance methods you may use. 

> **Instance methods** are applied to instances of objects, where as **static methods** apply independently of any instance of an object. Generally, we’ll be using instance methods but static methods are useful for providing behavior independent of an instance. The [`Math`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math) object is entirely static methods which we’ve used before.

### Creating data types

In many programming languages, the implementation of an ADT is known as a class. From a class, we can create instances of that class (our objects). To create a data type, you need to provide code which: 
- A set of values (instance variables)
- Implements operations on those values (methods)
- Creates and initializes new objects (constructors)

```js
// Class keyword defines a class called Apple.
class Apple {
  // Public variables.
  weight;
  color;
  isRotten = false; // Default value when an instance is created.

  // Private variables (denoted by #) mean you cannot access these variables outside of the function.
  // We don’t really use this in JS, but it is the default behavior in most other languages.
  #age = 0;

  // A method which gets invoked when new is called on the class name. Takes parameters which are the initial state of the object.
  constructor(weight, color) {
    // The this keyword refers to methods and properties that are associated within the object itself. Here, we set the instance’s variables to the passed in variables.
    this.weight = weight;
    this.color = color;
  }

  // An instance method, this method will modify the values of the object instance.
  bite() {
    
    // Doesn’t do anything if the instance of the apple is rotten. We must use the this keyword to refer to instance methods.
    if (this.isRotten()) {
      return;
    }

    if (this.weight > 0) {
      this.weight = this.weight - 1;
    }

    this.#age++;
  }
  
  // Another instance method which returns the value of isRotten.
  isRotten() {
    return this.isRotten;
  }

  // These are common setter and getter methods which are used to get and modify internal object properties.
  setAge(age) {
    this.#age = age;
  }

  getAge() {
    return this.#age;
  }

  // How would we modify this to set the rotten property of the apple?
}
```

Once we declare this class, we can use it like thus:
```js
// Creates a new instance of our Apple class data type.
let a = new Apple(100, 'red');
a.color; // Gets the color property, red
a.#age; // Won’t work because it is a private variable.

a.bite(); // Takes a bite of the apple.
```

> **Exercise: How would we represent a Shape? It should contain a relationship to an existin DOM element, and let us modify its properties. (Precursor to your next assignment)**

### Extending data types

The beauty of object oriented programming is that we can subclass classes. For example, let’s say we have a `Fruit` class which all fruit has. Then, the `Apple` class can `extend` it, which means all the original properties and methods of the `Fruit` class can still be accessed. But we can then define custom behavior for our `Apple` class. This is super helpful if we want all our fruit to implement some shared behavior.

```js
class Fruit {
  weight;
  color;
  isRotten = false;

  constructor(weight, color) {
    this.weight = weight;
    this.color = color;
  }

  bite() {
    this.weight--;
  }
}

class Apple extends Fruit {
  hasWorm = false;

  constructor(weight, color, hasWorm) {
    // If we want to override an existing constructor, we must call super(), which initializes the original class first.
    super();

    this.weight = weight;
    this.color = color;
    this.hasWorm = hasWorm;
  }
}

let a = new Apple(100, 'red', false);
a.bite();
```