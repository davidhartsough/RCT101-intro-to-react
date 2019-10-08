# Components

## Objective

> What will I learn?

The goal of this lesson is to learn what components are, why they're useful, and how they can be defined.

## Overview

> What are they?

Components are detailed elements you define with React that are the building blocks of your UI.
"Components let you split the UI into independent, reusable pieces, and think about each piece in isolation."
At the end of the day, React helps you build your whole UI for your app in one Component that contains several other Components therein (which also have their own Components, and so on). For example, a calendar app might have a Month Component that has some Week Components that have Day Components that have Event Components that have Event Detail Components and Action Button Components. BOOM. There's your calendar app UI built with reusable components.
So all in all, Components encapsulate element trees and return an element tree as their output.
"The returned element tree can contain both elements describing DOM nodes, and elements describing other components. This lets you compose independent parts of UI without relying on their internal DOM structure."

A component is either written as a function or as an ES6 class, but in either case, it should return a React element when called upon via JSX syntax. When you make a component as an ES6 class, you'll extend the class using React's Component class, which gives you access to some extra fun little function for performing custom logic related to the component's lifecycle (such as when the corresponding DOM node is created or destroyed). We'll get more into all that in later lessons, but for now, just realize that, "when a component is defined as a class, it is a little bit more powerful than a functional component." Functional components are simple and straightforward because they're only focused on defining the element tree that you want to render.
"However, whether functions or classes, fundamentally they are all components to React. They [just] return the elements as their output."

## Purpose

> Why should I care? What problem(s) does this solve?

The value of Components lie in their reusability and individuality. Conceptually, they are just functions, and their function is to give you an element tree. In the calendar app example, the data your app wants to focus on and handle are Events, so let's say you have an array of Events available to you that you want to render in your calendar app. All you have to do is create a single Event Component that defines and describes the elements that make up that bit of the UI. Then you'd simply map through your array of events and render each event using your new Event Component. Now if you wanted to change the click handler on the events, you'd just find your bit of code for the Event Component (which should be in its own file) and change the JavaScript right there. BOOM! All events will function according to the new change.

## Walkthrough

> Let's check it out!

It's Detailed Exploratory, Explanatory Application Examination Exercise Lab Activity Time! (Super trendy, I know. It's all over the internet. Look it up.)

Okay, so we left off with this snippet of JSX:

```jsx
    const element = <h1>Wow! Thanks Babel.</h1>;
    ReactDOM.render(
      element,
      document.getElementById('content')
    );
```

Let's make our header element into a React Component. First we'll start simple by creating the component as a function. 
All we have to do is write a function that returns that same element. (We'll just touch up the text accordingly, too.) 

```jsx
    function MyComponent() {
      return <h1>Wow! This is a neat component.</h1>;
    }
```

That was easy. It makes sense, too, because watch this:
You can change your `const element` variable to be equal to the function now.

```jsx
    function MyComponent() {
      return <h1>Wow! This is a neat component.</h1>;
    }
    const element = MyComponent();
    ReactDOM.render(
      element,
      document.getElementById('content')
    );
```

And this should work beautifully. However, an even simpler way to accomplish this is to use the magic of JSX and reference our new component as if it were just like any other HTML element. This is where that XML-like code comes into your JS again.

```jsx
    function MyComponent() {
      return <h1>Wow! This is a neat component.</h1>;
    }
    ReactDOM.render(
      <MyComponent />,
      document.getElementById('content')
    );
```

Wow. That sure is neat, eh? You've just written your first React component! That's pretty rad.
Don't stop now. Let's use our handy-dandy ES6 arrow function syntax to spruce this up.

```jsx
    const MyComponent = () => {
      return <h1>Wow! This is a neat component.</h1>;
    };
    ReactDOM.render(
      <MyComponent />,
      document.getElementById('content')
    );
```

Killer. Now why not make this thing into one of those fancy ES6 classes like I mentioned earlier? Make sure to extend the React.Component class, because you need its render method to make this work.

```jsx
    class MyComponent extends React.Component {
      render() {
        return <h1>Wow! This is a neat component.</h1>;
      }
    }
```

WHOA! You're unstoppable. This train has left the newbie station.
See how the functional component and the class component are virtually the same? Don't worry; we'll talk lots more about the other methods you get when your class extends from the React.Component class in a jiffy. For now, let's try one last thing to wrap up this lesson. 
I need you to breathe steadily and create a second component. (Just duplicate your first element via the ole copy-paste process.) We'll call this new component `App`. While you're at it, change your ReactDOM.render function to render `<App />`, too.
Got it? Okay, now I need you to take the `h1` element and wrap it with parentheses, leaving the semi-colon outside the parentheses, of course. 
After that, drop the `h1` element onto a new line. (Hit your return key when your cursor is just after the open parenthesis and then again when its just before the closing parenthesis.)
Here's how your new component should look now:

```jsx
    class App extends React.Component {
      render() {
        return (
          <h1>Wow! This is a neat component.</h1>
        );
      }
    }
    ReactDOM.render(
      <App />,
      document.getElementById('content')
    );
```

We're about to open you up to the wild world of component making with JSX. And all you needed was a couple of parentheses to get you in gear. Those parentheses allow you to write a whole bunch of JSX without needing a bunch of semi-colons at the end of every line. Since JavaScript does some automatic semi-colon insertion, these parentheses are necessary for you to write multiple lines without JS getting confused. Now you can hit your return key all day and keep adding new lines like a mad man... but I don't recommend that. Just to get it out of your system, why not try wrapping your `h1` element in a `div` element, but keep the `h1` element on its own line, nested and indented within the new div. 

```jsx
    class App extends React.Component {
      render() {
        return (
          <div>
            <h1>Wow! This is a neat component.</h1>
          </div>
        );
      }
    }
```

Okay, we're about to get crazy, so fasten your seatbelts.
I want you to add your first component into your new App component. However, it's important for you to note now that Components must return a single root element, so you cannot have a component just be two separate `div` or `h1` elements. For example, this is an **invalid** component:

```jsx
    class TotallyInvalidComponent extends React.Component {
      render() {
        return (
          <h1>Gee! This sure is a broken component.</h1>
          <h1>Huh! It won't work at all.</h1>
        );
      }
    }
```

However, all you need to do to save that poor component is to wrap it in a `div` element, allowing that new `div` to be the root element for the component. (You can obviously use any element, not just a `div`.)
Back to the task at hand! Add your first component into the mix!

```jsx
    class App extends React.Component {
      render() {
        return (
          <div>
            <MyComponent />
            <h1>Wow! This is a neat component.</h1>
          </div>
        );
      }
    }
```

Oh snap! Keep doin' what you're doin'! Go nuts!

```jsx
    class App extends React.Component {
      render() {
        return (
          <div>
            <MyComponent />
            <p>Extra! Extra!</p>
            <MyComponent />
            <MyComponent />
          </div>
        );
      }
    }
```

Look at what you've done. Now you've got a root React component being rendered to the DOM called App that has a handful of other elements inside it, including another component you made called MyComponent, which you've used THREE times!
Cheers, mate.

## Challenge

> I double dog dare you.

Bet you can't go even deeper and nest another, new component inside MyComponent! Just kidding. I still believe in you!

Also, why not try putting two or more different components inside your App component? Lemme know how that goes.

## Review

> Don't cheat. Answers are located upside-down on the reverse side of this README.

1. How might you explain what React components are to your grandma?
2. What benefits do React components offer?
3. What are two ways you can write a React component?
4. How can you define the element tree output of your component on multiple lines without running into issues with JavaScript's automatic semi-colon insertion?
5. How many elements can you have at the root of your component's tree?

## References

> Look at the sites I plagiarized from.

- https://facebook.github.io/react/docs/components-and-props.html
- https://facebook.github.io/react/docs/react-component.html
- https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html
- http://buildwithreact.com/tutorial/components
