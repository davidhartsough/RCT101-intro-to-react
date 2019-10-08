# State

## Objective

> What will I learn?

The goal of this lesson is to learn what state is, why it's useful, and how to instantiate and set state.

## Overview

> What is it?

State is an optional, private object that any component can initialize and manage internally to determine behavior and allow for dynamicity and interactivity. Just like props, state is also just a plain JS object. However, unlike props, "state is reserved only for interactivity, that is, data that changes over time." 
State can be mutated, often by user interactivity, but all state management happens internally within its respective component. The component class is the only manager over its own state business. 
"State is similar to props, but it is private and fully controlled by the component." 
"We mentioned before that components defined as classes have some additional features. Local state is exactly that: a feature available only to classes." 
"The key difference between props and state is that state is internal and controlled by the component itself while props are external and controlled by whatever renders the component." 

## Purpose

> Why should I care? What problem(s) does this solve?

The value of state lies in its dynamicity and interactivity. Its function is to manage the current conditions of its component. While props are immutable, state is mutable within its respective component class via a method called (you guessed it) `setState()`. 
"To make your UI interactive, you need to be able to trigger changes to your underlying data model. React makes this easy with state." 
If you ever think you might need alter the data you are passing down to the component, then it should be given to state instead of props. 

## Walkthrough

> Let's check it out!

It's Detailed Exploratory, Explanatory Application Examination Exercise Lab Activity Time!

Okay, so we left off with this snippet of JSX:

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

Thank goodness you got all those gold stars, unlocked the ultra-secret bonus round, and spent all that time building up a fancy little class component with a click handler to boot! That's gonna save us so much time now for this lesson. 
Seriously. Now all we need to do to add state is just initialize it in the constructor as a new, plain ole JS object.

```jsx
    class App extends React.Component {
      constructor(props) {
        super(props);
        this.state = {};
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
```

Now I'm gonna leave you hanging for a little while. I'm such a tease, I know!
First let's trim some fat to hone down on the bones of what we've got before we demonstrate the power of state. 
Go ahead and get rid of the functional component and just make your class component render a familiar `h1` element. (Get rid of the excess in the ReactDOM.render function too please.)

```jsx
    class App extends React.Component {
      constructor() {
        super();
        this.state = {};
      }
      handleClick() {
        console.log('Congrats! You clicked!');
      }
      render() {
        return (
          <h1 onClick={() => this.handleClick()}>
            Wow! I am a perfect candidate for state.
          </h1>
        );
      }
    }
    ReactDOM.render(
      <App />,
      document.getElementById('content')
    );
```

To demonstrate state, let's make this component that describes itself describe whether or not it has ever been clicked before. We'll want it to either tell us "Wow! I have *never* been clicked." or "Wow! I have *definitely* been clicked." To do this, add a `const` variable in the render method called `clickedStatus`, and for now, make it say 'never'. Next, change the text accordingly and put in an `em` element for emphasis around the `clickedStatus` string. 

```jsx
    class App extends React.Component {
      constructor() {
        super();
        this.state = {};
      }
      handleClick() {
        console.log('Congrats! You clicked!');
      }
      render() {
        const clickedStatus = 'never';
        return (
          <h1 onClick={() => this.handleClick()}>
            Wow! I have <em>{clickedStatus}</em> been clicked.
          </h1>
        );
      }
    }
```

Ready to use state? Here goes nothing!
Expand the state object to include a property called `hasBeenClicked` (which we will treat as a boolean) and initialize that property to be false at first. After that, let's use a conditional ternary operator to set the `const clickedStatus` to be either 'definitely' or 'never' based on whether or not `this.state.hasBeenClicked` is true or false.

```jsx
    class App extends React.Component {
      constructor() {
        super();
        this.state = {
          hasBeenClicked: false
        };
      }
      handleClick() {
        console.log('Congrats! You clicked!');
      }
      render() {
        const clickedStatus = (this.state.hasBeenClicked) ? 'definitely' : 'never';
        return (
          <h1 onClick={() => this.handleClick()}>
            Wow! I have <em>{clickedStatus}</em> been clicked.
          </h1>
        );
      }
    }
```

Time for mutation! Remember how to change state? Let's put that `setState()` biddy in your click handler method. In its simplest use, the `setState()` method takes a JS object as a parameter, and inside that object, you can set corresponding properties of the state object to any values you choose. In this case, you want to switch the `hasBeenClicked` property to true! 
Go ahead. Give it a try. Click it. Go on now. It won't bite.

```jsx
    class App extends React.Component {
      constructor() {
        super();
        this.state = {
          hasBeenClicked: false
        };
      }
      handleClick() {
        console.log('Congrats! You clicked!');
        this.setState({
          hasBeenClicked: true
        });
      }
      render() {
        const clickedStatus = (this.state.hasBeenClicked) ? 'definitely' : 'never';
        return (
          <h1 onClick={() => this.handleClick()}>
            Wow! I have <em>{clickedStatus}</em> been clicked.
          </h1>
        );
      }
    }
```

You should see your face now. You've got the same exact expression now that you had when you first saw a semi-truck transform into Optimus Prime.
That's it! You have successfully implemented state into a component class.
You've also made it halfway through the RCT101 lessons! And I must say, you've been quite the rockstar, but don't tell any of the other people who have gone through these lessons that. They might get jealous.

## Challenge

> I double dog dare you.

Obviously there are a million new things you could try with state now under your belt, so play around for little while longer. But then go take a well-deserved break! 
Stretch them limbs. Smell the flowers. Give somebody a high-five!

## Review

> Don't cheat. Answers are located upside-down on the reverse side of this README.

1. How might you explain what state is to your grandma?
2. What benefits does state offer?
3. Who manages a component's state?
4. What is the difference between state and props?
5. What question can you ask yourself if you are stuck trying to decide whether or not you need to add state to your component?

## References

> Look at the sites I plagiarized from.

- https://facebook.github.io/react/docs/state-and-lifecycle.html
- https://facebook.github.io/react/docs/thinking-in-react.html
- https://github.com/uberVU/react-guide/blob/master/props-vs-state.md
- https://thinkster.io/tutorials/understanding-react-state
- http://buildwithreact.com/tutorial/state
