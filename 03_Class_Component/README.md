## Working with Class Component in ReactJS ##
Creating a class component is pretty simple; just define a class that extends Component and has a render function.
```js
// MyComponent.js
import React, { Component } from 'react';

class MyComponent extends Component {
  render() {
    return (
      <div>This is my component.</div>
    );
  }
}

export default MyComponent;
```

From there, you can use it in any other component.
```js
// MyOtherComponent.js
import React, { Component } from 'react';
import MyComponent from './MyComponent';

class MyOtherComponent extends Component {
  render() {
    return (
      <div>
        <div>This is my other component.</div>
        <MyComponent />
      </div>
    );
  }
}

export default MyOtherComponent;
```
You don't have to define one component per file, but it's probably a good idea.

Using props

As is, MyComponent isn’t terribly useful; it will always render the same thing. Luckily, React allows props to be passed to components with a syntax similar to HTML attributes.
```html
<MyComponent myProp="This is passed as a prop." />
```
Props can then be accessed with this.props.
```js
class MyComponent extends Component {
  render() {
    const {myProp} = this.props;
    return (
      <div>{myProp}</div>
    );
  }
}
```
A variable wrapped in brackets will be rendered as a string.

Using state

One of the benefits class components have over functional components is access to component state.
```js
class MyComponent extends Component {
  render() {
    const {myState} = this.state || {};
    const message = `The current state is ${myState}.`;
    return (
      <div>{message}</div>
    );
  }
}
```
But since it wasn’t set anywhere, this.state will be null, so this example isn’t really useful. Which brings us to…

Using lifecycle hooks

Class components can define functions that will execute during the component’s lifecycle. There are a total of seven lifecycle methods: ```
componentWillMount, componentDidMount, componentWillReceiveProps, shouldComponentUpdate, componentWillUpdate, componentDidUpdate, and componentWillUnmount. 
```
For the sake of brevity, only one will be demonstrated.
```js
class MyComponent extends Component {
  // Executes after the component is rendered for the first time
  componentDidMount() {
    this.setState({myState: 'Florida'});
  }

  render() {
    const {myState} = this.state || {};
    const message = `The current state is ${myState}.`;
    return (
      <div>{message}</div>
    );
  }
}
```
this.state should not be assigned directly. Use this.setState, instead.

this.setState cannot be used in render.
