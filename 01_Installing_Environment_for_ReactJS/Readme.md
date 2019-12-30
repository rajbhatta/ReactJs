## Installing Required Environment for ReactJS ##

### 1. Install Node from https://nodejs.org/en/download/ ###

### 2. Use node package manager npm to create react project and run react project ###

1. Goto Visual Studio code
2. Goto Terminal inside Visual Studio Code
3. Use create-react-app generator for creating a React App for that goto Visual Studio Code terminal or command prompt to type type
```
npm install -g create-react-app
create-react-app my-app
cd my-app
npm start
```
or 
```
npx create-react-app my-app
cd my-app
npm start
```

### 3. Snapshot of created project in MacOS ###
<img src="my-app/public/create-app.png"/>

## 3.1 Folder Directory in ReactJS ##
**1. public folder:** Work with index.html for single page application and it is starting point. Where we have defined 
```
<div id="root"></div> 
```
<img src="my-app/public/index-img.png"/>

**2. src:** starting poing 
We specify the root component  that is App component and DOM element which will be controlled by react app. In our case
```
ReactDOM.render(<App />, document.getElementById('root'));
```
which is defined inside index.js
<img src="my-app/public/app-img.png"/>

**3. App component represents the view that we see in the browser** 
```
import React, { Component } from 'react';
import './App.css';

class App extends Component
{
  render(){
    <h3>Hello Raj Bhatta</h3>
  }
}

export default App;
```
<img src="my-app/public/main-img.png"/>
