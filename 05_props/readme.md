## Working with Props in React JS ##
To pass the data as params (parameters) from one React component to another component? That's because you don't want to have a component rendering static data, but pass dynamic data to your component instead. That's where React's props come into play. 

### Props in Functional component ###
**Note:** If we need to use Props in functional component then we have to use 
```js
props.propName
```
**Example:**
```js
import React, { Component } from 'react';
class App extends Component {
  render() {
    const greeting = 'Welcome to React';
    return (
      <div>
        <Greeting greeting={greeting} />
      </div>
    );
  }
}
const Greeting = props => <h1>{props.greeting}</h1>;
export default App;
```

### Props in Class component ###
**Note:** If we need to use Props in class component then we have to use 
```js
this.props.propName
```
**Example:**
```js
import React, { Component } from 'react';
class App extends Component {
  render() {
    const greeting = 'Welcome to React';
    return (
      <div>
        <Greeting greeting={greeting} />
      </div>
    );
  }
}
class Greeting extends Component {
  render() {
    return <h1>{this.props.greeting}</h1>;
  }
}
export default App;
```


### Destructing props in RectJs ###
