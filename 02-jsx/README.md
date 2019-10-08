# JSX

## Objective

> What will I learn?

The goal of this lesson is to learn what JSX is, why it is useful, and how it can be transpiled using Babel.

## Overview

> What is it?

JSX stands for JavaScript XML.
"JSX is a preprocessor step that adds XML syntax to JavaScript."
JSX is "a syntax extension to JavaScript."
"Fundamentally, JSX just provides syntactic sugar for the `React.createElement()` function."
"JSX is an inline markup that looks like HTML and gets transformed to JavaScript."

You: "Okay, so JSX is just a syntax extension for JavaScript that adds HTML-like markup in the mix to help simplify the creation of React elements and components."

Yeah. I like the way you paraphrase. Keep that up. 

Just remember that JSX needs to be compiled into JavaScript at the end of the day. That's where Babel comes in.

Babel is "the compiler for writing next generation JavaScript." Put next-gen JS in and get browser-compatible JS out. Essentially, Babel is an ES6 to ES5 compiler (translating JavaScript to JavaScript), but it has several bonuses.
Fun side note: translator + compiler = transpiler.
Personally I sometimes feel as though calling a transpiler "Babel" is misleading because the origin of the word "Babel" is rooted in the biblical story of the Tower of Babel (or city of Babel). What BabelJS really does is the opposite of that story, because it takes languages that browsers currently don't understand (namely ES6 and JSX) and transpiles them into one common language that all browsers understand (ES5).
Anyway, the Babel team (a bunch of nice ladies and gents, I'm sure) was kind enough to implement a way to compile React's JSX to the non-JSX `React.createElement()`.

## Purpose

> Why should I care? What problem(s) does this solve?

The value of JSX lies in its readability and familiarity. Its function is to keep all logic local and testable in JavaScript; AKA it will stop you from going down the template language route for defining your UI and components. JSX allows you to simply define your UI and components with this unique/custom HTML right inside the same file. Even designers can recognize and parse the code you're writing now!
Put simply, JSX looks nice and reads nice. JSX can take a nasty, nested looking JS file that is full of React elements and turn it into something quite lovely and comprehensible at a glance.
Also, because JSX is compiled to JavaScript, all the code you write with JSX can have tests written against it and can be debugged much more simply than with some template language, like handlebars. Although many don't even like comparing JSX to a template language because it is so much more flexible and versatile.

Babel is awesome because it takes advanced JavaScript and JSX and turns it into good ole ES5 code that will run in nearly every browser/JS environment.
"Babel has support for the latest version of JavaScript through syntax transformers. These plugins allow you to use new syntax, *right now* without waiting for browser support."

## Walkthrough

> Let's check it out!

It's Detailed Exploratory, Explanatory Application Examination Exercise Lab Activity Time! (Super catchy, I know. You'll be saying it all day.)

Okay, so we left off with this snippet of pure JS:

```js
    ReactDOM.render(
      React.createElement('h1', null, 'Wow! Thanks React.'),
      document.getElementById('content')
    );
```

At a glance, you can kind of gather what's happening. But how might you read this?
"Render an h1 React element that says 'Wow! Thanks React.' inside the element with the id of 'content'."
Now you're thinking, "In an ideal world, I would just write out the header tags and content inside to explicitly define my React element."
What if I told you that that ideal world exists and is this one? What if I told you that JSX allows you to do exactly that?
Check this out:

```jsx
    ReactDOM.render(
      <h1>Wow! Thanks JSX.</h1>,
      document.getElementById('content')
    );
```

Was that what you were imagining? Good. I'm glad JSX could make your dreams come true.
Did you try this in your browser? Ha! Gotchya! It didn't work. You know why? That's right: we forgot about Babel.
Wanna see the magic work? Add this just before your script:

```html
    <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

Try again! Did it work? Woohoo! We have accomplished the same thing as the first lesson, but don't you love the new look?
Let's get a lil' more crazy. Are you familiar with new features from ES6? I sure hope so. One of my personal favorites are the inclusions of `let` and `const` in the syntax. Wanna see it in action? Bring the React element that you pass in as the first parameter for the ReactDOM render function out and make it variable, like so:

```jsx
    const element = <h1>Wow! Thanks Babel.</h1>;
    ReactDOM.render(
      element,
      document.getElementById('content')
    );
```

Babel will transpile the line `const element = <h1>Wow! Thanks Babel.</h1>;` into `var element = React.createElement("h1", null, "Wow! Thanks Babel.");`

Just so you know, you're currently using a standalone version of Babel, which is a no-no in a production app. Here's what Babel docs say on the matter: "Compiling in the browser has a fairly limited use case, so if you are working on a production site you should be precompiling your scripts server-side." That's where a build system would come into play, but we're not diving into that right now.

If you want to see the magic working in real-time, check out this site and have some good, safe fun!
https://babeljs.io/repl/

## Challenge

> I double dog dare you.

Bet you can't make some other cool React element that uses other HTML tags! (Just kidding, I believe in you with full confidence.)

Also, why not try out some new fancy, next-gen ECMAScript 6 / ECMAScript 2015 syntax such as arrows, let and const, a for...of loop, promises, or any of the many new library additions to built-in JS objects like Math and Number!

## Review

> Don't cheat. Answers are located upside-down on the reverse side of this README.

1. How might you explain what JSX is to your grandma?
2. What benefits does JSX offer?
3. What does the X in JSX stand for?
4. What is Babel?
5. At the end of a typical day, what React function does JSX boil down to after Babel works its transcompilation magic?

## References

> Look at the sites I plagiarized from.

- https://facebook.github.io/react/docs/introducing-jsx.html
- https://facebook.github.io/react/docs/jsx-in-depth.html
- https://facebook.github.io/react/docs/rendering-elements.html
- http://buildwithreact.com/tutorial/jsx
- https://medium.com/javascript-scene/jsx-looks-like-an-abomination-1c1ec351a918#.fpimf8pgf
- https://babeljs.io/
- https://www.quora.com/What-exactly-is-BabelJs-Why-does-it-understand-JSX-React-components
- http://codemix.com/blog/why-babel-matters
