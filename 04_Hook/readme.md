## Working with HOOk (State in Function component)- useState ##
useState is a hook that allows you to have state variables in functional components.

There are two types of components in React, class and functional components.

Class components are ES6 classes that extend from React.Component and can have state and lifecycle methods:
```js
class Message extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      message: ‘’    
    };
  }

  componentDidMount() {
    /* ... */
  }

  render() {
    return <div>{this.state.message}</div>;
  }
}
```
Functional components are functions that just accept arguments as the properties of the component and return valid JSX:
```js
function Message(props) {
  return <div>{props.message}</div>
}
// Or as an arrow function
const Message = (props) =>  <div>{props.message}</div>
```
As you can see, there are no state or lifecycle methods.

However, since React 16.8 we can use hooks, which are functions with names starting with use, to add state variables to functional components and instrument the lifecycle methods of classes.

This article is a guide to the useState (state) hook, the equivalent of this.state/this.setSate for functional components.

Declaring state

useState is a named export from react so to use it, you can write:

React.useState
Or to import it just write useState:
```js
import React, { useState } from 'react';
```
But unlike the state object that you can declare in a class, which allows you to declare more than one state variable, like this:
```js
import React from 'react';

class Message extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      message: '',
      list: [],    
    };
  }
  /* ... */
}
```
The useState hook allows you to declare only one state variable (of any type) at a time, like this:

import React, { useState } from 'react';

const Message= () => {
   const messageState = useState( '' );
   const listState = useState( [] );
}
useState takes the initial value of the state variable as an argument. You can pass it directly, as shown in the previous example, or use a function to lazily initialize the variable (useful when the initial state is the result of an expensive computation):

const Message= () => {
   const messageState = useState( () => expensiveComputation() );
   /* ... */
}
The initial value will be assigned only on the initial render (if it’s a function, it will be executed only on the initial render).

In subsequent renders (due to a change of state in the component or a parent component), the argument of the useState hook will be ignored and the current value will be retrieved.

It is important to keep this in mind because, for example, if you want to update the state based on the new properties the component receives:

const Message= (props) => {
   const messageState = useState( props.message );
   /* ... */
}
Using useState alone won’t work because its argument is used the first time only, not every time the property changes (look here for the right way to do this).

But useState doesn’t return just a variable as the previous examples imply. It returns an array, where the first element is the state variable and the second element is a function to update the value of the variable:

const Message= () => {
   const messageState = useState( '' );
   const message = messageState[0]; // Contains ''
   const setMessage = messageState[1]; // It’s a function
}
Usually, you’ll use array destructuring to simplify the code shown above:

const Message= () => {
   const [message, setMessage]= useState( '' );
}
This way, you can use the state variable in the functional component like any other variable:

const Message = () => {
  const [message, setMessage] = useState( '' );

  return (
    <p>
      <strong>{message}</strong>
    </p>
  );
};
But why does useState return array?

Because compared to an object, an array is more flexible and easy to use.

If the method returned an object with a fixed set of properties, you wouldn’t be able to assign custom names in an easy way. You’d have to do something like this (assuming the properties of the object are state and setState):

// Without using object destructuring
const messageState = useState( '' );
const message = messageState.state;
const setMessage = messageState

// Using object destructuring
const { state: message, setState: setMessage } = useState( '' );
const { state: list, setState: setList } = useState( [] );
Updating state

The second element returned by useState is a function that takes a new value to update the state variable.

Here’s an example that uses a text box to update the state variable on every change:

const Message = () => {
  const [message, setMessage] = useState( '' );

  return (
    <div>
      <input
         type="text"
         value={message}
         placeholder="Enter a message"
         onChange={e => setMessage(e.target.value)}
       />
      <p>
        <strong>{message}</strong>
      </p>
    </div>
  );
};
Try it here.

However, this update function doesn’t update the value right away. Rather, it enqueues the update operation. Then, after re-rendering the component, the argument of useState will be ignored and this function will return the most recent value.

If you use the previous value to update state, you must pass a function that receives the previous value and returns the new value:

const Message = () => {
  const [message, setMessage] = useState( '' );

  return (
    <div>
      <input
        type="text"
        value={message}
        placeholder="Enter some letters"
        onChange={e => {
          const val = e.target.value;
          setMessage(prev => prev + val)
        } }
      />
      <p>
        <strong>{message}</strong>
      </p>
    </div>
  );
};
Try it here.

However, there are two important things to know about updates.

First, if you use the same value as the current state to update the state (React uses Object.is for comparing), React won’t trigger a re-render.

For example, when working with objects, it’s easy to make the following mistake:

const Message = () => {
  const [messageObj, setMessage] = useState({ message: '' });

  return (
    <div>
      <input
        type="text"
        value={messageObj.message}
        placeholder="Enter a message"
        onChange={e => {
          messageObj.message = e.target.value;
          setMessage(messageObj); // Doesn't work
        }}
      />
      <p>
        <strong>{messageObj.message}</strong>
      </p>
  </div>
  );
};
Try it here.

Instead of creating a new object, the above example mutates the existing state object. To React, that’s the same object.

To make it work, a new object must be created:

onChange={e => {
  const newMessageObj = { message: e.target.value };
  setMessage(newMessageObj); // Now it works
}}
This leads us to the second important thing you need to remember.

When you update a state variable, unlike this.setState in a component class, the function returned by useState does not automatically merge update objects, it replaces them.

Following the previous example, if we add another property to the message object (id):

const Message = () => {
  const [messageObj, setMessage] = useState({ message: '', id: 1 });

  return (
    <div>
      <input
        type="text"
        value={messageObj.message}
        placeholder="Enter a message"
        onChange={e => {
          const newMessageObj = { message: e.target.value };
          setMessage(newMessageObj); // id property is lost
        }}
      />
      <p>
        <strong>{messageObj.id} : {messageObj.message}</strong>
      </p>
  </div>
  );
};
The new property is lost.

Try it here.

You can replicate this behavior by using the function argument and the object spread syntax:

onChange={e => {
  const val = e.target.value;
  setMessage(prevState => {
    return { ...prevState, message: val }
  });
}}
This will have the same result as Object.assign, the ...prevState part will get all of the properties of the object and the message: val part will overwrite the message property.

For this reason, the React documentation recommends splitting the state into multiple state variables based on which values tend to change together.

Track state and user interaction with components

It’s important to validate that everything works in your production React app as expected. If you’re interested in monitoring and tracking issues related to components AND seeing how users interact with specific components, try LogRocket. LogRocket Dashboard Free Trial Bannerhttps://logrocket.com/signup/

LogRocket is like a DVR for web apps, recording literally everything that happens on your site. The LogRocket React plugin allows you to search for user sessions where the user clicks a specific component in your app. With LogRocket you can understand how users interact with components, and surface any errors related to components not rendering.

In addition, LogRocket logs all actions and state from your Redux stores. LogRocket instruments your app to record requests/responses with headers + bodies. It also records the HTML and CSS on the page, recreating pixel-perfect videos of even the most complex single-page apps.

Modernize how you debug your React apps – Start monitoring for free.

Rules for using the state hook

useState follows the same rules that all hooks do:

Only call hooks at the top level
Only call hooks from React functions
The second rule is easy to follow. Don’t use useState in a class component:

class App extends React.Component {
  render() {
    const [message, setMessage] = useState( '' );

    return (
      <p>
        <strong>{message}</strong>
      </p>
    );
  }
}
Or regular JavaScript functions (not called inside a functional component):

function getState() {
  const messageState = useState( '' );
  return messageState;
}
const [message, setMessage] = getState();
const Message = () => {
 /* ... */
}
You’ll get an error.

The first rule means that, even inside functional components, you shouldn’t call useState in loops, conditions, or nested functions because React relies on the order in which useState functions are called to get the correct value for a particular state variable.

In that regard, the most common mistake is to wrap useState calls in a conditional statement (they won’t be executed all the time):

if (condition) { // Sometimes it will be executed, making the order of the useState calls change
  const [message, setMessage] = useState( '' );
  setMessage( aMessage );  
}
const [list, setList] = useState( [] );
setList( [1, 2, 3] );
A functional component can have many calls to useState or other hooks. Each hook is stored in a list, and there’s a variable that keeps track of the currently executed hook.

When useState is executed, the state of the current hook is read (or initialized during the first render), and then, the variable is changed to point to the next hook. That’s why it is important to always maintain the hook calls in the same order, otherwise, a value belonging to another state variable could be returned.

In general terms, here’s an example of how this works step by step:

React initializes the list of hooks and the variable that keeps track of the current hook
React calls your component for the first time
React finds a call to useState, creates a new hook object (with the initial state), changes the current hook variable to point to this object, adds the object to the hooks list, and return the array with the initial state and the function to update it
React finds another call to useState and repeats the actions of the previous step, storing a new hook object and changing the current hook variable
The component state changes
React sends the state update operation (performed by the function returned by useState) to a queue to be processed
React determines it needs to re-render the component
React resets the current hook variable and calls your component
React finds a call to useState, but this time, since there’s already a hook at the first position of the list of hooks, it just changes the current hook variable and returns the array with the current state and the function to update it
React finds another call to useState and since a hook exists in the second position, once again, it just changes the current hook variable and returns the array with the current state and the function to update it
If you like to read code, ReactFiberHooks is the class where you can learn how hooks work under the hood.

Conclusion

useState is a hook (function) that allows you to have state variables in functional components. You pass the initial state to this function, and it returns a variable with the current state value (not necessarily the initial state) and another function to update this value.