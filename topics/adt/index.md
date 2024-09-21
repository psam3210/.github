<br><br>
<figure align="center">
  <img src="https://s3-us-west-2.amazonaws.com/collections.lacma.org-images/remote_images/ma-150089-WEB.jpg?2M5gpOjk6YunoEGDIm8UI_PcjswOPnBv">
  <figcaption>René Magritte, <em>The Treachery of Images</em>, 1929. <a href="https://collections.lacma.org/node/239578">↗</a></figcaption>
</figure>
<br><br>


# Abstract Data Types

A **data type** is a set of values of a set of operations on those values. We’ve used these before, mainly the primitive data types of `Number`, `String`, and `Boolean`. These primitive data types map on to direct operations within the computer.

But as our programs become more complex, we may want to be able to represent other types of data. Enter the **abstract data type**, a data type whose representation is hidden to the client.

We often used Object Oriented Programming (OOP) to implement our abstract data types. OOP provides us with objects that we can manipulate and modify. For example, we can represent an abstract representation of a Color, a Picture, or as we’ve used before in class, an Apple.

## Using data types

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

## Creating data types

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

## Extending data types

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