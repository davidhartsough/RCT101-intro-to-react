# Lifecycle

## Objective

> What will I learn?

The goal of this lesson is to learn what the different lifecycle methods of a React component class are, why they're useful, and how to use them!

## Overview

> What is it?

Lifecycle is the series of changes in the life of an organism, including reproduction (which completes and perpetuates the cycle). Duh.
Any other questions?

OH! Right... Sorry. Wrong lessons. Here we are; React. Yes. Very good. 

Today we are discussing the lifecycle of a React component. The component lifecycle consists of three primary stages of existence: (1) Mounting, (2) Updating, and (3) Unmounting. AKA Birth, Life, and Death. In your UI, your components may not go through all these stages of life; some may only ever mount because they are purely presentational and the data they are presenting never changes; others may only ever mount and then update when data changes but never experience unmounting. Some components are forever...eternal. Do those components ever unmount? Many philosophers and theologians have long-questioned the after-life of components. No one knows what happens to them when your browser closes. More problematic still: what happens to React components when the browser crashes? Do React components have a soul?

Here's a (near) copy and paste from Facebook's React doc entitled React.Component:

### Mounting
These methods are called when an instance of a component is being created and inserted into the DOM:

#### constructor()
`constructor(props)`
The constructor for a React component is called before it is mounted.
Remeber to call `super(props)` before any other statement.

The constructor is the right place to initialize state. If you don't initialize state and you don't bind methods, you don't need to implement a constructor for your React component.

It's okay to initialize state based on props. This effectively "forks" the props and sets the state with the initial props.

However, beware of this pattern, as state won't be up-to-date with any props update. Instead of syncing props to state, you often want to lift the state up.

#### componentWillMount()
`componentWillMount()`
This method is invoked immediately before mounting occurs (before `render()` is called). Setting state synchronously in this method will not trigger a re-rendering. Avoid introducing any side-effects or subscriptions in this method.

This is the only lifecycle hook called on server rendering. Generally, we recommend using the `constructor()` instead.

#### render()
`render()`
Render is the only method that is required. When called, it should examine `this.props` and `this.state` and return a single React element. 

If you don't want anything to render, simply return `null` or `false`.

The `render()` function should be pure, meaning that it does not modify component state, it returns the same result each time it's invoked, and it does not directly interact with the browser. If you need to interact with the browser, perform your work in `componentDidMount()` or the other lifecycle methods instead.

#### componentDidMount()
`componentDidMount()`
This is invoked immediately after a component is mounted. Initialization that requires DOM nodes should go here. If you need to load data from a remote endpoint, this is a good place to instantiate the network request. Setting state in this method will trigger a re-rendering.

### Updating
An update can be caused by changes to props or state. These methods are called when a component is being re-rendered:

#### componentWillReceiveProps()
`componentWillReceiveProps(nextProps)`
This is invoked before a mounted component receives new props. If you need to update the state in response to prop changes (for example, to reset it), you may compare this.props and nextProps and perform state transitions using this.setState() in this method.

Note that React may call this method even if the props have not changed, so make sure to compare the current and next values if you only want to handle changes. This may occur when the parent component causes your component to re-render.

React doesn't call componentWillReceiveProps with initial props during mounting. It only calls this method if some of component's props may update. Calling this.setState generally doesn't trigger componentWillReceiveProps.

#### shouldComponentUpdate()
`shouldComponentUpdate(nextProps, nextState)`
This is invoked before rendering when new props or state are being received.
This method needs to return a boolean. By default, this returns `true`, and when this happens, `componentWillUpdate()`, `render()`, and `componentDidUpdate()` will fire. If you return `false`, those lifecycle methods will not be triggered.
If you are confident you want to write it by hand, you may compare this.props with nextProps and this.state with nextState and return false to tell React the update can be skipped.

The default behavior is to re-render on every state change, and in the vast majority of cases you should rely on the default behavior.
Notes: 
 - This method is not called for the initial render or when forceUpdate() is used.
 - Returning false does not prevent child components from re-rendering when their state changes.
 - In the future, React may treat shouldComponentUpdate() as a hint rather than a strict directive, and returning `false` may still result in a re-rendering of the component.

#### componentWillUpdate()
`componentWillUpdate(nextProps, nextState)`
This is invoked immediately before rendering when new props or state are being received (and if and only if `shouldComponentUpdate()` returns `true`). Use this as an opportunity to perform preparation before an update occurs. This method is not called for the initial render.

Note that you cannot call `this.setState()` here. If you need to update state in response to a prop change, use `componentWillReceiveProps()` instead.

#### render()
This is the same as explained before; except this time, it will not be invoked if `shouldComponentUpdate()` returns `false`.

#### componentDidUpdate()
`componentDidUpdate(prevProps, prevState)`
This is invoked immediately after updating occurs (and if and only if `shouldComponentUpdate()` returned `true`). This method is not called for the initial render.

Use this as an opportunity to operate on the DOM when the component has been updated. This is also a good place to do network requests as long as you compare the current props to previous props (e.g. a network request may not be necessary if the props have not changed).

### Unmounting
This method is called when a component is being removed from the DOM:

#### componentWillUnmount()
`componentWillUnmount()`
This is invoked immediately before a component is unmounted and destroyed. Perform any necessary cleanup in this method, such as invalidating timers, canceling network requests, or cleaning up any DOM elements that were created in `componentDidMount`.

## Purpose

> Why should I care? What problem(s) does this solve?

The value of understanding the lifecycle of methods lies in the associated methods that you can access when you extend the React Component class. Those methods can serve several functions (as described above). Here are some prime examples that demonstrate the usefulness of lifecycle methods:

Let's say you are building an app that will render information about data that you need to fetch from your database. (Wowie, David! I sure can relate to this example!) You have all your UI nicely designed by a cool team of researchers and designers. Thus far, you've broken down the UI into nice components and created React Components respectively. Now you're wanting to play with some data and put it in the state of your top-level component. When should make the call to the server? At what in your code should you throw in that handy-dandy call to your remote JSON endpoint? If you said "Well gee, that sounds like the Updating stage of a component's life, so I'd probably do that in `componentDidMount()`!" then please proceed to pat yourself on the back while rubbing your stomach in a circle. (Challenging, eh? Switch hands and try again!) Once you've acquired some data (on success), you can set your state with `this.setState({data})`. NEAT.

Now let's take this example a step further. Let's say you're building a small component as part of a larger app that might be only one section of your UI. However, this component need to subscribe to certain events that you trigger in other parts of your app in order to ensure that this small component is displaying accurate data that is dynamic and synced in real-time. When do you want to subscribe? At what point in your component's code do you want to place your event listener? "Easy! Just use `componentDidMount()` again! What's new about this example?" NOICE! Glad you asked! Now you have a small part of your UI as a dedicated React component subscribed to an event or two. What happens when you're done with this small component and need to close/remove it? Are you just gonna leave that event listener hangin' around all willy-nilly? Nah, silly. You gotta do a little clean-up! So where might you want to unsubscribe? At what point in your React component should you remove event listeners? "`componentWillUnmount()`?" BINGO.

## Walkthrough

> Let's check it out!

It's Detailed Exploratory, Explanatory Application Examination Exercise Lab Activity Time!


Okay, let's go back to where we left off in lesson 05-state with this snippet of JSX:

```jsx
    class MyComponent extends React.Component {
      constructor(props) {
        super(props);
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
    ReactDOM.render(
      <MyComponent />,
      document.getElementById('content')
    );
```

Looking back at this, now you can recognize that we've already been using two methods that are a part of a React compenent's lifecycle: `constructor()` (which is a part of Mounting) and `render()` (which is a part of both Mounting and Updating). But let's explore all the other lifecycle methods. The simplest way to do this is to simply add them all in (in order) and to throw a `console.log()` in each. Remember to `return true` in `shouldComponentUpdate()`.

```jsx
    class MyComponent extends React.Component {
      constructor(props) {
        super(props);
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
      componentWillMount() {
        console.log('triggered componentWillMount');
      }
      componentWillReceiveProps(nextProps) {
        console.log('triggered componentWillReceiveProps');
      }
      shouldComponentUpdate(nextProps, nextState) {
        console.log('triggered shouldComponentUpdate');
        return true;
      }
      componentWillUpdate(nextProps, nextState) {
        console.log('triggered componentWillUpdate');
      }
      render() {
        console.log('triggered render');
        const clickedStatus = (this.state.hasBeenClicked) ? 'definitely' : 'never';
        return (
          <h1 onClick={() => this.handleClick()}>
            Wow! I have <em>{clickedStatus}</em> been clicked.
          </h1>
        );
      }
      componentDidMount() {
        console.log('triggered componentDidMount');
      }
      componentDidUpdate(prevProps, prevState) {
        console.log('triggered componentDidUpdate');
      }
      componentWillUnmount() {
        console.log('triggered componentWillUnmount');
      }
    }
```

Go ahead and throw this in the browser and check out the console output! Cool beans. 

But to give you a better idea of these lifecycle's let's incorporate some component interactions—let's put in a child component. First, copy and paste your component so you don't have to write out all those methods again. Next, call the original component `Parent` and the new component `Child` (update the first parameter of `ReactDOM.render`). Now, simplify the `Child` by removing the click handler and any mention of state (`handleClick()`, `constructor()`, `onClick`, and `clickedStatus`). Also, instead of completely removing `clickedStatus` from within render, just add `this.props.` before it. Here's what your `Child` component should look like thus far:

```jsx
    class Child extends React.Component {
      constructor(props) {
        super(props);
      }
      componentWillMount() {
        console.log('triggered componentWillMount');
      }
      componentWillReceiveProps(nextProps) {
        console.log('triggered componentWillReceiveProps');
      }
      shouldComponentUpdate(nextProps, nextState) {
        console.log('triggered shouldComponentUpdate');
        return true;
      }
      componentWillUpdate(nextProps, nextState) {
        console.log('triggered componentWillUpdate');
      }
      render() {
        console.log('triggered render');
        return (
          <h1>
            Wow! I have <em>{this.props.clickedStatus}</em> been clicked.
          </h1>
        );
      }
      componentDidMount() {
        console.log('triggered componentDidMount');
      }
      componentDidUpdate(prevProps, prevState) {
        console.log('triggered componentDidUpdate');
      }
      componentWillUnmount() {
        console.log('triggered componentWillUnmount');
      }
    }
```

To clarify what's happening in your console logs, add "Parent" after "triggered" for all the logging happening in the `Parent` and add "Child" after "triggered" for all the logging happening in the `Child`. Sweetness. The only problem now is that the `Child` is never rendered. So let's tweak it a bit to replace the `<em>` element in the `Parent`. All you need to do is make the `<em>` element the only element returned from the `render` in `Child`. Then replace the `<em>{clickedStatus}</em>` in the `render` in `Parent` with `<Child clickedStatus={clickedStatus} />`.

Here's the whole thing!

```jsx
    class Child extends React.Component {
      constructor(props) {
        super(props);
      }
      componentWillMount() {
        console.log('triggered Child componentWillMount');
      }
      componentWillReceiveProps(nextProps) {
        console.log('triggered Child componentWillReceiveProps');
      }
      shouldComponentUpdate(nextProps, nextState) {
        console.log('triggered Child shouldComponentUpdate');
        return true;
      }
      componentWillUpdate(nextProps, nextState) {
        console.log('triggered Child componentWillUpdate');
      }
      render() {
        console.log('triggered Child render');
        return <em>{this.props.clickedStatus}</em>;
      }
      componentDidMount() {
        console.log('triggered Child componentDidMount');
      }
      componentDidUpdate(prevProps, prevState) {
        console.log('triggered Child componentDidUpdate');
      }
      componentWillUnmount() {
        console.log('triggered Child componentWillUnmount');
      }
    }
    class Parent extends React.Component {
      constructor() {
        super();
        this.state = {
          hasBeenClicked: false
        };
      }
      componentWillMount() {
        console.log('triggered Parent componentWillMount');
      }
      componentWillReceiveProps(nextProps) {
        console.log('triggered Parent componentWillReceiveProps');
      }
      shouldComponentUpdate(nextProps, nextState) {
        console.log('triggered Parent shouldComponentUpdate');
        return true;
      }
      componentWillUpdate(nextProps, nextState) {
        console.log('triggered Parent componentWillUpdate');
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
            Wow! I have <Child clickedStatus={clickedStatus} /> been clicked.
          </h1>
        );
      }
      componentDidMount() {
        console.log('triggered Parent componentDidMount');
      }
      componentDidUpdate(prevProps, prevState) {
        console.log('triggered Parent componentDidUpdate');
      }
      componentWillUnmount() {
        console.log('triggered Parent componentWillUnmount');
      }
    }
    ReactDOM.render(
      <Parent />,
      document.getElementById('content')
    );
```

Check out dat console, yo! It's beautiful--the culmination of your work.

## Challenge

> I double dog dare you.

Bet you can't use some of those parameters passed into to these lifecycle methods to do something crazy cool! (Actually, I bet you can, but I'm trying to provoke you.)

## Review

> Don't cheat. Answers are located upside-down on the reverse side of this README.

1. How might you explain the lifecycle of a React component to your grandma?
2. What benefits do lifecycle methods offer?
3. In which lifecycle method should you initialize the default state for a component?
4. In which lifecycle method should you fetch data and/or subscribe to events?
5. In which lifecycle method should you "clean up" and remove event listeners?
6. What is the only lifecycle method that is required?
7. Which lifecycle methods take parameters?
8. Which lifecycle method must return a boolean?
9. Which lifecycle methods are invoked when props are updated?
10. Which lifecycle method should you use if you need to interact with the browser?

## References

> Look at the sites I plagiarized from.

- https://facebook.github.io/react/docs/react-component.html
- https://facebook.github.io/react/docs/state-and-lifecycle.html
- http://busypeoples.github.io/post/react-component-lifecycle/
- https://engineering.musefind.com/react-lifecycle-methods-how-and-when-to-use-them-2111a1b692b1
