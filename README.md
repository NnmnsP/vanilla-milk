# Vanilla Milk
Simple library for building simple reactive web components.
  
## What is...
Vanilla Milk is a library which helps create to web component which can be used on anywhere! Embrace future of web-component made it future-proof. Built-in reactive state and props but also is very small and fast.

Vanilla Milk feature:
* Reusable component.
* Simple way for creating web components.
* Milk DOM, a special implementation of DOM manipulation.
* Reactive state and props.
* No class, just function.
* Life-cycle hooks.
* Expressive Vanilla like API structure.
* Very small (1kB gzipped on production).
* Encapsulated style, ID and class name.
* Zero dependencies.
* TypeScript support.

## Milk DOM
Vanilla Milk doesn't use Virtual DOM, instead it has its own `Milk DOM` which share the same idea with Virtual Dom, update where neccessary. Unlike VDOM, Milk DOM use a collection of Native Browser API to perform diffing instead of creating long list of object which also prevent heavy recursion.
Drawback is diffing algorithm isn't as deep as Virtual DOM does but Milk DOM is blazing fast and has less memory consumption.
###### Note: Milk DOM is in an very early stage of development, you might experience some bug and when that time came, you can always open the issue on this repo.
  
## Getting started
Glad you're interested in! Let's get over quick start! Let us add library just for creating a component, after the component has been built, you don't have to install it anymore! Just moved the built components to your any project~

### Install Vanilla Milk
Let install this real quick with package manager of your choice.
```bash
// Using npm
npm install vanilla-milk --save

// Using yarn
yarn add vanilla-milk
```
Well done! Now you're ready to create a Milk Component! (Shortname for Vanilla Milk Component for the greater good.)
  
### Hello World! It's Milk Component!
Let's start by building `milk-component', a traditional hello world for programmer but as web component.

Milk Component are separated into 2 step:
1) Define it with `create`
2) Create it with `define`

Just 2 step, that's all for building reactive web component.

#### Create
First of all, import `create` function from vanilla-milk. This function is responsible to create a model of component.
```javascript
import { create } from "vanilla-milk"
```

First parameter is a callback to display HTML Element.
```javascript
import { create } from "vanilla-milk"

const MilkComponent = create((display) => 
	display(`<h1>Hello World! Milk Component!</h1>`)
)
```
We have just create `MilkComponent` which will return `<h1>Hello World</h1>`  

### Define
Next we just have to `define` this component, so it can work like every HTML Element.  
But first, import `define` from `vanilla-milk`
```javascript
import { define } from "vanilla-milk"
```

Now let's name the component `milk-element`, so it can be use in HTML. (Web Component need `-` to be declared, like `milk-component` which has `-` in it)
```javascript
import { create, define } from "vanilla-milk"

const MilkComponent = create((display) => 
	display(`<h1>Hello World! Milk Component!</h1>`)
)

define("milk-component", MilkComponent)
```
That's it! You have just created a Milk Component!! :tada::tada:
  
#### Milk Component in action
Soooo~ We have just created a web component, let's see it in action!  
Just add `<milk-component></milk-component>` in HTML. Make sure you've link the JavaScript file which contain milk component in it. You can use quick set-up like `parcel`.
```html
<!-- Any HTML here -->
<html>
<head>
	<title>Hello Milk Component</title>
    <script src="milk_component_here.js"></script>
</head>
<body>
	<milk-component></milk-component>
</body>
</html>
```
And it should display something like this.
    
#### But that's not what make Vanilla Milk special...
In Vanilla Milk, component are reactive which mean every time `state` or `props` changed, Milk Component will know it.

## State
Like `angular`, `react` and `vue`, Vanilla Milk also has `state`.  
State is like a variable which are responsible for storing data and display it to view.

Let's say we want to create a counter and display count to view. In Vanilla Milk we do like this.
  
If you're familiar with React, we have `useState`. But it's not like what you might think.
```javascript
import { useState } from 'vanilla-milk'
```
Vanilla Milk are designed to be expressive. So we separate the state, props and life-cycle to the second parameter of `create` like this.
```javascript
import { create, define, useState } from "vanilla-milk"

const MilkComponent = create((display, state) => {
	let { count } = state
    
	return display(`<h1>Count ${count}</h1>`)
},
{
	counter: useState(0)
})

define("milk-component", MilkComponent)
```
So, what's happend here?
  
The second parameter of `create` are responsible for handling data, so we can separate and focus on each part.
```
{
	count: useState(0)
}
```
Here we defined state name `count` with value of `0`. `useState` is a function that say, "Hey! We want a state name 'count' with value of 0" to Vanilla Milk or something like that. Vanilla Milk will just handle the rest of that and pass the `count` to the `second parameter` of the view callback.
```
const MilkComponent = create((display, state) => {
	console.log(state) // { count: 0 }
},
{
	count: useState(0)
})
```
Lastly, we display counter to the view.
```javascript
import { create, define, useState } from "vanilla-milk"

const MilkComponent = create((display, state) => {
	return display(`<h1>Count ${state.count}</h1>`)
},
{
	count: useState(0)
})

define("milk-component", MilkComponent)
```
Now that will display `count` to HTML, so... how do we set it in Vanilla Milk?
  
As an early state of development of Vanilla Milk that's where the last parameter of `create` kick in!  
We add button and attach event in to it, so we can change the value of `counter`. // Hey don't make face of disappointment like that! I'll fix it in some next update, I promise! It's quite hard to create something like that ya know.
  
Let add a button.
```javascript
import { create, define, useState } from "vanilla-milk"

const MilkComponent = create((display, state) => {
	return display(`
		<h1>Count ${state.count}</h1>
    	<button id="increase-count">Increase count</button>
	`)
},
{
	count: useState(0)
})

define("milk-component", MilkComponent)
```
Vanilla Milk is declarative, this is where the last parameter come in!  
You define any `events` you want here! It's take callback function so you're free to define everything like normal JavaScript here. Vanilla Milk called this parameters as `useEvents`.
```javascript
import { create, define, useState } from "vanilla-milk"

const MilkComponent = create((display, state, _, events) => {
	console.log(events) // { increaseCount: "increaseCount" }

	return display(`
		<h1>Count ${state.count}</h1>
    	<button id="increase-count" @click="${events.increaseCount}">Increase count</button>
	`)
},
{
	count: useState(0)
},
([state, set], _) => {
	let increaseCount = () => set.count(state.count + 1)
    
    return { increaseCount }
})

define("milk-component", MilkComponent)
```
`useEvents` take an callback which pass `[state, set]` which are used to access state and set the state. Receive `props` and second parameter. 
###### *props will be explained in next section.
```javascript
([state, set], props) => {
	let increaseCount = () => set.count(state.count + 1)
    
    return { increaseCount }
})
```
You might notice something there's `@click` in `button`, Vanilla Milk use pseudo attribute and match event to the node. This pseudo attribute take an export events name in `useEvents`.
```javascript
<button id="increase-count" @click="${events.increaseCount}">
    Increase count
</button>
```
Now everytime the button that as `@click` will run the function match to exports in `useEvents`, in this case it's `increaseCount`
```javascript
let increaseCount = () => set.count(state.count + 1)

return { increaseCount }
```
At here we get value of `count` from `state.count` and set it with `set.count` which came out with something like this:
```javascript
set.count(0 + 1) // Count is now 1
```
Now it should increase the value of count everytime we click it!

That's it! You've just achieve an advance concept like state of Vanilla Milk :tada::tada:

## Props
Contrast to state, it's super simple. props is shorten for `properties`. A property is like a HTML attribute. For a button like:
```html
<button class="nice-button">Hello! I'm a button</button>
```
class is a props of this button and it's value is "nice-button".
  
As Milk Component, we also can have something like that! and guess what? You can create any attribute you like effortlessly!
  
All you have to do is import `useProps` from `vanilla-milk` and set it like `useState`.
```javascript
import { useProps } from "vanilla-milk"
```
Now let's say that we want to have a prop name `hello`. Like `useState` we do it like this:
```javascript
{
	count: useState(0),
	hello: useProps()
}
```
And access it like state
```javascript
create((display, state, props) => {
	console.log(props.hello) // null if there's no value passed
})
```
So let's define the component real quick to see how it work.
```javascript
import { create, define, useProps } from "vanilla-milk"

const MilkComponent = create((display, state, props) => {
	return display(`<h1>hello ${props.hello}</h1>`)
},{
	hello: useProps()
})

define("milk-component", MilkComponent)
```
And in HTML
```html
<milk-component hello="world"></milk-component> // hello world
```
The reason we have to define `props` so that Vanilla Milk can just update and listen only to the props you used! 
  
But there's one special props which is always available... `children`
Children is any text or HTML Element between the Milk Component.
In HTML:
```html
<milk-component>Hello World</milk-component>
```
And access it:
```javascript
const MilkComponent = create((display, state, props) => {
	console.log(props.children) // Hello World
})
```
That's it! Unlike `useState`, `useProps` is super to use! :tada::tada:

## useEffect
Like React, you can defined lifecycle with `useEffect`.
  
Let's say, that you want to `console.log()` everytime prop `hello` change. You just have to import `useEffect` from `vanilla-milk`.
```javascript
import { useEffect } from "vanilla-milk"
```
Again, add it to second paramter of `create` like `useState` and `useProps`
```javascript
{
	hello: useProps(),
	logHello: useEffect((state, props) => {
		console.log(props.hello) // Log everytime hello changed
    }, ["hello"])
}
```
Notice that we add array contain "hello" as string as the second parameter of useEffect.
Because we can't directly access value of hello so we just add it as string, Vanilla Milk will take care of the rest (In fact, we can add it as variable but it takes an extra effort and performance cost).

You can express name of `useEffect` as anything, It just easier to remind you lifecycle with name if there are too much lifecycle.

Now we hello is changed, first callback will run. Like view callback, useEffect is paired with `(state, props)`. You can directly access it in the callback.

`useEffect` is not only limited to state but props too! You can also add `useEffect` as much as you want or many listener as much as you need!
```javascript
// State and Props
{
    hello: useProps(),
    count: useState(),
	logHello: useEffect((state, props) => {
    	console.log(props)
    }, ["count"])
}
  
// Multiple listener
{
    hello: useProps(),
    count: useProps(),
	logHello: useEffect((state, props) => {
    	console.log(props)
    }, ["count", "count"])
}
```
  
## Milk DOM In depth
Milk DOM cover a lot of thing under the cover but the main concept is to compare and diff DOM Node aimming for lesser iteration as possible (Same idea with VDOM). Instead of constructing an object node representation of DOM for comparing between two. Vanilla Milk heavily use Native Browser API comparing between each node instead of DOM and update it directly instead.

Since Milk DOM is written in template string, it's very hard for Vanilla Milk to identify which textNode should replace Node or vice-versa. In this case, you shouldn't replace existed dom with blank text ("") like how React, Vue, other library does (which will sometime, occured an unexpected error). Instead Vanilla Milk offter a way to replace Node with blank Node by putting any element with attribute of `__hidden__`
```javascript
const MilkCard = create(
	(display, { isOpen }, { title, cover, children }, events) => {
		return display(`
			<main id="card" tabIndex=0>
				<img id="cover" src="${cover}" alt="${title}" />
				<section id="body">
					<header id="header">
						<h1 id="title">${title}</h1>
						<button id="toggle" @click="${events.toggleCard}">+</button>
					</header>
					${
						isOpen
							? `<section id="detail">${children}</section>`
							: `<input @hidden />`
					}
				</section>
			</main>
		`)
	},
	{
		title: useProps(),
		cover: useProps(),
		isOpen: useState(false),
		cardStyle: useStyleSheet("/card.css")
	},
	([state, set], props) => {
		let toggleCard = () => set.isOpen(!state.isOpen)

		return { toggleCard }
	}
)

```

Milk DOM's diffing algorithm are seperated into 3 parts.
* Diff - Diffing for the different attribute, child node, etc.
* Hard Diff - When tagName are different, whole element node have to be replaced. Hard Diff take care of that preventing child node for being recursion.
* Text Diff - Diffing for text different.
  
### Best practice
To prevent an unexpected update, it's recommend to contained textNode in an element where textNode is state.
```javascript
import { create, useState } from "vanilla-milk"

// Please don't
let notSoGood = create((display, { counter }) => {
	return display(`
		<section>
			<h1>Counter: </h1>
			{counter}
		</section>
	`)
})

// Better~
let wellDone = create((display) => {
	return display(`
		<section>
			<h1>
				Counter: {state}
			</h1>
		</section>
	`)
})
```
Not only that storing textNode in an element are safer but also has better performance from prevent recursion between each node.
  
### Multiple node on root
Unlike VDOM, Vanilla Milk use native DOM for diffing freeing root node from stucking with only one node on the top.

```javascript
import { create } from "vanilla-milk"

// This is ok
let multipleRootNode = create((display, { counter }) => {
	return display(`
		<h1>Hello! I'm the root node!</h1>
		<h2>No way! I'm the root node too!</h2>
		<p>It's ok to use multiple root node in Vanilla Milk</p>
	`)
})
```

## Encapsulation
Vanilla Milk use Shadow DOM to encapsulate logic and style. Preventing an unexpected change while working in multiple library.
  
To use external stylesheet in Milk Component, you can assign stylesheet in the component instead
```javascript
import { create, useStyleSheet } from "vanilla-milk"

// Link stylesheet in component.
let coolCard = create((display, { counter }) => {
	return display(`
		<div class="card"></div>
	`)
},
{
	cardStyle: useStyleSheet("/card.css")
})

// Or even define style tag inside
let coolCard = create(
	(display) => display(`
		<style>
			.card {
				display: flex;
				flex-direction: column;
				width: 300px;
				padding: 20px 12px;
				borderRadius: 4px;
				box-shadow: 0 4px 8px rgba(0,0,0,.25);
			}
		</style>
		<div class="card">
		</div>
	`)
)
```
  
That's pretty much it of Vanilla Milk. :tada::tada: