# Object Oriented Javascript and Classes

## Learning Objectives

- Explain the importance of OOJS
- Describe the role of ES2015 Classes and how they work
- Use the `new` keyword to create objects with shared properties
- Create a class that inherits from another using the `extends` and `super` keywords

## Framing

We've already gotten exposure to Javascript objects using object literal notation (i.e., the curly brackets). Some of you might have had something like this in your first project...

```js
const game = {
  cards: $(".cards"),
  startingTime: 0,
  createBoard: function(){
    let gameBoard = $("#game-board")
    gameBoard.css("display", "inline")
  }
};
```

What's nice about the above code snippet? Try answering that question by comparing it to this...

```js
let cards = $(".cards")
let startingTime = 0;
function createBoard(){
  let gameBoard = $("#game-board")
  gameBoard.css("display", "inline")
}
```

<details>

  <summary><strong>Some thoughts...</strong></summary>

  > * Related properties and methods are packaged together.
  > * Fewer global variables.
  > * Readability.

</details>

#### How have we been writing code up until this point?

We have been writing **procedural code**, which basically means we are writing and executing code as we need it. We'll define some variables and functions here, maybe some event listeners there. We end up with a lot of separate pieces that contribute to the overall functionality of an application.

#### What is an object in programming?

An object encapsulates related data and behavior in an organized structure.

#### Why might we use an OOP approach to programming?

Object-oriented programming (OOP) provides us with opportunities to clean up our procedural code and model it more-closely to the external world.

OOP helps us to achieve the following...
  * **Abstraction:** Determining essential features
  * **Encapsulation:** Containing and protecting methods and properties
  * **Modularity:** Breaking down a program into smaller sub-programs

OOP becomes **very** important as our front-end code grows in complexity. Even a simple app will have lots of code on the front-end to do things like...
* Send requests to a back-end to fetch / update / destroy data
* Update the state of the page as data changes
* Respond to events like clicking buttons

### Creating Objects

So far, we've had to make our objects 'by hand' (i.e. using object literals)...

```js
var celica = {
  model: 'Toy-Yoda Celica',
  color: 'limegreen',
  fuel: 100,
  drive: function() {
    this.fuel--;
    return 'Vroom!';
  },
  refuel: function() {
    this.fuel = 100;
  }
}

var civic = {
  model: 'Honda Civic',
  color: 'lemonchiffon',
  fuel: 100,
  drive: function() {
    this.fuel--;
    return 'Vroom!';
  },
  refuel: function() {
    this.fuel = 100;
  }
}
```

Even though we're technically using objects to organize our code, we can see a noticeable amount of duplication. Just imagine if we needed a hundred cars in our app! Our code would certainly not be considered "DRY".

As you may have noticed, some of these properties change between cars (`model` and `color`), and others stay the same. For example, `fuel` starts at 100, while the `drive` and `refuel` functions are the same for every car.

Why don't we build a function that makes these objects for us!

### We Do: Create a `makeCar` Function

> 5 minutes exercise. 5 minutes review.

Let's define a function `makeCar` that takes two parameters - `model` and `color` - and returns an object literal representing a car using those params.

```js
// This should return a car object just like the previous example
var celica = makeCar("Toy-Yoda Celica", "limegreen");
```

<details>
  <summary><strong>Solution...</strong></summary>

  ```js
  // ES5
  function makeCar(model, color){
    return {
      model: model,
      color: color
    }
  }
  ```

  ```js
  // ES6
  let makeCar = (model, color) => {
    return {
      model: model,
      color: color
    }
  }
  ```

</details>

This is the basic idea behind OOP: we define a blueprint for an object and use it to generate multiple instances of it!

## Classes

### Overview

If you want to follow along...

```bash
$ touch index.html script.js
$ atom .
$ open index.html
```

<details>

  <summary><strong>Here's some starter HTML if you need it...</strong></summary>

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Classes Practice</title>
  </head>
  <body>

  </body>
  <script src="script.js"></script>
  </html>
  ```
</details>

It's so common that we need to make objects with similar properties and methods that programming languages usually have some features to help with this.

In Javascript, we use an ES6 feature called **classes** to accomplish this. Here's a class that serves the same purpose as our car blueprint. We'll dive into the details in just a bit...

```js
class Car {
  constructor(model, color){
    this.model = model;
    this.color = color;
    this.fuel = 100;
  }
  drive(){
    this.fuel--;
    return "Vroom!";
  }
  refuel(){
    this.fuel = 100;
  }
}

const celica = new Car("Toy-Yoda Celica", "limegreen");
const civic = new Car("Honda Civic", "lemonchiffon");
```

Classes are a lot like the `makeCar` function we just created, but they're supported by JS and we use the `new` keyword to generate instances of an object (just like our earlier `celica` and `civic` examples).

> Note that classes start with a capital letter to make it obvious
that they are classes. This isn't necessary, but is a convention you should follow.



#### `this`

Notice the use of `this`, and the fact that we don't return from the class. Here's why we write classes this way...

When we generate a class instance using `new`, Javascript will automatically...
  1. Create a new, empty object for us  
  2. Generate a context for that object (`this` -> the new object)  
  3. Return the object  

#### Where are the Commas?

Unlike object notation, you do not need to use commas when separating class methods.

## Inheritance (15 minutes / 1:30)

Although OOP can help us keep our Javascript nice and clean, it's still easy to duplicate code when defining multiple classes. Consider the following example...

```js
class Dog {
  constructor(name, breed, tail){
    this.name = name;
    this.breed = breed;
    this.waggingTail = tail;
    this.diet = [];
  }
  eat(food){
    this.diet.push(food);
    console.log(this.diet);
  }
  bark(){
    return `Bark! Hello, this is dog. My name is ${this.name}`
  }
}

class Cat {
  constructor(name, breed, numLives){
    this.name = name;
    this.breed = breed;
    this.numLives = numLives;
    this.diet = [];
  }
  eat(food){
    this.diet.push(food);
    console.log(this.diet);
  }
  meow(){
    return `Meow! I am not a dog! My name is ${this.name}`
  }
}
```

Here we have two classes: `Dog` and `Cat`. They have some things in common: `name`, `breed`, `diet` and `eat`. They do differ, however, in that one `bark`s and the other `meow`s.

Imagine that we had to create a number of other classes - `Horse`, `Goat`, `Pig`, etc. - all of which share the same aforementioned properties but also have methods that are particular to the class.

How could we refactor this so that we don't have to keep writing out the shared class properties and methods. Enter **inheritance**...

```js
class Animal{
  constructor(name, breed){
    this.name = name;
    this.breed = breed;
    this.diet = [];
  }
  eat(food){
    this.diet.push(food);
    console.log(this.diet);
  }
}

const dog = new Animal("Fido", "Beagle");
```

Here we've defined an `Animal` class. It contains the properties and methods that are common among specific animal classes. Wouldn't it be nice if `Dog` and `Cat` could just reference this "parent" `Animal` class so that the only things we need to put in their "child" class definitions are the properties and methods that are particular to them (e.g., `bark`, `meow`).

Lucky for us, we can do that...

```js
class Animal {
  constructor(name, breed){
    this.name = name;
    this.breed = breed;
    this.diet = [];
  }
  eat(food){
    this.diet.push(food);
    console.log(diet);
  }
}

class Dog extends Animal {
  constructor(name, breed, tail){
    this.waggingTail = tail;
  }
  bark(){
    return `Bark! Hello, this is dog. My name is ${this.name}`
  }
}

class Cat extends Animal {
  constructor(name, breed, numLives){
    this.numLives = numLives;
  }
  meow(){
    return `Meow! I am not a dog! My name is ${this.name}`
  }
}
```

The clincher is `extends`. Whatever class is to the left of the `extends` keyword should inherit the properties and methods that belongs to the class to the right of the keyword. Let's see if this works...

```js
// Let's test out our parent. It just needs a name and breed.
const goat = new Animal("Gregory", "Mountain Goat");

// And now the children.
const fido = new Dog("Fido", "Beagle", true);
console.log(fido); // "this is not defined"
```

That didn't work out the way we expected. That's because we're forgetting one thing. When creating an instance of a child class, we need to make sure it invokes the constructor of the parent (`Animal`) class.

We can do that using the keyword `super()`...

```js
class Dog extends Animal {
  constructor(name, breed, tail){
    super(name, breed);
    this.waggingTail = tail;
  }
  bark(){
    return `Bark! Hello, this is dog. My name is ${this.name}`
  }
}
```

`super()` calls the constructor of the parent class. In the above example, once `super` does what it needs to do, it then runs through the rest of `Dog`s constructor.

> In order to give an instance of a child class context (i.e., be able to use `this`), you must call `super`.

## You Do: [Inheritance](https://github.com/ga-wdi-exercises/es6-classes-inheritance-practice) (20 minutes / 1:50)

> 15 minutes exercise. 5 minutes review.

-------

## Closing / Questions (10 minutes / 2:00)

* What are the benefits to using an OOP approach to programming?
* What is a class? What is `new`? How are they related?
* What does it mean to use "inheritance" when working with classes?
* How do we indicate that one class inherits from another?
* What does `super` mean?

## Homework: [Geometry](https://github.com/ga-wdi-exercises/js_geometry)

## Additional Reading

* [MDN Documentation on Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
* [Introduction to Javascript ES6 Classes](https://strongloop.com/strongblog/an-introduction-to-javascript-es6-classes/)
* [Getters, Setters, and Organizing Responsibility in Javascript](http://raganwald.com/2015/08/24/ready-get-set-go.html)
* [Static Members in ES6](http://odetocode.com/blogs/scott/archive/2015/02/02/static-members-in-es6.aspx)
* [Lesson: JS View Classes](https://github.com/ga-wdi-lessons/js-view-classes)

#### Prototypical Inheritance

* [Inheritance and the Prototype Chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
* [ES6 Classes and Javascript Prototypes](https://reinteractive.com/posts/235-es6-classes-and-javascript-prototypes)
* [Master the Javascript Interview: What's the Difference Between Class & Prototypical Inheritance](https://medium.com/javascript-scene/master-the-javascript-interview-what-s-the-difference-between-class-prototypal-inheritance-e4cd0a7562e9#.uzl8ohf8c)
