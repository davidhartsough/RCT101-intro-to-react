# React

## Goals of this lesson

The goal of this lesson is to introduce React at the highest possible level.

## Wat it is

Here's what the peeps who made the thing say:

"REACT: A JAVASCRIPT LIBRARY FOR BUILDING USER INTERFACES"

"React is a declarative, efficient, and flexible JavaScript library for building user interfaces."

"Usually the V in MVC."

You: "Well gee Facebook devs, I sure do need to build me a user interface. But what the hay do you mean by declarative?"

Google: "Declarative: denoting high-level programming languages that can be used to solve problems without requiring the programmer to specify an exact procedure to be followed."

Wikipedia (paraphrased): It is not imperative. (Imperative programming is a programming paradigm that uses statements that change a program's state.)
A declarative program describes what computation should be performed and not how to compute it.

Other internet friends: Think HTML. You describe the elements that you want to make up UI via HTML. React isn't much different.

## Why

- React is declarative, this means that you don't worry about mucking with the DOM, instead you focus on data and the 
component renders that data into the DOM for you. No more `$('.some-list').forEach(...)` 👏 .
- React gives you a one-way data flow, this means that you'll spend less time trying to find the source of truth (is it
 in the markup, is it in the javascript?) and more time focusing on more interesting problems.
- React allows you to conceptualize your application in a more modular way. React has a construct called components,
think of components like lego blocks; reusable, simple, self contained pieces of functionality that you can connect 
together to make more complex behavior if you see fit.
- Last but certainly not least, React makes testing a breeze. Components should behave just like
functions, data is passed in and the same ui comes out, every, single, time. With this in mind you can more easily 
create unit tests for your ui.

Sold yet? You buying into this? Still not convinced? Well... You're reading through this for some reason. You might as
well just try it out before leaving.

## Let's get started

Check this out.
We can get up and rollin' with React in less than ten lines in the body element of a simple index.html file.
Like so:
```html
  <body>
    <div id="content"></div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.3.1/react.js" type="text/javascript"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.3.1/react-dom.js" type="text/javascript"></script>
    <script type="text/javascript">
      ReactDOM.render(
        React.createElement('h1', null, 'Wow! Thanks React.'),
        document.getElementById('content')
      );
    </script>
  </body>
```

You: "HOLD UP. What's this `react-dom.js` business doing in here? You thought you could just sneak in another 
dependencies/library without me seeing?!"

Me: "You caught me. Lemme explain. ReactDOM connects React to the DOM."

You: "Wow. Thanks. Couldn't have figured that out from the name..."

Me: "Hey now, no need to get sarcastic. The ReactDOM library is really as simple as that. It has very few functions 
that you will utilize. In fact, most of the time, the only thing you will use ReactDOM for is its `render` function. The 
`render` function takes two parameters (and an optional callback): (1) the React element you would like to be rendered 
and (2) the container (a DOM element) you would like your React element to be rendered in. It's kinda like 
[node.appendChild](https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild) or 
[$('.some-selector').append(...)](http://api.jquery.com/append/)."

So in this example, we find the `div` element in the DOM with the `id` of 'content' and insert a new React element into 
it. For a classic introductory example, we created an `h1` element that says "Wow! Thanks React." To see this live, just
 open the index.html in your browser (or click and drag the file to your browser).

You can now check "Use ReactJS" off your bucket-list and add "Used ReactJS" to your resumé, CV, LinkedIn, etc.

## Up next

What do you think?

I agree with you. This doesn't demonstrate the sheer power that lies within that `react.js` file that's over 150 KB. 
We both know there's way more involved. Let's dig deeper now that you've had a taste. Head on over to 
[lesson 2](../02-jsx/README.md).
