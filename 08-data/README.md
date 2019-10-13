# Data

## Objective

> What will I learn?

The goal of this lesson is to learn how to tie data to your React components. #DataBindingFTW

## Overview

> What is it?

It's data! Nothing new here to see here. Carry on.

(Oh and there's no need for a "Purpose" section here either!)

## Just do it

> GET TO THE CHOPPER!

Let's build a Color List that will display a list of colors and color each item accordingly. We can use this example to fake a request for JSON data. In this exercise we'll say that the JSON data is just a list of colors, each with a name and an HSL value.

Go ahead and start off fresh with a new snippet of JSX:

```jsx
    class ColorList extends React.Component {
      constructor(props) {
        super(props);
      }
      render() {
        return (
          <ul style={{fontSize: 48}}>
            <li style={{color: 'hsl(0, 100%, 50%)'}}>red</li>
          </ul>
        );
      }
    }
    ReactDOM.render(
      <ColorList />,
      document.getElementById('content')
    );
```

Now let's create some fake data outside the React component and just put it into a variable for this example.

```jsx
const colorJSON = '[{"name": "red", "hsl": "hsl(0, 100%, 50%)"}, {"name": "blue", "hsl": "hsl(240, 100%, 50%)"}, {"name": "purple", "hsl": "hsl(270, 100%, 50%)"}]';
```

In this example we won't be manipulating this data at all, but we know that we will want to allow the user to edit it in some future version of this app. So let's go ahead and stick this data into the `ColorList` component's state.
To do this, first we need to initialize a default state in the `constructor`. Let's call this `colorData` and assume that it will be an array:

```jsx
      constructor(props) {
        super(props);
        this.state = {
          colorData: []
        };
      }
```

With that all set, it's time to fake a data fetch! Thinking back to the last lesson, where should we make a call to our servers for fetching data from a JSON endpoint? Yup! `componentDidMount()`

First we'll "grab" the JSON and throw it into a variable by using `JSON.parse()` on our `colorJSON` variable that exists outside and separate from our React component. And then we'll set the state, updating the `colorData` array.

```jsx
      componentDidMount() {
        const colorData = JSON.parse(colorJSON);
        this.setState({colorData});
      }
```

(Normally you'd have some nice call to your JSON endpoint with a lovely Promise handler that says "then, if no error, set the state." But you get the idea. This example will do for now.)

Now that you've got the data, whatchu gonna do with it?! I'd think it'd be pretty cool to map that array of color data and render each color into a nice little list item (`<li>`) element. To do this, let's use the handy array map function, and let's create a new function to use in the map function. We shall call it `renderColorItem` and it will be amazing; it will accept an object that has the `name` and `hsl` keys as a parameter, and it will return a list element that says the name of the color and uses the HSL value to set the font color of the item.

```jsx
      renderColorItem({name, hsl}) {
        return <li style={{color: hsl}}>{name}</li>;
      }
      render() {
        return (
          <ul style={{fontSize: 48}}>
            {this.state.colorData.map(this.renderColorItem)}
          </ul>
        );
      }
```

Amazing. Absolutely amazing. Your component looks truly terrific. Here is all your code in full:

```jsx
    const colorJSON = '[{"name": "red", "hsl": "hsl(0, 100%, 50%)"}, {"name": "blue", "hsl": "hsl(240, 100%, 50%)"}, {"name": "purple", "hsl": "hsl(270, 100%, 50%)"}]';
    class ColorList extends React.Component {
      constructor(props) {
        super(props);
        this.state = {
          colorData: []
        };
      }
      renderColorItem({name, hsl}) {
        return <li style={{color: hsl}}>{name}</li>;
      }
      render() {
        return (
          <ul style={{fontSize: 48}}>
            {this.state.colorData.map(this.renderColorItem)}
          </ul>
        );
      }
      componentDidMount() {
        const colorData = JSON.parse(colorJSON);
        this.setState({colorData});
      }
    }
    ReactDOM.render(
      <ColorList />,
      document.getElementById('content')
    );
```

## Challenge

> I double dog dare you.

Bet you can't actually fetch some real data from an external JSON endpoint!
