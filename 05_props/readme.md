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
Destructuring was introduced in ES6. It’s a JavaScript feature that allows us to extract multiple pieces of data from an array or object and assign them to their own variables.

Imagine you have a person object with the following properties:
```js
const person = {
  firstName: "Lindsay",
  lastName: "Criswell",
  city: "NYC"
}
```
Before ES6, you had to access each property individually:
```js
console.log(person.firstName) // Lindsay
console.log(person.lastName) // Criswell
console.log(person.city) // NYC
```
Destructuring lets us streamline this code:
```js
const { firstName, lastName, city } = person;
```
is equivalent to
```js
const firstName = person.firstName
const lastName = person.lastName
const city = person.city
```
So now we can access these properties without the person. prefix:
```js
console.log(firstName) // Lindsay
console.log(lastName) // Criswell
console.log(city) // NYC
```

### Destructing in Class Component ###
For example, if we have simple component, which in render function uses 4 different props.
```js
import React from "react";
class Row extends React.Component {
  state = {
    showEmail: false
  };
  render() {
    return (
      <div>
        <div>
          <span>First Name: {this.props.firstName}</span>
        </div>
        <div>
          <span>Last Name: {this.props.lastName}</span>
        </div>
        {this.state.showEmail && (
          <div>
            <span>Email: {this.props.email}</span>
          </div>
        )}
        <button onClick={this.props.doSomethingAmazing}>Click me</button>
      </div>
    );
  }
}
```
We could apply ES6 destructuring assignment to simplify code.
```js
import React from "react";
class Row extends React.Component {
  state = {
    showEmail: false
  };
  render() {
    const { firstName, lastName, email, doSomethingAmazing } = this.props;
    return (
      <div>
        <div>
          <span>First Name: {firstName}</span>
        </div>
        <div>
          <span>Last Name: {lastName}</span>
        </div>
        {this.state.showEmail && (
          <div>
            <span>Email: {email}</span>
          </div>
        )}
        <button onClick={doSomethingAmazing}>Click me</button>
      </div>
    );
  }
}
```
Note: I didn't destruct showEmail variable because in the render function I am only using one property one time from state.
In my mind function like this
```js
showAlert(){
    alert(this.state.showEmail)
}
```
reads more easily than
```js
showAlert(){
    const { showEmail } = this.state;
    alert(showEmail);
}
```
Especially if there is a lot of code between destructing and variable usage. Though I would destruct variable if I want to use one prop for it more than one time.
```js
showAlert(){
    const { showEmail } = this.state;
    alert(showEmail);
    alert(showEmail);
}
```
### Desctructing in Functional Component ###
The benefits of ES6 destructuring assignment looks even nicer in the function component.
If we would have a similar component without a state:
```js
import React from "react";
const Row = props => (
  <div>
    <div>
      <span>First Name: {props.firstName}</span>
    </div>
    <div>
      <span>Last Name: {props.lastName}</span>
    </div>
    <div>
      <span>Email: {props.email}</span>
    </div>
    <button onClick={props.doSomethingAmazing}>Click me</button>
  </div>
);
```
Applying props destructuring technique allows to write code like this:
```js
import React from "react";
const Row = ({ firstName, lastName, email, doSomethingAmazing }) => (
  <div>
    <div>
      <span>First Name: {firstName}</span>
    </div>
    <div>
      <span>Last Name: {lastName}</span>
    </div>
    <div>
      <span>Email: {email}</span>
    </div>
    <button onClick={doSomethingAmazing}>Click me</button>
  </div>
);
```
