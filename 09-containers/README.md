# Containers

## Objective

> What will I learn?

The goal of this lesson is to learn what containers are, why/when they're useful, and how to use them! 

## Overview

> What is it?

A Container is a React Component that fetches data, then renders its corresponding sub-components, which are often presentational components. 

Here are some typical distinctions between these two types of components: 

Container components are often "stateful" in that they serve as the data source, while presentational components are often stateless in that they merely receive data and callbacks exclusively via props. 
The container component handles data mutation, while the presentational components merely receive those callbacks from their respective containers and apply them to event handlers appropriately. 
Finally, containers are usually written as Class components, while presentational components are written as functional components (unless they too need state or a lifecycle method for a specific reason). 

## Purpose

> Why should I care? What problem(s) does this solve?

The value of containers lie in their ability to separate the data-fetching and rendering concerns into different components, thereby making the rendering components much more reusable. The function of a container is to manage how the app will work and render respective presentational components, whereas presentational components manage how the app will look. 

As you can see, while the title of this lesson is "Containers", a majority of the beauty of Containers is actually seen in their counterpart: the presentational components. Presentational components that are "dumb", stateless, and extremely reusable are the foundation for building UI with components — a dream that both developers AND designers strive for. But presentational components are only possible if you take the data-handling and lift that up to Container components. When you separate your concerns, you enter into a life of near nirvana (when it comes to building an app at least).

## Walkthrough

> Let's check it out!

It's Detailed Exploratory, Explanatory Application Examination Exercise Lab Activity Time!

Let's revisit that Color List app and separate our concerns! To do this, we're gonna want to separate the large ColorList component into a Container component and then a corresponding presentational component. Oh! And while we're at it, we're going to allow users to click on colors in the list to see them in different levels of saturation. 
Let's GO!

Okay, so we left off with this snippet of JSX:

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
        return <ul style={{fontSize: 48}}>{this.state.colorData.map(this.renderColorItem)}</ul>;
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

*Step 1: Prep the engines.*

Our first move is to rename the ColorList class component to "ColorListContainer." Bold move! 

Next, take the ColorList component as we knew it now and turn it into a pure component, because we want to make a new presentational component, without state. Let the ColorList component take colorData as a prop, map that data, then render it out in an unordered list. 

```jsx
    const ColorList = (props) => {
      const colors = props.colorData.map(({name, hsl}) => {
        return <li style={{color: hsl}}>{name}</li>;
      });
      return <ul style={{fontSize: 48}}>{colors}</ul>;
    };
    class ColorListContainer extends React.Component {
      constructor(props) {/*...*/}
      render() {
        return <ColorList colorData={this.state.colorData}/>;
      }
      componentDidMount() {/*...*/}
    }
```
*Step 2: Stock the frigdes.* 

Get ready for what's comin'. We're gonna add in a new feature interaction where users can click on each color. But before we dive into that, let's make the presentational component as reusable as possible. It's a dumb component; it doesn't know its left from right; and it certainly doesn't know what is going to happen when a color item is clicked on. So what does that mean? PROPS. BINGO. We need to give the presentational component both a click handler AND a description of that click handler to let the users know that we've added this awesome new feature in. To open the component for this incoming prop, let's just add a paragraph tag that takes a clickHandlerDescription prop as its text. We'll get to the click handler momentarily. 

Oh, and while we're adding more text and a div envelope around our presentational component, it's prime time to address the fact that serif font is nasty and we don't want it comin' into our app and making it look yucky. So please change the font-family style here. Nobody wants to read in serif font. Scientific fact. 

```jsx
      return (
        <div style={{fontFamily: 'sans-serif'}}>
          <p>{props.clickHandlerDescription}</p>
          <ul style={{fontSize: 48}}>{colors}</ul>
        </div>
      );
```

*Step 3: Hire a click handler.*

Yup. Just hire this gal. She'll handle all the clicks... She's quite apt at it. 

But where will this function exist? That's right: the Container. Add this to the class. 

It's necessary to include the index so that we know which color we are changing and to update the state accordingly. 

```jsx
      handleColorClick(index) {
        let colorData = this.state.colorData;
        colorData[index].hsl = this.changeSaturation(colorData[index].hsl);
        this.setState({colorData});
      }
```

*Step 4: Just throw in this fancy fun function.*

You: "Oh yeah... what was that changeSaturation function we just called before??"

Me: "Don't even worry 'bout it! Don't ask where it came from. Just accept it. Quietly."

(Also goes in the Container class component.)

```jsx
      changeSaturation(hsl) {
        const regexp = /hsl\(\s*(\d+)\s*,\s*(\d+%)\s*,\s*(\d+%)\)/g;
        const hslArray = regexp.exec(hsl).slice(1);
        const randomSaturation = Math.floor((Math.random() * 75) + 25);
        return `hsl(${hslArray[0]},${randomSaturation}%,${hslArray[2]})`;
      }
```

*Step 5: Pass them props yo!*

Now the Container and presentational components are ready to go — once we pass down the right props of course! To do this, we're gonna get a lil' funky and try out something new here: let's make an object constant that defines the props in the Container's render function. Then, using the new ES6 spread operator, we can spread that object all over our ColorList component sandwich. 

First, remember that we need to still pass the colorData of course. Next, we have to add two new props. The next one should be the click handler. We're gonna call that one "handleColorClick", and it will use our new handleColorClick function. Finally, we have the description, which will just be a string. You can say the description is "Click a color to see a different saturation.", if you'd like. 

```jsx
      render() {
        const colorListProps = {
          colorData: this.state.colorData,
          handleColorClick: (index) => this.handleColorClick(index),
          clickHandlerDescription: 'Click a color to see a different saturation.'
        };
        return <ColorList {...colorListProps}/>;
      }
```

*Step 6: Utilize that click handler prop.* 

Back in our presentational component, DING DONG, we got a new prop! Let's per 'er to use. 

While mapping the colorData, add an onClick handler that simply calls the new prop as a function. Remember that we'll need the index now! Luckily the map function will handle that for us outta-the-box. 

Also remember that this ColorList component is a pure function/presentational component. It doesn't specify its data or the functions that manipulate that data; it only decides how to present/render the data (which is receives as a prop) and decides which events will trigger those data manipulattion functions (which it also receives as props). Reusability, HO! 

```jsx
      const colors = props.colorData.map(({name, hsl}, index) => {
        return <li style={{color: hsl}} onClick={() => props.handleColorClick(index)}>{name}</li>;
      });
      return (
        <div style={{fontFamily: 'sans-serif'}}>
          <p>{props.clickHandlerDescription}</p>
          <ul style={{fontSize: 48}}>{colors}</ul>
        </div>
      );
```

*Step 7: Admire the final result.* 

BAM! Now THAT'S separation of concerns! Just look at how reusable that presentational component is now! 

```jsx
    const colorJSON = '[{"name": "red", "hsl": "hsl(0, 100%, 50%)"}, {"name": "blue", "hsl": "hsl(240, 100%, 50%)"}, {"name": "purple", "hsl": "hsl(270, 100%, 50%)"}]';
    const ColorList = (props) => {
      const colors = props.colorData.map(({name, hsl}, index) => {
        return <li style={{color: hsl}} onClick={() => props.handleColorClick(index)}>{name}</li>;
      });
      return (
        <div style={{fontFamily: 'sans-serif'}}>
          <p>{props.clickHandlerDescription}</p>
          <ul style={{fontSize: 48}}>{colors}</ul>
        </div>
      );
    };
    class ColorListContainer extends React.Component {
      constructor(props) {
        super(props);
        this.state = {
          colorData: []
        };
      }
      render() {
        const colorListProps = {
          colorData: this.state.colorData,
          handleColorClick: (index) => this.handleColorClick(index),
          clickHandlerDescription: 'Click a color to see a different saturation.'
        };
        return <ColorList {...colorListProps}/>;
      }
      componentDidMount() {
        const colorData = JSON.parse(colorJSON);
        this.setState({colorData});
      }
      handleColorClick(index) {
        let colorData = this.state.colorData;
        colorData[index].hsl = this.changeSaturation(colorData[index].hsl);
        this.setState({colorData});
      }
      changeSaturation(hsl) {
        const regexp = /hsl\(\s*(\d+)\s*,\s*(\d+%)\s*,\s*(\d+%)\)/g;
        const hslArray = regexp.exec(hsl).slice(1);
        const randomSaturation = Math.floor((Math.random() * 75) + 25);
        return `hsl(${hslArray[0]},${randomSaturation}%,${hslArray[2]})`;
      }
    }
    ReactDOM.render(
      <ColorListContainer />,
      document.getElementById('content')
    );
```

Wow. Stellar work. You make my mother proud. 

## Challenge

> I double dog dare you. 

... No, I TRIPLE dog dare you to finish this introduction to React and take on the FINALE! Go to lesson 10. You're finally ready. You're FINALE-ready. 

## Review

> Don't cheat. Answers are located upside-down on the reverse side of this README. 

1. How might you explain what Containers are to your grandma? 
2. What benefits do containers offer? 
3. What is the difference between a container component and a presentational component? 
4. Why might designers love separate, presentational components? 
5. What is a great spread for any sandwich? (Jif Extra Chunky Peanut Butter might be the only acceptable answer.)

## References

> Look at the sites I plagiarized from. 

- https://medium.com/@learnreact/container-components-c0e67432e005
- https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0
- https://css-tricks.com/learning-react-container-components/
- http://reactpatterns.com/#container-component
