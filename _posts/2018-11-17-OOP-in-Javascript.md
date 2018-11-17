---
layout: post
title: OOP in Javascript with Mosh Hamedani
tags: [Javascript, OOP, ES6, node.js, npm, babel, webpack, babel, vscode]
---
### [Review of the course Object-oriented Programming in JavaScript](https://www.udemy.com/javascript-object-oriented-programming/)

This is my second course in Udemy. The first was the [web developer bootcamp](https://www.udemy.com/the-web-developer-bootcamp/). I take the first part, the Front-end part, for the moment. The second part, Backend's, is waiting for the right moment. Also I have the pending matter to do a review of that course. I hope I will do it soon.

Anyway, I choose this OOP in Javascript course for two main reasons:

- First, after all my path learning web development, and specifically Javascript, I realized that there was something I was not confortable with and things that taked me a lot of difficulties to learn. The goal to take that course is that, to strength my knowledge and be prepared for all that is coming.

- Second, the instructor. [Mosh Hamedani](https://programmingwithmosh.com/) is a great teacher and explains all the subjects in an easy and clear way, and clearly knows everything he explains, and you see that in his courses.

Below I start my review of the content of the course, section by section:

The course has 6 sections.

#### The first section is an introduction to OOP in Javascript. Explains the 4 pilars of OOP and puts yourself in the right way to learn. These 4 pilars are:

  - **Encapsulation**: To group in a little units different things, such as variables (properties) and functions (methods) in objects to operate with them.

  - **Abstraction**: Hide complexity through hiding the excess of visible methods in the objects. This keeps simpler interfaces and reduces the negative impact of changes in your application.

  - **Inheritance**: Mechanism that allows you to eliminate redundant code creating parent's object from which all these new objects will inherit some features.

  - **Polymorphism**: Objects can behave in many ways. Different objects can have the same method but, at the sime time, the outcome may be different.

All along the course, this pilars are introduced and deeply reviewed.

---


#### The second section introduces _Objects_.

In this sections the **encapsulation** is explained all along.

You can find, also, the different ways to create an Object (literal, factory function and constructor function); an explanation of the constructor property and the functions as Objects.

As the instructor says all along the course:
> In JavaScript, functions are objects. They have properties and methods.

Other topics in this section are the value and reference types and the different things you can do with Object properties, such as add properties, remove, enumerate, etc. There is a also a topic about the private properties and methods and how to access that with getters and setters. With this private methods we can learn about another pilar in OOP, **abstraction**.

At the end of every section there is an exercise to consolidate all the topics we have learned. I have to recognise I have had a lot of trouble with the exercises because I didn't know even how to start. Finally I did some of them.

#### The third section is about _Prototypes_.

This section talks about **Inheritance** in JavaScript, also known as Prototypical Inheritance.

In JavaScript, unlike other languages with clases (in Javascript there is no clases, just Objects), there is a different way to implement Inheritance. This way is through Prototypes. Prototypes are basically, parents of other objects.

> Every object (except the root object) has a prototype (parent).

You could define the **Prototypical Inheritance** as the capacity to inherit properties or methods from your prototype.

An example of that is when you create a new object based on an object constructor. The new object inherits all the properties and methods to the constructor and all the properties and methods to the root Object. This is _multilevel inheritance_.

**Inheritance** works like this: you have a property or method in an Object and then, when you create a new object through the parent (constructor) JavaScript engine looks in this new Object searching a property or method. If it doesn't find there, looks the parent Object to find it.

That mechanism is useful to structure your code because you can put a method in the prototype of your constructor and that method will be accessible for all the instances that you will create through this constructor.

In order to access to the instance members (property or method in constructor) or prototype members you have to use for loop because other options such as _Object.key()_ just shows you the instance members.

---

#### Fourth section is about _Prototypical Inheritance and Polymorphism_

This section follows the same topic of the third section and goes a little further.

It is explained how to use prototypes to do custom Prototypical inheritance with your own objects. Is how you structure your data in order to do a good OOP coding.

That works because you convert the prototype of an object in a prototype or another object. Because of that, the second object inherits all the properties and methods of that new and custom object. You can do it like:
```
obj.prototype = Object.create(parentobj.prototype);
```

But, this way has some problems with the constructor because this constructor is overwrited. Then, you have to force the constructor of your new Object. Also, you have some trouble if you want to access to a property of the new Object's parent. For that purpose you need to use a _call_ method to call the _super constructor_.

In order to clean all the code and do it more readable, it is possible to put the _obj.prototype_ and the forced constructor in a new function, known as _intermediate function inheritance_.

After that, there is another topic about _method overwriting_ in the child Objects.

All this information is useful to understand one of the 4 pilars of OOP in Javascript: **Polymorphism**. Polymorphism basically is when you have a custom property or method in an Object but, at the same time, you have the same method in different Objects with a parent in common. The method behaves different thanks to  _method overwriting_.

After all that information, the instructor explains some warnings about inheritance.
The abuse of that tool may have some troubles because of the complexity and the big hierarchy is created with inheritance. The tip is clear:
> Keep it simple!

In contrast, we can use **Composition**.

Composition works like this: you create different Objects and, after that, you can create a new Object and assign the previous Objects to give the new Object some behaviour. You can do the composition thing with _mixins_.

---

#### Fifth section is about _Classes_

In ES6 there is a new way to create objects and implement inheritance: **classes**. This classes aren't like classes in other languages, like C# or Java. In JavaScript classes are _syntactic sugar_ over Prototypical inheritance. In short, classes are like regular objects with a different syntax that approaches to the old-school classes. The syntax is:
```
class Example {
    constructor(value) {
        this.value = value;
    }

    // These methods will be added to the prototype.
    obj() {
    }

    // This will be available on the Example class (Example.newobj())
    static newobj(another) {
    }
}

```

It is possible to prove that going to [Babel](https://babeljs.io/) and compiling the new syntax classes. The outcome shows to you these classes are regular constructor functions (objects).

Classes has a different features and benefits:
- Better implementation of Inheritance. You just have to add _extend_ like this:
```
class Newclass extends Parentclass {}
```
- No hoisting. _Hoisting_ is when you have a function declaration and you can call it everywhere in your code. That does not happen with all the expression sentences, like functions expressions or variables expressions, neither with all the types of classes.
- Also you have private methods (or static methods). And also there is private members to do some _abstraction_ through _symbols_ and _weakmaps_. This _symbols_ are a new primitive type in ES6.

---

#### The last section is about _modules_

Modules are the different files in what you split your application. This modularity has different benefits:

- _Maintainability_: The code is better organized.
- _Reuse_: With modules increases the options to reuse one or more modules in other parts of the application or in another application as well.
- _Abstract code_: You can hide some abstraction in a module and shows the necessary.

Until ES6 does not exists modules. With this new implementation we have a different module systems. In this course we can see two of them, _CommonJS_ to work with Node and _ES6 Modules_ to work with Javascript in the browsers.

- _CommonJS_: You can export modules with _module exports_ and import with _require_.
- _ES6 Modules_: You can export modules adding _export_ to a class or a function. And you can import modules like this:
```
import {Module} from 'origin'
```

After that, there is a different sections talking about ES6 tooling like a trasnpilers or bundlers. Here we can find information about how it works Babel and Webpack and some valuable examples.

---
<br>

As a final notes I want to emphasize that when I was progressing in the course I realized that my last chapter will be React or another similar library. All the concepts and information ring me a bell and I was thinking, well, that is how it is structured React. Not the way it works, but how it is created.

After take all the course I definetely could confirm that feeling. This course is very useful, not just to know the OOP in Javascript, but also if you want to know how some of the current Javascript libraries has been made.

And that is my short-term future, learn how to Reacts works and do some stuff with it. For that purpose I bought a great course in React of the same author, Mosh Hamedani.

---
