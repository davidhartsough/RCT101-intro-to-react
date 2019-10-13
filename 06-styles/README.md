# Styles

## Objective

> What will I learn?

The goal of this lesson is to learn a few different (and unique) ways to style React components.

## Overview

> What is it?

Styling? Oh, it's exactly what it has always been. There are just some different patterns to discuss now in this new UI-building methodology (React-land).

### Inline styles

Why inline styling in itself is no new concept, the idea of creating a variable that is defines a style object very well may be for you. One of the oddities of this pattern is that you will be writing CSS rules in JS. I'm sure you've had to do this at some point, and you've accessed/modified the style object of an element, like so:
```js
document.getElementById('content').style.fontWeight = 100;
```
So we're going to take that approach and apply it within React-land by creating separate style object variables and attaching them to the style attributes of React elements. It's pretty straightforward — but you know what's even more straightforward?

### The ever-classic and familiar CSS

By the end of this, I also hope to leave you with a sense of familiarity by demonstrating how the traditional methods of styling are still applicable in React-land.

## Purpose

> Why should I care? What problem(s) does this solve?

There are times when different styling approaches are necessary to undertake. Some approaches are better than others in specific scenarios. For example, with pure JS, I cannot use fancy CSS selectors to apply styles for special cases (most any selector that incorporates a colon), such as ":hover" or "::after". On the flip side, with pure CSS, I cannot dynamically determine style rule values based on variables (most any value that incorporates a number), such as using math to calculate the required size for an element.
Therefore, it's valuable to be knowledgable about the different approaches, so that you can effectively use them when duty calls!

## Walkthrough

> Let's check it out!

It's Detailed Exploratory, Explanatory Application Examination Exercise Lab Activity Time!


Okay, let's just say that we left off with this snippet of JSX, even though we really didn't:

```jsx
    const Child = (props) => <h1>Wow! I sure love these lessons.</h1>;
    const Parent = (props) => {
      return (
        <div>
          <Child />
          <Child />
        </div>
      );
    };
    ReactDOM.render(
      <Parent />,
      document.getElementById('content')
    );
```

Cool. Glad you've still got your imagination. Now, imagine a component that is a just a block of text with a cool font color that will tell you what color it is? Let's make that image become a reality with that Child component you've got there.

But first, let's make the Parent component nice and good-lookin'. For this one, we can just stylize it using a style object and inline styles.
First, create an object in the Parent component called parentStyle and give it two properties: `backgroundColor: '#EEE',` and `fontFamily: 'sans-serif'`.

```jsx
    const Parent = (props) => {
      const parentStyle = {
        backgroundColor: '#EEE',
        fontFamily: 'sans-serif'
      };
      return (
        <div>
          <Child />
          <Child />
        </div>
      );
    };
```

All you have to do to apply that set of styles in that parentStyle object is just put it as the `<div>`'s style attribute value.

```jsx
    const Parent = (props) => {
      const parentStyle = {
        backgroundColor: '#EEE',
        fontFamily: 'sans-serif'
      };
      return (
        <div style={parentStyle}>
          <Child />
          <Child />
        </div>
      );
    };
```

It's ready. Next up, it's time to make the Child component meet your wild imagination.

*Step 1: Define the color.*
Create another object in the Parent component called childStyle and give it a property `color: 'red'`.

```jsx
    const Parent = (props) => {
      const parentStyle = {
        backgroundColor: '#EEE',
        fontFamily: 'sans-serif'
      };
      const childStyle = {
        color: 'red'
      };
      return (
        <div style={parentStyle}>
          <Child />
          <Child />
        </div>
      );
    };
```

*Step 2: Pass down the prop.*
For the two Child elements that you're rendering in the Parent component, pass them a `style` prop with a value of your newly made childStyle variable.


```jsx
    const Parent = (props) => {
      const parentStyle = {
        backgroundColor: '#EEE',
        fontFamily: 'sans-serif'
      };
      const childStyle = {
        color: 'red'
      };
      return (
        <div style={parentStyle}>
          <Child style={childStyle} />
          <Child style={childStyle} />
        </div>
      );
    };
```

*Step 3: Utilize the Child's props*
Now that you've given the Child component a `style` prop to work with, put it to action! All you gotta do is add a style attribute to the `<h1>` and give it a value of `props.style`.

```jsx
    const Child = (props) => (
      <h1 style={props.style}>Wow! I sure love these lessons.</h1>
    );
```

Easy peasy.

*Step 4: Speak the true-true*
But how do you make the component tell you what color it is? Good thinking; you're right! The style prop is just another object, so you can access its properties with the dot syntax, duh.

```jsx
    const Child = (props) => (
      <h1 style={props.style}>Wow! I sure look great in {props.style.color}.</h1>
    );
```

Now you've got two text blocks that tell you what color they are, but they're both the same color, so that's rather uninteresting.

*Step 5: Spice up session*
Let's expand the style object being passed down to the Child components to have two separate objects inside itself. First, rename it to childStyles. Then make one internal object called `redStyle` and the other `blueStyle`. I don't even have to tell you what to do next. (But remember to pass down the new redStyle object to the first Child component and the blueStyle object to the second one.)

```jsx
    const Parent = (props) => {
      const parentStyle = {
        backgroundColor: '#EEE',
        fontFamily: 'sans-serif'
      };
      const childStyles = {
        redStyle: {
          color: 'red'
        },
        blueStyle: {
          color: 'blue'
        }
      };
      return (
        <div style={parentStyle}>
          <Child style={childStyles.redStyle}/>
          <Child style={childStyles.blueStyle}/>
        </div>
      );
    };
```

Gold star! Gold star! How many gold stars do you have now? You've done lots o' cool stuff.
Before wrapping up, try out writing an inline object inside the style attribute of another element:

```jsx
      return (
        <div style={parentStyle}>
          <Child style={childStyles.redStyle}/>
          <Child style={childStyles.blueStyle}/>
          <p style={{ color: 'teal' }}>Thanks funky inline styles.</p>
        </div>
      );
```

And since we shouldn't forget the OG, check out adding a class attribute to an element to reference some CSS. To do this, you'll have to add a `<style>` block in `<head>`.

```HTML
  <style type="text/css">
    .primary {
      color: orange;
    }
  </style>
```

Now that you defined a class, you can apply it to an element via the `className` attribute in JSX.

```jsx
      return (
        <div style={parentStyle}>
          <Child style={childStyles.redStyle}/>
          <Child style={childStyles.blueStyle}/>
          <p style={{ color: 'teal' }}>Thanks funky inline styles.</p>
          <p className="primary">Oh, and classes are still a thing, too.</p>
        </div>
      );
```

Classic!

## Challenge

> I double dog dare you.

This time, instead of challenging you to code some more elaborate example, I challenge you to read more into this discussion. Specifically, I challenge you to do some research on styling in React using CSS Modules. Of all the styling options availabe to you, the CSS Modules approach seems to provide the best balance while solving all needs. It wasn't possible to demonstrate this approach in these simple examples. I'll let you read up on why that is on your own. Reference the References section below for some links to articles.

## Review

> Don't cheat. Answers are located upside-down on the reverse side of this README.

1. How might you compare and contrast inline vs traditional styling to your grandma?
2. What benefits do inline styles offer?
3. What benefits do traditional styles offer?
4. What benefits do CSS Modules offer in React? (Ha! Trick question. You have to do the challenge to answer this!)
5. What is your favorite color?

## References

> Look at the sites I plagiarized from.

- https://google.com/
- https://survivejs.com/react/advanced-techniques/styling-react/
- http://andrewhfarmer.com/how-to-style-react/
