---
title: React - An Intro
date: 2017-10-15 17:29:14
tags:
---
# React - An (extended) intro


-----

We are going to find out what React is. Why use React? And a simple getting started....

## What is React?

React is a javascript library for components and single page applications. It is the **VIEW** layer. It enables us to create rich user experiences for web (and mobile and other things too like VR but yeah..)

React acts as the **View** layer for your application. Why even use a framework? Makes us more efficient. Write less code. Get more done. Etc. It gives us a level of abstraction over front-end development.

React thinks of the **VIEW** as a **FUNCTION** of **DATA**. `view = function(data)`
What does this mean?

It means given data, we use a function to derive the view. React gives us the capabilities of writing this function.

data -> react -> ui

## What makes React different from Angular(1) ( and other frameworks? )

React is currently **the most popular** library for front-end development. Virtual-DOM diffing, Components, ...


Why use React? It is simple to learn and use.

React can be used as much or little as you like. It does not have to be your entire application (but it can be). You can create react components, that just live within your app alongside the rest of your code, that you can feed data into.

Unlike Angular, Ember, Backbone, etc... React is not a full-fledged framework. It does not prescribe a way to handle a lot of things such as AJAX, 


## React and ReactDOM
(When using React for the web) React is broken into 2 packages. React and ReactDOM.

The **React package** contains Top-Level APIs for writing React components.

The **ReactDOM package** contains Top-Level APIs that handle rendering your component(s) to the DOM.

## Creating your first React Component

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>
            My first React app
        </title>
        <script src='https://unpkg.com/react@16.0.0/umd/react.production.min.js' type='text/javascript'></script>
        <script src='https://unpkg.com/react-dom@16.0.0/umd/react-dom.production.min.js' type='text/javascript'></script>
    </head>
    <body>
        <div></div>
        <script src='./index.js'></script>
    </body>
</html>

```

```js
	// index.js
    class MyComponent extends React.Component {
		render(){
        	return React.createElement('div', null, 'hullo!');
        }
    }
    ReactDOM.render(
    	React.createElement(MyComponent, null, null),
        document.getElementById('app')
    );
```

So here we are using ES6 syntax. We create a class for our component that extends the React.Component class. React.Component has many different methods we can implement but the one requirement is that we must implement `render` method. This method must return some output to render into the DOM. And we create this output by using `React.createElement`.

Lets read this syntax for React.createElement. __Create an element__ of **_type_ div**, with **null for _props_**, and **the string 'hullo!' for _children_**. After the third parameter you can continue putting more parameters which will count as sibling children. It's important to note that _children_ is not limited to a single string. It can also be other DOM elements and other React Components.

The call to `ReactDOM.render` is read, **render** an element of `MyComponent` in the DOM element with Id 'app'.


## JSX

React gives you the option of building components with JSX. JSX is a syntax that allows you to mix HTML markup with javascript. _This is an opinion_: It helps immensely with readability and brevity when writing React components.

JSX is not a requirement to using React, just an included and available option.

Here's how you would build a React component without JSX:
```js
	class HelloDiv extends Component {
    	render(){
        	return (
            	React.createElement(
                	'div',
                    null,
                    'hello!'
                )
            );
        }
    }
```

Here's the same thing using JSX:
```js
	class HelloDivJSX extends Component {
    	render(){
        	return (
            	<div>
                	hello!
                </div>
            );
        }
    }
```

Looks very familiar doesn't it? JSX is syntactic sugar. React will read this and transpile it back into javascript form looking more like the first snippet. 
## Props and State

So far we've created a component that just returns a div that says hello. That's fine I guess. But what happened to the view being a function of data?

That's where props and state come in. The second parameter in react.createElement takes a **props object**. This object is the data in `view = function(data)`.

Our first component revisited: 

```js
class MyComponent extends React.Component {
    render() {
        return React.createElement('div', null, this.props.msg);
    }
}

ReactDOM.render(
    React.createElement(MyComponent, { msg: 'YO!' }, null),
    document.getElementById('app')
);
```

Here we've changed `MyComponent` to use `this.props.msg` as its children instead of 'hullo!'. When we are rendering MyComponent to the DOM we are passing it the `{ msg: 'YO!' }` object I wrote above. Now let's do something funny...


## Component State

React components can have a notion of state. Components can each have an internal state that belongs to each component instance. You can mutate this state and react will know to rerender your component when state changes. 

```js
class MyComponent extends React.Component {
	constructor(props){
    	super(props);
        this.state = {
        	msg: 'hello'
        }
    }
    componentDidMount(){
    	setTimeout((function(){
        	this.setState({msg: 'hello world!'});
        }).bind(this), 4000);
    }
	render(){
    	return React.createElement('div', null, this.state.msg);
    }
}
ReactDOM.render(
    React.createElement(MyComponent),
    document.getElementById('app')
);
```

Okay we're introducing quite a few different things here.

So now we are overriding the constructor of React.Component. We do this so we can create state when this component is instantiated. We have to call `super(props);` to make sure the component calls the parent constructor. Next we set `this.state` to an object similar to what we saw in the last example.

Now I'm also introducing a **lifecycle method** that react provides. `componentDidMount` will fire after the component has been instantiated and mounted. In this method, I'm setting a timeout that will call in 4 seconds, a function that calls `this.setState`. This method that React provides will change the state according to the object we pass it. Notice that I'm binding `this` to the function. In ES6 syntax we can bind `this` to the function by using an arrow function, like so:

```js
	setTimeout(() => { this.setState({msg: 'hello world'}); }, 4000);
```


## Component Lifecycle

So I mentioned that `componentDidMount` is a **lifecycle method**. React.Component has a few methods that you can implement to tap into the component during certain points in its lifecycle.
These are :
- `componentDidMount()`
- `componentWillMount()`
- `componentWillReceiveProps()`
- `shouldComponentUpdate()`
- `componentWillUpdate()`
- `componentDidUpdate()`
- `componentWillUnmount()`

Some of these have access to variables such as `props`, `state`, `nextProps`, `nextState`. The methods are pretty self explanatory so I won't be going through all of them. But we will visit `shouldComponentUpdate()`.

## ShouldComponentUpdate

`ShouldComponentUpdate` is the method to let React know if a component's output is not affected by certain changes in state or props. The default behavior is to always return true, so the component rerenders everytime there is a state/props change.

Here's an example:

```js
class MyComponent extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            msg: 'hello',
            counter: 0
        }
    }
    componentDidMount() {
        setInterval(
            () => {
                this.setState({ counter: this.state.counter + 1 },
                    () => { console.log(this.state.counter); })
            }, 1000);
    }
    render() {
        console.log('render');
        return React.createElement('div', null, this.state.msg);
    }
}
```

So here we are using the same component as before but setting an interval within `componentDidMount`. This interval will set `counter` to increment every second.

We've put a `console.log` in the render method to show everytime React is calling `render`. And we can see that this component rerenders every second.

This doesn't make sense since we aren't _using_ `counter` in the returned markup. We can fix this by adding the following:

```js
    shouldComponentUpdate(nextProps, nextState){
        return (this.state.msg !== nextState.msg);
    }
```

We are changing this method to only return true when `this.state.msg` has changed and we are ignoring `counter` completely.

It is normally recommended to let React handle deciding whether your component should update but sometimes `shouldComponentUpdate` comes in handy when you know of only certain cases you want the component to rerender.

## Handling interaction

When you give props to a component through `React.createElement`, you can also pass functions as a part of this props object. Here's an example: 

```js
class MyComponent extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            counter: 0
        }
    }
    increment() {
        this.setState({ counter: this.state.counter + 1 });
    }
    render() {
        console.log('render');
        return React.createElement('div', null,
            React.createElement('button', { onClick: this.increment.bind(this) }, 'increment'),
            this.state.counter);
    }
}
```

## That's it for easy stuff...
Questions?

## Functional Stateless Components
Another way to write components is to write **functional stateless components**. These components are just functions that return markup and are useful when you do not need internal component state. Let's see one.

```js
const MyComponent = (props) => {
	return React.createElement('div', null, props.msg);
}
```

Here we have a component that returns a div with `msg` in it. This is the same component as the first one we wrote but look how much shorter and more concise it is. We don't have a class to worry about and dont have to worry about the `this` keyword and binding it properly.

But where is the *functional* part of it?

```js
const MyComponent = (props) => {
    const increment = () => {
        props.increment();
    }
    return React.createElement('div', null,
        React.createElement('button', { onClick: increment }, 'increment'),
        props.counter);
}

class App extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            counter: 0
        }
    }
    increment() {
        this.setState({ counter: this.state.counter + 1 });
    }
    render() {
        return React.createElement(
                MyComponent, 
                { increment: this.increment.bind(this), counter: this.state.counter  }
        )
    }
}
ReactDOM.render(
    React.createElement(App),
    document.getElementById('app')
);
```

See now that we can call functions that operate on the parent props from this component as well. Neat-o

## Higher Order Components

Higher Order Components are a pattern in React that looks a lot like the decorator pattern. It is useful in abstracting away functionality we see in multiple components. An **HOC** is implemented as a function that takes a React component as input, and outputs a react component.

`function(component): Component`.

Here's an example...

LOL API POC

Anyone that has used Redux has been using Higher Order Components. The `connect` function that connects state and props to the Redux store is a higher order component.


