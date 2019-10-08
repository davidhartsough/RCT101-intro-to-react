# Props

## Objective

> What will I learn?

The goal of this lesson is to learn what props are, why they're useful, and how they can be used.

## Overview

> What are they?

Props are just the configuration/options properties that every React component can receive. (Yup, props is probs short for properties.)
Remember how we conceptualized components like functions? Well, props are just the primary parameter of that function!
So props are to component input as elements are to component output. 
"Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called "props") and return React elements describing what should appear on the screen."
Props are a great way for a one-way communication between components. Parent components can pass information down to child components via props. Specifically, child components are given props via passing that information along through JSX attributes. At that point, the child component can access its props through its very own props JS object. That child component can do whatever it pleases with its props, but it *cannot* change its props. It's stuck with what it's got and is just gonna have to live with it.
"Props are read-only. Whether you declare a component as a function or a class, it must never modify its own props. All React components must act like pure functions with respect to their props."

Here's a little stretch of a metaphor, but, please, indulge me:
Personally, I like to conceptualize props as one of their dictionary definitions: "a portable object other than furniture or costumes used on the set of a play or movie." If you imagine your app to be a play, a movie, or even just a photograph, your UI is the scene, the actors are the components, and the props are the props. You're the director here, so you can give your actors whatever objects you wish them to hold and use, like a broom, a gun, a book, a glass, or a pair of chopsticks. 

## Purpose

> Why should I care? What problem(s) does this solve?

The value of props lie in their ability to open a one-way communication channel from parent components to child ones. Props function as a means for giving components input; they allow you to configure components and set their properties. You can pass down variables, data, and even more components!

For example, if you had a Greeting component, you might want one of its properties would be the name that should come after the jolly `"Hola, "`. To make your Greeting component greet Jimmy, then you should render it like so: `<Greeting name="Jimmy" />`. Then your Greeting component would render that name in its return element like so: `return <h1>Hola, {this.props.name}!</h1>;`.

## Walkthrough

> Let's get it!

It's Detailed Exploratory, Explanatory Application Examination Exercise Lab Activity Time!


Okay, so we left off with this snippet of JSX:

```jsx
    const MyComponent = () => {
      return <h1>Wow! This is a neat component.</h1>;
    };
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
    ReactDOM.render(
      <App />,
      document.getElementById('content')
    );
```

Let's just focus on the simple MyComponent first, so quickly change the first parameter of ReactDOM.render to `<MyComponent />`.
Now, for functional components, props are passed in as a parameter. All you have to do is as the `props` parameter to the definition of your function.

```jsx
    const MyComponent = (props) => {
      return <h1>Wow! This is a neat component.</h1>;
    };
    ReactDOM.render(
      <MyComponent />,
      document.getElementById('content')
    );
```

Are you ready? Here comes your first prop declaration! Since MyComponent seems to just be a description of itself, let's make the adjective in its sentence a configurable property. 
To do this, start off by adding a JSX attribute to its reference in the ReactDOM.render function. Call the attribute `adjective` and pass it something!

```jsx
    const MyComponent = (props) => {
      return <h1>Wow! This is a neat component.</h1>;
    };
    ReactDOM.render(
      <MyComponent adjective="nifty" />,
      document.getElementById('content')
    );
```

Remember how I said a component could access its props? That's right, props is nothing more than a JS object. Try grabbing the `adjective` property of your component's props object with a basic `console.log(props.adjective);`.

```jsx
    const MyComponent = (props) => {
      console.log(props.adjective);
      return <h1>Wow! This is a neat component.</h1>;
    };
    ReactDOM.render(
      <MyComponent adjective="nifty" />,
      document.getElementById('content')
    );
```

Time to make the switch! In order to replace "neat" with whatever adjective you pass in, you need to inject the prop into the JSX. To break open the typical XML syntax to make way for your JS writings, just use curly braces, like this: `{props.adjective}`.

```jsx
    const MyComponent = (props) => {
      return <h1>Wow! This is a {props.adjective} component.</h1>;
    };
    ReactDOM.render(
      <MyComponent adjective="nifty" />,
      document.getElementById('content')
    );
```

Quick! What would this look like as an ES6 class component? How would it access props then? If you guessed anything other than the use of the `this` object, then you haven't been reading thoroughly enough to realize that I already gave an example of this. I'm offended. Everyone else gets a gold star. Make a class component, and in the render function, grab your adjective prop from `{this.props.adjective}`.

```jsx
    class MyComponent extends React.Component {
      render() {
        return <h1>Wow! This is a {this.props.adjective} component.</h1>;
      }
    }
    ReactDOM.render(
      <MyComponent adjective="banger" />,
      document.getElementById('content')
    );
```

I can tell that you're getting pretty stoked right now, but this is just the beginning. What if I told you that you could pass entire components in your props? Well, I already told you. Hope you caught that one at least. Pay attention please.
What if I told you that you could pass entire components in props as child components intended to be rendered within a wrapper component? That I didn't tell you. But it's true.
In React Land there is a special prop with a reserved name: `children`. Unlike other props, the `children` prop is not defined as a JSX attribute. Instead, React automagically passes any components you nest inside your component element (opening and closing tags in JSX) into the `children` prop. Don't believe me? Then bet against me and see for yourself.

Start fresh and create a family of components: a Child component, a Parent component, and a GrandParent component.
For the Child component, stick to the familiar and just have it render some text in an `h1` element.
For the Parent component, make the root element a `div` but inside of it put `{this.props.children}`.
For the GrandParent component, make the root element a `Parent`. HI OH! Stay with me. Now, use the normal HTML opening and closing tag syntax with your `Parent` component like so `<Parent></Parent>`. You know what's coming next. I don't even have to tell you.
You guessed it! Pop a `Child` in the `Parent`! Got `<Parent><Child /></Parent>`? Add another!

```jsx
    class Child extends React.Component {
      render() {
        return <h1>Wow! Thanks props.</h1>;
      }
    }
    class Parent extends React.Component {
      render() {
        return (
          <div>
            {this.props.children}
          </div>
        );
      }
    }
    class GrandParent extends React.Component {
      render() {
        return (
          <Parent>
            <Child />
            <Child />
          </Parent>
        );
      }
    }
    ReactDOM.render(
      <GrandParent />,
      document.getElementById('content')
    );
```

Sorry if I blew your mind a little. But you're cruising along full speed. You're coming in hott at lightning speed. Why quit now? We're gonna push the limits.

## BONUS ROUND!

> B-b-b-b-bonus!

You've just unlocked the secret lesson in lesson 4 due to the absurd amount of **gold stars** you've earned!
Welcome to the bonus lesson. I've been expecting you. Step on in. The waters warm.
For this lesson, we're going to take you one step back in order to let you leap one great jump forward.
Go back to the beginning of lesson 4 when you created the simple functional component that accepted the adjective prop input. Put the `App` class component back in and have that be the root element to be rendered.

```jsx
    const MyComponent = (props) => {
      return <h1>Wow! This is a {props.adjective} component.</h1>;
    };
    class App extends React.Component {
      render() {
        return (
          <div>
            <MyComponent />
          </div>
        );
      }
    }
    ReactDOM.render(
      <App />,
      document.getElementById('content')
    );
```

Go ahead and put a few MyComponent elements inside the App component and give them each unique adjective props. Next, add another `div` element inside the App component's root `div` component, and put `{this.props.children}` inside of that.

```jsx
    const MyComponent = (props) => {
      return <h1>Wow! This is a {props.adjective} component.</h1>;
    };
    class App extends React.Component {
      render() {
        return (
          <div>
            <MyComponent adjective="rad" />
            <MyComponent adjective="super" />
            <MyComponent adjective="dank" />
            <div>
              {this.props.children}
            </div>
          </div>
        );
      }
    }
    ReactDOM.render(
      <App />,
      document.getElementById('content')
    );
```

On one of the MyComponent elements, change the adjective JSX attribute to be `adjective={this.props.adjective}`. How do you think we could populate that? Duh! Too easy for you. Yeah, just add the JSX attribute for `adjective` to the `<App />` element declaration. 
To take this another step even further, open up the `<App />` element to have a child `p` element with some text inside. It should look like this:

```jsx
    const MyComponent = (props) => {
      return <h1>Wow! This is a {props.adjective} component.</h1>;
    };
    class App extends React.Component {
      render() {
        return (
          <div>
            <MyComponent adjective="rad" />
            <MyComponent adjective="super" />
            <MyComponent adjective={this.props.adjective} />
            <div>
              {this.props.children}
            </div>
          </div>
        );
      }
    }
    ReactDOM.render(
      <App adjective="dope"><p>Hey look at me! I am a child in a p tag.</p></App>,
      document.getElementById('content')
    );
```

See how the adjective "dope" was passed down twice before finally being rendered? Also, keep in mind that you don't need quotes around the value of a JSX attribute if the value is a snippet of JS inside curly braces. (AKA `adjective={this.props.adjective}`.) 
But now you're saying, "Hey! None of this is new. It's just an interesting spin on what I just learned! Gimme the bonus goodness already!"
You're right. You deserve it. Let's get our hands dirty. Let's talk more about extending our App class component.
I'm sure you're familiar with classes already, so why don't we just hop in and add a constructor method that takes props as its parameter and then calls the super method with those props.

```jsx
    const MyComponent = (props) => {
      return <h1>Wow! This is a {props.adjective} component.</h1>;
    };
    class App extends React.Component {
      constructor(props) {
        super(props);
      }
      render() {
        return (
          <div>
            <MyComponent adjective="rad" />
            <MyComponent adjective="super" />
            <MyComponent adjective={this.props.adjective} />
            <div>
              {this.props.children}
            </div>
          </div>
        );
      }
    }
    ReactDOM.render(
      <App adjective="dope"><p>Hey look at me! I am a child in a p tag.</p></App>,
      document.getElementById('content')
    );
```

But that's not good enough for you. The hot tubs too hot. The hot dogs are too small. Not enough ketchup. 
Then let's turn it up to eleven! Let's get interactive. OH YEAH. You know what that means: click handlers baby!
And while we're at it, we're gonna dine on some delicious ES6 syntax. Mmmmmhmmm.
Step one: Add a method to the App component class called `handleClick()`.
Step two: Make the method do something like `console.log('Congrats! You clicked!');`.
Step three: Add an `onClick` JSX attribute to the root `div` element that calls your `handleClick` class method.
And don't be silly with it. Make it sexy. Don't you dare write the word `function` when you can just use the new parentheses and arrow business that everyone's been raving about. Now's your chance. This is the moment we've all been waiting for. Let's see it!

```jsx
    const MyComponent = (props) => {
      return <h1>Wow! This is a {props.adjective} component.</h1>;
    };
    class App extends React.Component {
      constructor(props) {
        super(props);
      }
      handleClick() {
        console.log('Congrats! You clicked!');
      }
      render() {
        return (
          <div onClick={() => this.handleClick()}>
            <MyComponent adjective="rad" />
            <MyComponent adjective="super" />
            <MyComponent adjective={this.props.adjective} />
            <div>
              {this.props.children}
            </div>
          </div>
        );
      }
    }
    ReactDOM.render(
      <App adjective="dope"><p>Hey look at me! I am a child in a p tag.</p></App>,
      document.getElementById('content')
    );
```

It's absolutely beautiful. You've done it again. This bonus round has nothing left for you. The well's run dry, and oh how we've drunk!

## Review

> Don't cheat. Answers are located upside-down on the reverse side of this README.

1. How might you explain what props are to your grandma?
2. What benefits do props offer?
3. How do you break open JSX XML syntax to write a snippet of JS inside?
4. What is the name of the special reserved prop that is the only prop to not be defined by a JSX attribute, and why is it awesome?
5. What... is the air-speed velocity of an unladen swallow?

## References

> Look at the sites I plagiarized from.

- https://facebook.github.io/react/docs/components-and-props.html
- https://facebook.github.io/react/docs/thinking-in-react.html
- http://buildwithreact.com/tutorial/components
- https://github.com/uberVU/react-guide/blob/master/props-vs-state.md
- https://themeteorchef.com/tutorials/understanding-props-and-state-in-react
