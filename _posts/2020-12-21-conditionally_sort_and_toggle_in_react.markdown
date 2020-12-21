---
layout: post
title:      "Conditionally Sort and Toggle in React"
date:       2020-12-21 22:23:35 +0000
permalink:  conditionally_sort_and_toggle_in_react
---


When migrating from vanilla JavaScript to React, I regretted the loss of the DOM manipulation skills I worked hard to develop  during the JavaScript module of the Flatiron curriculum.  However, while working on my final React/Redux project, I was able to learn how to conditionally render using `state` and the ternary operator in my `render()` as a way to continue to manipulate the DOM without having to refresh the page.  

To begin, unless you're using React hooks, you will need to be in a class component in order to set the state.  In the example we're going to use here, it is from the `Trades` component of my trade journal app.  Originally, the `props.trades` were handed down from the parent `Home` container component.  The `Trades` component was more of a presentational component, which rendered all the trade entries from the database and displayed them in order of the database-generated `id`.  A simple `this.props.trades.map` was used to iterate over the objects (trades) in the trades array and present them onto the screen.  

However, what if we wanted a button that would display the trades in alphabetical order of their ticker symbol (which is a string)?  Moreover, what if we wanted to merely hit the button again and have it un-sorted?  Let's see...

First, since we're in a class component, we can use our local `state`.  Let's start out by declaring a state with a simple boolean data type:

```
state = {
  toggle: false
}

```

Next, we will create a `<button>` somewhere in the `render()` to trigger a state change:

```
<button onClick={this.handleToggle}>ReSort</button>
```

And you can already see that the next step will be to create a function within our class to "handle the toggle."

```
handleToggle = () => {
  this.setState(prevState => ({
    toggle : !prevState.toggle
  })
  )
}
```

There are a few interesting things happening in this simple function.  We are using our React `.setState()` function to change the `state` which is proper React form.  Indeed, although one can alter `state` without using `.setState()`, convention keeps us from doing so in order to avoid any surprises or uncontroller behavior.  Also, by using `prevState`  we are making sure that we are updating the correct version of `state` which may or may not already be updated since React updates `this.state` as well as `this.props` asynchronously.  So, as written, `handleToggle()` is taking the boolean value of `toggle` and updating it with its opposite value.

Next, we need our alphabetical `sort` feature which we can take directly from the MDN website here:
```
// sort by name
items.sort(function(a, b) {
  var nameA = a.name.toUpperCase(); // ignore upper and lowercase
  var nameB = b.name.toUpperCase(); // ignore upper and lowercase
  if (nameA < nameB) {
    return -1;
  }
  if (nameA > nameB) {
    return 1;
  }
```

Since we want to return an array, we need to modify this code a bit.  First, lets name it: `alphaSort` and then change the `var` to `let` since `var` has a lot of extra hoisting baggage attached to it.  Also, replace their `items` variable with our own `this.props.trades`.  Then, we need to actually return an array, so we wrap the entire function into a `return` statement.  However, if we put a `debugger` on our page, we will notice that we can't un-sort `this.props.trades` - this is not what we want.  We want to be able to toggle it back and forth to its original state.  What went wrong?  Unlike `.map`, `.sort` is destructive.  It remedy this: the spred operator.  `[...this.props.trades].sort` is the way to accomplish this.  So now our function looks like this:

```
alphaSort = () => {
  
  return (
  [...this.props.trades].sort(function(a, b) {
    let nameA = a.ticker.toUpperCase(); // ignore upper and lowercase
    let nameB = b.ticker.toUpperCase(); // ignore upper and lowercase
    if (nameA < nameB) {
      return -1;
    }
    if (nameA > nameB) {
      return 1;
    }
  
    // names must be equal
    return 0;
  }));
}
```

The tricky part of this comes now at the point of conditionally rendering the results.  Originally in our `render()` the iteration looked like this: 
```
 <ul>
    {this.props.trades.map(trade =>
     <li key={trade.id}>
     
     <Link to={`/trades/${trade.id}`}>{trade.ticker} - {trade.strategy}</Link>
     </li>)}
   </ul>
```

In order to get the correct `this.props.trade` array to iterate over, we make the following change preceding the `.map` statement:
```
(this.state.toggle ? this.alphaSort() : this.props.trades).map
```
Our ternary operator lets us use an If/Then statement in JSX to now easily render conditionally onto the page.

I hope this helps make for easier conditional rendering and toggling in your React code.
