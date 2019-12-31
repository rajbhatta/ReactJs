## Working with State in ReactJs using setState ##
**Note:** It's important to note that state can only be created in a class component. 

Passing only props from component to component doesn't make the component interactive, because we can not change the props. Props are read-only. In order to address this issue state in ReactJs is introduced.

The code below shows how to set up an empty constuctor. This should not be something we're putting into production code as we only want to use constructors if they are actually doing something. A constructor isn't needed for a class component to receive props, so unless you have state or have to bind a function you probably won't need it.
```js
import React, { Component} from 'react';
class Example extends Component {
  constructor(props) {
    super(props);
  }
  render() {
    return (
      ...
    )
  }   
}
```
Adding our state object is easy enough. Inside the constructor, after super(props);, just add this.state and set it equal to an empty object. Once we have created the empty object, we can fill it with data of whatever key and value pair we'd like. 
```js
import React, { Component} from 'react';
class Example extends Component {
  constructor(props) {
    super(props);
    this.state = {
      isHungry: true,
      topping: "Pepperoni",
      slices: 8
    }
  }
  render() {
    return (
      ...
    )
  }   
}
```

### How do we change the state ###
```js
this.setState({ item: "newValue" });
```

The code above calls a this.setState function and passes in an object with key-value pairs. If the key matches one we already have in state, it updates the value in state to the new value provided. If the key doesn't exist in state, it will be created with the given value.
```js
import React, { Component} from 'react';
class Example extends Component {
  constructor(props) {
    super(props);
    this.state = {
      isHungry: true,
      topping: "Pepperoni",
      slices: 8
    }
    this.eatSlice = this.eatSlice.bind(this);
  }
  eatSlice() {
    const totalSlices = this.state.slices - 1;
    this.setState({
      slices: totalSlices
    });
  }
  render() {
    return (
      ...
    )
  }   
}
```
If we assume this function will be fired when a button is clicked, then each time the user clicks that button our slices in state will go down by one (even into negatives because we have not created logic to prevent that). Each time the state changes from the button click, our component will re-render with the new data.

### Destructing state in ReactJs ###
