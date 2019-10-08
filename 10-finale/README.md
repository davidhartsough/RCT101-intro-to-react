# Finale

## Objective

> Bring it on home.

This is it. You're on the home stretch. 

Your mission is to finish building the Color List app by adding a color creator form that allows users to either make their own color or randomly generate a color.

I also have snuck in a final lesson in here on Controlled Components. Sorry, not sorry. 

## Purpose

> What is my purpose? (You pass butter.)

You're going to bring all that you've learned in these lessons (and some!) together. You will demonstrate your newfound skills and prove yourself in the final test of your React prowess! 
Do you have what it takes? 

## Controlled Components

> HOLD UP! Forms??!?

You: "The teacher added another lesson for the last day of class?!"

Me: Deal with it. :sunglasses:

You almost have all the knowledge you could ever need. But you'll need to learn about one more key ingredient before you can complete your mission. 

Ever wonder how form building and handling might work in React? Well, now's the time to learn. (It's never too late.)

The answer is: Controlled Components. (Usually.)

Controlled components are unique in that they are stateful components because each the form inputs control their own state internally. Controlled components don't usually carry the state of the data (or data manipulation) of an app, just the state of the user input in the form. This can kind of be confusing, because normally only the Container component has state, whereas the other components are usually all presentational components that are pure functions. But fear not, you can still make your forms super reusable and "dumb". You just have to manage control over the state of the inputs in your form by using event handlers. You can still leave labels and data manipulation functions as props. Typically, a form will submit user input to be saved, adding to the data of the app. That data handling will still happen in the Container component, so it will need to pass down that function to the controlled component as a prop. 

For example, a simple form component might have a text input and a submit button. The value of that text input will be stored in the form component's state, maybe even as `value`: 

```jsx
    constructor(props) {
        super(props);
        this.state = {
            value: ''
        };
    }
```

But in order for this form component to manage the user input and keep the state in sync, you'd have to add an event handler: `onChange`. And in that change handler function, you'll need to update the state to match the user's input via `this.setState({})`.

```jsx
    /*...*/
    handleChange(event) {
        this.setState({value: event.target.value});
    }
    render() {
        <form onSubmit={this.handleSubmit}>
            <label>
                {this.props.label}
                <input value={this.state.value} onChange={this.handleChange} />
            </label>
            <input type="submit" value="Submit" />
        </form>
    }
    /*...*/
```

You'd also have to handle the submission event (`this.handleSubmit`), and that's where you can hook into whatever props may have been passed down to update (..., change, manipulate, etc.) the data:

```jsx
    handleSubmit(event) {
        this.props.submitValue(this.state.value);
        event.preventDefault();
    }
```

With this final learning, I bid you come forth and take on the challenge that lies before you here. You have been summoned. Do you have what it takes? (If you have gone through all these lessons, then, yes, you definitely do. I just want the final test to be intimidating... But you totally got this.)

## The Final Test

> Show me what you got!

You will start off with where you left off in the last lesson and build upon your Color List app. 

You have 3 steps to glory: 

  1) Create a `createColor` function in the container component class that accepts a new color object as a parameter and adds it to the array of colorData in the app's main state. 
  
  2) Create a `ColorCreator` controlled component class that accepts the `createColor` function as a prop and uses it when the user submits a new color. This color creator form will need a text input for the name of the new color and a number input for the hue of the new color, a submission button, and just for fun, a button that will randomly generate a new color for you. 
  
  3) Add that new `ColorCreator` component into the render function of the `ColorListContainer` component. Profit. 

**DJ, HIT IT!**

Okay, so we left off with this snippet of JSX:

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

## Let's do this.

> The safety word will be "whiskey"

**Step 1: Create the `createColor` function**

The primary data of the Color List app is made up of Color items. Each color item is an object with two properties: "name" and "hue". The "name" property is simply a string, while the "hue" property is a number from 0–360. For a refresher, here's a sample color item object: 

```jsx
    {
        name: 'green',
        hue: 120
    }
```

The state of the ColorListContainer, which is the core of your Color List app, is an array of these objects called `colorData`: 

```jsx
      constructor(props) {
        super(props);
        this.state = {
          colorData: []
        };
      }
```

Your first goal is to create a `createColor` function that accepts a new color item object as a parameter and adds that new item to the `colorData` array in the ColorListContainer's state. So first, code what you know now: 

```jsx
    createColor(newColor) {
      // Get the current array of color item objects from this.state.colorData and append newColor to it.
      const colorData = [];
      // Give the state the updated array with the newColor added.
      this.setState({colorData});
    }
```

I'll give you a hint that the ES6 spread operator offers a great way to create a new array that is just a clone of another array, plus a new item. Look it up and see if you can write a one-liner bit of code for `const colorData = //...` before looking at the code below...

```jsx
      createColor(newColor) {
        const colorData = [...this.state.colorData, newColor];
        this.setState({colorData});
      }
```

**Step 2: Create a `ColorCreator` controlled component class**

Hold onto your hats... I don't know why you're wearing multiple hats. And it's just you right? Can't have multiple people wearing multiple hats all going through this final lesson together. I'd consider that cheating and would fail you on the spot. It's a zero tolerance policy. Sorry. 

Well, in any case, I wouldn't want you to lose any hats you might be wearing just because you're about to write some killer code and whip up a new form for your app. 

I'll stop speaking now and just ask that you do this: 

```jsx
    class ColorCreator extends React.Component {
      constructor(props) {
        super(props);
        this.state = {};
      }
      render() {
        return (
          <div style={{fontFamily: 'sans-serif'}}>
            FORM ELEMENTS GO HERE!
          </div>
        );
      }
```

Great job! I maybe wouldn't have used the caps locks type that you put in your div, but no judgements here. We'll replace it now anyway. In fact, go ahead and add the form elements that we will need: two inputs (with labels) and a button for submission. The first input will be a text input for the *Name* of the new color the user is creating, while the second input will be a number input for the *Hue* of the new color the user is creating. And just to get this nicety out of the way here, go ahead and add 4px of left margin to the Hue label and the button to help the design and help the user see some separation. 

```jsx
        return (
          <div style={{fontFamily: 'sans-serif'}}>
            <label>Name: 
              <input type="text"/>
            </label>
            <label style={{marginLeft: 4}}>Hue: 
              <input type="number"/>
            </label>
            <button style={{marginLeft: 4}}>Create color</button>
          </div>
        );
```

Fantastic. Now, we know that the hue value can only range from 0–360, and since 0 and 360 are virtually the same hue, go ahead and put a `min` attribute on there of 0 and a `max` of 355. Why 355? Well, because I think we should make the `step` attribute of this number input 5. So do as I say, please, and don't question me.

```html
    <input type="number" min="0" max="355" step="5"/>
```

NEXT!

What about state? We have a class component here because we know we want state. Why do we know we want state? Because I snuck in another lesson here about controlled components, and by jove, this surely is going to be controlled component — one of the best, I'd say. 

YES! I completely agree that the only state keys we will need will be `name` and `hue` to match the two possible user inputs when creating a new color item object (those are the only two properties the object needs). So wouldn't you know it, the state object of your new color creator (controlled) component class will match that of a color item object!

Set the defaults in the constructor to be blank (empty string for name, 0 for hue). Then assign these state properties to the respective `value` attributes of the inputs: 

```jsx
      constructor(props) {
        super(props);
        this.state = {
          name: '',
          hue: 0
        };
      }
      render() {
        return (
          <div style={{fontFamily: 'sans-serif'}}>
            <label>Name: 
              <input type="text" value={this.state.name}/>
            </label>
            <label style={{marginLeft: 4}}>Hue: 
              <input type="number" value={this.state.hue} min="0" max="355" step="5"/>
            </label>
            <button style={{marginLeft: 4}}>Create color</button>
          </div>
        );
      }
```

Terrific! 
Now we just have to hook up the pangartonic yingle-bits to the warbletene bymanamum once the cycanebs has made its 3rd revolution. 

```jsx
      render() {
        return (
          <div style={{fontFamily: 'sans-serif'}}>
            <label>Name: 
              <input type="text" value={this.state.name} onChange={(event) => this.handleNameChange(event)}/>
            </label>
            <label style={{marginLeft: 4}}>Hue: 
              <input type="number" value={this.state.hue} onChange={(event) => this.handleHueChange(event)} min="0" max="355" step="5"/>
            </label>
            <button style={{marginLeft: 4}} onClick={() => this.generateRandomColor()}>Create color</button>
          </div>
        );
      }
      handleNameChange(event) {}
      handleHueChange(event) {}
      handleClick() {}
```

Wow. I sure am glad you knew exactly what I meant. You truly must be an exceptional developer indeed! 

Don't let these encouraging tidbits get to your head. I'd say you've only made it halfway through the final test. And just look at what you've written! Those three functions lingering at the bottom are most certainly unacceptable. Please revisit them. 

Firstly, the onChange event handlers should be rather straightforward for you to take on, since you just ran through the controlled component lesson mere minutes ago. While it's all fresh in your brain, please regurgitate that lesson's learnings now before they get moldy. 

All I'm asking now is for you to take those two onChange event handlers and have them set the state for their respective value.

```jsx
      handleNameChange(event) {
        this.setState({name: event.target.value});
      }
      handleHueChange(event) {
        this.setState({hue: event.target.value});
      }
```

Marvelous. But we also need to handle all the button clicks that your users may feed to your new form. These button clicks are of course, as we may assume, the user's best attempt at interacting with your app with the desire to create a new color. So your first order of business is to help them create a new color item object by using whatever values are currently stored in the state of this controlled form component (name and hue). Once you've created that new color item object, it would be high time for you to recall that you have conveniently passed this controlled ColorCreator component a prop, a most peculiar prop; a prop so peculiar that you just want to throw it down in your next line of code while passing the new color item object to it. Invigorating, I know! 

And please do help the users clean up after themselves. Once they've made a new color, there's no sense in leaving their inputs filled with whatever nonsense they might have given them. No, not at all. Instead, please return all seats to their upright positions, put all tray tables back, and set the state to its blank and empty defaults once again. 

```jsx
      handleClick() {
        const newColor = {
          name: this.state.name,
          hsl: `hsl(${this.state.hue}, 100%, 50%)`
        };
        this.props.createColor(newColor);
        this.setState({name: '', hue: 0});
      }
```

Wonderful! I would say this is A++ material, but something is lacking. 

I imagine you might've guessed this by now, but I am a rather large fan of randomness. (Some might even describe my fandom to be bigger than the biggest fan; it's really quite big!) Randomness is debatabely the best source of "true fun". The only problem is that we humans cannot fully achieve true randomness, so we have yet to experience true fun. We've designed computers so that we can become ever closer towards true randomness, but even the greatest supercomputers of our time cannot produce true randomness. 

While some may see the glass as half filled with grapefruit juice, I prefer to see the glass as half filled with pieces of eight, which are still far more useful than the nasty juice of a grapefruit could ever dream of being, despite the fact that pieces of eight have not been a legal tender or currency in any capacity for countless years around our globe. All this to say: if any of us have half a care at achieving true fun, then we might as well use these computers we've designed to get some randomness out of them, even if we get subpar results. The truth is, we don't know any better or about some sort of "truer" fun, so it's the best we've got as far as we're concerned. 

Therefore, in order for me to consider your Color List app to be A++ material, you need to include an aspect of randomness in it. Otherwise, I will have to dock points off on the rubric under the category "fun", which is, unsurprisingly, worth 27% of your grade. 

Knowing that you strive for exemplary scores on your rubric and exemplary apps on your portfolio of projects, allow me to help get you in the right direction towards achieving the best that randomness can bring to this ColorCreator component. 

First, please add a paragraph element at the bottom of your form that contains the most provoking text you can think of between its opening and closing tags: 

```html
    <p>Or...</p>
```

Next, simply place a button below that which reads "Generate random color" (or the likes). And give it an `onClick` handler that will call upon a new function for you to create called `generateRandomColor()`.

```jsx
      render() {
        return (
          <div style={{fontFamily: 'sans-serif'}}>
            <label>Name: 
              <input type="text" value={this.state.name} onChange={(event) => this.handleNameChange(event)}/>
            </label>
            <label style={{marginLeft: 4}}>Hue: 
              <input type="number" value={this.state.hue} onChange={(event) => this.handleHueChange(event)} min="0" max="355" step="5"/>
            </label>
            <button style={{marginLeft: 4}} onClick={() => this.handleClick()}>Create color</button>
            <p>Or...</p>
            <button onClick={() => this.generateRandomColor()}>Generate random color</button>
          </div>
        );
      }
```

LIFEHACK: Just copy the click handler function and rename it to `generateRandomColor()`. 

Now, you won't be needing to clear the inputs and clean up after your user because the computer will generate the color for you and is not nearly as messy as humans. Please delete the line that resets the state at the bottom of your new `generateRandomColor()` function. 

```jsx
      generateRandomColor() {
        const newColor = {
          name: this.state.name,
          hsl: `hsl(${this.state.hue}, 100%, 50%)`
        };
        this.props.createColor(newColor);
      }
```

Grand! But now comes the exciting bit! 

In the newColor object, replace the name value with `this.generateRandomColorName()`. We'll build out that function in a second. But before we do, creating a random hue is much more simple, so let's tackle that. 

To generate a random hue, you can just use some `Math`! Math is great. Math is a key ingredient to fun because it is a key ingredient to randomness. What you'll want to do here is simply ask the computer to use its `Math` to create a random number and multiply that by 359, giving us a random float for the hue. BUT! You need a nice round integer, so please ask your computer to use its `Math` to throw the float to the floor. Got it? 

```jsx
      generateRandomColor() {
        const newColor = {
          name: this.generateRandomColorName(),
          hsl: `hsl(${Math.floor((Math.random() * 359))}, 100%, 50%)`
        };
        this.props.createColor(newColor);
      }
```

Super! Here comes the most funnest fun part of your new app: random color name generation, baby!


...


> I regret to inform you that all of the FUN funds have dried up and disappeared. Please take this function as a token of our regret. It's plug-n-play, so it goes great with some ole copy-pasta.

> (You see, the writer of this lesson was about to dive into a manifesto on the utter beauty and importance of randomness while explaining how to generate a random color name for this new function. But the editors could not see the business value in this and promptly transferred all FUN funding over to the SERIOUS SALES department, which gave the leadership exec team at SERIOUS SALES just enough money to finally hire someone to fire Chris.)

```jsx
      generateRandomColorName() {
        const consonants = ['b','c','d','f','g','h','j','k','l','m','n','p','r','s','t','v','x','z','w','y'];
        const vowels = ['a','e','i','o','u'];
        let name = consonants[Math.floor((Math.random() * consonants.length))] + vowels[Math.floor((Math.random() * vowels.length))] + consonants[Math.floor((Math.random() * consonants.length))];
        if (Math.random() >= 0.5) {
          name += vowels[Math.floor((Math.random() * vowels.length))] + consonants[Math.floor((Math.random() * consonants.length))];
        }
        if (Math.random() >= 0.5) {
          name += vowels[Math.floor((Math.random() * vowels.length))] + consonants[Math.floor((Math.random() * consonants.length))];
        }
        return name;
      }
```

Incredible! You have just now finished creating the most remarkable controlled component class that React.Component has ever granted extension from: 

```jsx
    class ColorCreator extends React.Component {
      constructor(props) {
        super(props);
        this.state = {
          name: '',
          hue: 0
        };
      }
      render() {
        return (
          <div style={{fontFamily: 'sans-serif'}}>
            <label>Name: 
              <input type="text" value={this.state.name} onChange={(event) => this.handleNameChange(event)}/>
            </label>
            <label style={{marginLeft: 4}}>Hue: 
              <input type="number" value={this.state.hue} onChange={(event) => this.handleHueChange(event)} min="0" max="355" step="5"/>
            </label>
            <button style={{marginLeft: 4}} onClick={() => this.handleClick()}>Create color</button>
            <p>Or...</p>
            <button onClick={() => this.generateRandomColor()}>Generate random color</button>
          </div>
        );
      }
      handleNameChange(event) {
        this.setState({name: event.target.value});
      }
      handleHueChange(event) {
        this.setState({hue: event.target.value});
      }
      handleClick() {
        const newColor = {
          name: this.state.name,
          hsl: `hsl(${this.state.hue}, 100%, 50%)`
        };
        this.props.createColor(newColor);
        this.setState({name: '', hue: 0});
      }
      generateRandomColor() {
        const newColor = {
          name: this.generateRandomColorName(),
          hsl: `hsl(${Math.floor((Math.random() * 359))}, 100%, 50%)`
        };
        this.props.createColor(newColor);
      }
      generateRandomColorName() {
        const consonants = ['b','c','d','f','g','h','j','k','l','m','n','p','r','s','t','v','x','z','w','y'];
        const vowels = ['a','e','i','o','u'];
        let name = consonants[Math.floor((Math.random() * consonants.length))] + vowels[Math.floor((Math.random() * vowels.length))] + consonants[Math.floor((Math.random() * consonants.length))];
        if (Math.random() >= 0.5) {
          name += vowels[Math.floor((Math.random() * vowels.length))] + consonants[Math.floor((Math.random() * consonants.length))];
        }
        if (Math.random() >= 0.5) {
          name += vowels[Math.floor((Math.random() * vowels.length))] + consonants[Math.floor((Math.random() * consonants.length))];
        }
        return name;
      }
    }
```

**Step 3: Profit**

Being the final step of this final lesson's finale, this step is the most difficult step, as expected. However! If you can succeed in accomplishing this task, you will profit in the most profitable manner. 

Are you ready? Do you accept the challenge before you? 

I'd honestly find it a bit silly for you to have come all this way only to chicken out now. You're not a chicken, are you?... 

*NOW!* Add that new, good-lookin' `ColorCreator` component into the render function of the `ColorListContainer` component. I think it'd fit nicely at the top, nestled inside a wrapper `<div>`, right next to the `ColorList` component -- don't you think so?

```jsx
        return (
          <div>
            <ColorCreator createColor={(newColor) => this.createColor(newColor)}/>
            <ColorList {...colorListProps}/>
          </div>
        );
```

Bring it allllll together: 

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
    class ColorCreator extends React.Component {
      constructor(props) {
        super(props);
        this.state = {
          name: '',
          hue: 0
        };
      }
      render() {
        return (
          <div style={{fontFamily: 'sans-serif'}}>
            <label>Name: 
              <input type="text" value={this.state.name} onChange={(event) => this.handleNameChange(event)}/>
            </label>
            <label style={{marginLeft: 4}}>Hue: 
              <input type="number" value={this.state.hue} onChange={(event) => this.handleHueChange(event)} min="0" max="355" step="5"/>
            </label>
            <button style={{marginLeft: 4}} onClick={() => this.handleClick()}>Create color</button>
            <p>Or...</p>
            <button onClick={() => this.generateRandomColor()}>Generate random color</button>
          </div>
        );
      }
      handleNameChange(event) {
        this.setState({name: event.target.value});
      }
      handleHueChange(event) {
        this.setState({hue: event.target.value});
      }
      handleClick() {
        const newColor = {
          name: this.state.name,
          hsl: `hsl(${this.state.hue}, 100%, 50%)`
        };
        this.props.createColor(newColor);
        this.setState({name: '', hue: 0});
      }
      generateRandomColor() {
        const newColor = {
          name: this.generateRandomColorName(),
          hsl: `hsl(${Math.floor((Math.random() * 359))}, 100%, 50%)`
        };
        this.props.createColor(newColor);
      }
      generateRandomColorName() {
        const consonants = ['b','c','d','f','g','h','j','k','l','m','n','p','r','s','t','v','x','z','w','y'];
        const vowels = ['a','e','i','o','u'];
        let name = consonants[Math.floor((Math.random() * consonants.length))] + vowels[Math.floor((Math.random() * vowels.length))] + consonants[Math.floor((Math.random() * consonants.length))];
        if (Math.random() >= 0.5) {
          name += vowels[Math.floor((Math.random() * vowels.length))] + consonants[Math.floor((Math.random() * consonants.length))];
        }
        if (Math.random() >= 0.5) {
          name += vowels[Math.floor((Math.random() * vowels.length))] + consonants[Math.floor((Math.random() * consonants.length))];
        }
        return name;
      }
    }
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
        return (
          <div>
            <ColorCreator createColor={(newColor) => this.createColor(newColor)}/>
            <ColorList {...colorListProps}/>
          </div>
        );
      }
      componentDidMount() {
        const colorData = JSON.parse(colorJSON);
        this.setState({colorData});
      }
      createColor(newColor) {
        const colorData = [...this.state.colorData, newColor];
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
        return `hsl(${hslArray[0]}, ${randomSaturation}%, ${hslArray[2]})`;
      }
    }
    ReactDOM.render(
      <ColorListContainer />,
      document.getElementById('content')
    );
```

**OUTSTANDING! A++**

You've really done it. You have achieved the highest mark in RCT101. 

Congratulations!

Would someone please see to it that this extraordinary developer receives the grand prize award?

## Challenge

> I double dog dare you. 

There are cases when you need an "uncontrolled" component. They are rare, but they may happen. Due to the rarity of their use cases, I have chosen to leave them out of these lessons. But you won't settle for that. So read on! Check out this article on `refs`: 

https://facebook.github.io/react/docs/refs-and-the-dom.html

## Review

> You've proven yourself most worthy! 

You're a passionate and enthused developer. You wanna be the very best (like no one ever was). You want to keep learning new things for a lifetime, because a true developer knows that LEARNING is the best tool of the trade. Technology moves quickly, and we have to keep up. You're a pioneer. You push the limits of innovation. You are on the cutting edge of breakthrough technology, peering into a future of amazing possibilities. In this realm wherein you are a masterful creator, you take pride in your craftsmanship and want to build what is best. You keep an ever-watching eye on the horizon of the development world, ready to discover and even contribute to the next step of progress toward redefining what is best. You always ask, "What is best?" because that is what you want. And when carving a vision to answer that question, you continually recognize what is possible, and in examining the broad potentiality before you, you know how to reach your mark. 

Never stop learning. Don't settle until you've achieved what is best. 

Best,

David

## References

- https://facebook.github.io/react/docs/forms.html
