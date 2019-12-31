## Conditional Rendering in ReactJS ##
### IF ELSE IN REACT ###
The easiest way to have a conditional rendering is to use an if else in React in your render method. Imagine you don't want to render your component, because it doesn't have the necessary props. For instance, a List component shouldn't render the list when there is no list of items. You can use an if statement to return earlier from the render lifecycle.
```js
function List({ list }) {
  if (!list) {
    return null;
  }
  return (
    <div>
      {list.map(item => <ListItem item={item} />)}
    </div>
  );
}
```
A component that returns null will render nothing. However, you might want to show a text when a list is empty to give your app user some feedback for a better user experience.
```js
function List({ list }) {
  if (!list) {
    return null;
  }
  if (!list.length) {
    return <p>Sorry, the list is empty.</p>;
  } else {
    return (
      <div>
        {list.map(item => <ListItem item={item} />)}
      </div>
    );
  }
}
```
The List renders either nothing, a text or the list of items. The if-else statement is the most basic option to have a conditional rendering in React.

### TERNARY OPERATION IN REACT ###
You can make your if-else statement more concise by using a ternary operation.
```js
condition ? expr1 : expr2
```
For instance, imagine you have a toggle to switch between two modes, edit and view, in your component. The derived condition is a simple boolean. You can use the boolean to decide which element you want to return.
```js
function Item({ item, mode }) {
  const isEditMode = mode === 'EDIT';
  return (
    <div>
      { isEditMode
        ? <ItemEdit item={item} />
        : <ItemView item={item} />
      }
    </div>
  );
}
```
If your blocks in both branches of the ternary operation are getting bigger, you can use parentheses.
```js
function Item({ item, mode }) {
  const isEditMode = mode === 'EDIT';
  return (
    <div>
      {isEditMode ? (
        <ItemEdit item={item} />
      ) : (
        <ItemView item={item} />
      )}
    </div>
  );
}
```
The ternary operation makes the conditional rendering in React more concise than the if-else statement. It is simple to inline it in your return statement.

### LOGICAL && OPERATOR IN REACT ###
It happens often that you want to render either an element or nothing. For instance, you could have a LoadingIndicator component that returns a loading text or nothing. You can do it in JSX with an if statement or ternary operation.
```js
function LoadingIndicator({ isLoading }) {
  if (isLoading) {
    return (
      <div>
        <p>Loading...</p>
      </div>
    );
  } else {
    return null;
  }
}
function LoadingIndicator({ isLoading }) {
  return (
    <div>
      { isLoading
        ? <p>Loading...</p>
        : null
      }
    </div>
  );
}
```

### SWITCH CASE OPERATOR IN REACT ###
Now there might be cases where you have multiple conditional renderings. For instance, the conditional rendering could apply based on different states. Let's imagine a notification component that can render an error, warning or info component based on the input state. You can use a switch case operator to handle the conditional rendering of these multiple states.
```js
function Notification({ text, state }) {
  switch(state) {
    case 'info':
      return <Info text={text} />;
    case 'warning':
      return <Warning text={text} />;
    case 'error':
      return <Error text={text} />;
    default:
      return null;
  }
}
```
Please note that you always have to use the default for the switch case operator. In React a component always has to return an element or null.
```js
function Notification({ text, state }) {
  switch(state) {
    case 'info':
      return <Info text={text} />;
    case 'warning':
      return <Warning text={text} />;
    case 'error':
      return <Error text={text} />;
    default:
      return null;
  }
}
Notification.propTypes = {
   text: React.PropTypes.string,
   state: React.PropTypes.oneOf(['info', 'warning', 'error'])
}
```
An alternative way would be to inline the switch case. Therefore you would need a self invoking JavaScript function.
```js
function Notification({ text, state }) {
  return (
    <div>
      {(function() {
        switch(state) {
          case 'info':
            return <Info text={text} />;
          case 'warning':
            return <Warning text={text} />;
          case 'error':
            return <Error text={text} />;
          default:
            return null;
        }
      })()}
    </div>
  );
}
```
Again it can be more concise with an ES6 arrow function.
```js
function Notification({ text, state }) {
  return (
    <div>
      {(() => {
        switch(state) {
          case 'info':
            return <Info text={text} />;
          case 'warning':
            return <Warning text={text} />;
          case 'error':
            return <Error text={text} />;
          default:
            return null;
        }
      })()}
    </div>
  );
}
```

### CONDITIONAL RENDERING WITH ENUMS ###
In JavaScript an object can be used as an enum when the object is used as a map of key value pairs.
```js
const ENUM = {
  a: '1',
  b: '2',
  c: '3',
};
```
An enum is a great way to have multiple conditional renderings. Let's consider the notification component again. This time we can use the enum as inlined object.
```js
function Notification({ text, state }) {
  return (
    <div>
      {{
        info: <Info text={text} />,
        warning: <Warning text={text} />,
        error: <Error text={text} />,
      }[state]}
    </div>
  );
}
```
The state property key helps us to retrieve the value from the object. That's neat, isn't it? It is much more readable compared to the switch case operator.

In this case we had to use an inlined object, because the values of the object depend on the text property. That would be my recommended way anyway. However, if it wouldn't depend on the text property, you could use an external static enum too.
```js
const NOTIFICATION_STATES = {
  info: <Info />,
  warning: <Warning />,
  error: <Error />,
};
function Notification({ state }) {
  return (
    <div>
      {NOTIFICATION_STATES[state]}
    </div>
  );
}
```
Although we could use a function to retrieve the value, if we would depend on the text property.
```js
const getSpecificNotification = (text) => ({
  info: <Info text={text} />,
  warning: <Warning text={text} />,
  error: <Error text={text} />,
});
function Notification({ state, text }) {
  return (
    <div>
      {getSpecificNotification(text)[state]}
    </div>
  );
}
```
After all, the enum approach in comparison to the switch case statement is more readable. Objects as enums open up plenty of options to have multiple conditional renderings. Consider this last example to see what's possible:
```js
function FooBarOrFooOrBar({ isFoo, isBar }) {
  const key = `${isFoo}-${isBar}`;
  return (
    <div>
      {{
        ['true-true']: <FooBar />,
        ['true-false']: <Foo />,
        ['false-true']: <Bar />,
        ['false-false']: null,
      }[key]}
    </div>
  );
}
FooBarOrFooOrBar.propTypes = {
   isFoo: React.PropTypes.boolean.isRequired,
   isBar: React.PropTypes.boolean.isRequired,
}
```
 ### MULTI-LEVEL CONDITIONAL RENDERING IN REACT ###
What about nested conditional renderings? Yes, it is possible. For instance, let's have a look at the List component that can either show a list, an empty text or nothing.
```js
function List({ list }) {
  const isNull = !list;
  const isEmpty = !isNull && !list.length;
  return (
    <div>
      { isNull
        ? null
        : ( isEmpty
          ? <p>Sorry, the list is empty.</p>
          : <div>{list.map(item => <ListItem item={item} />)}</div>
        )
      }
    </div>
  );
}
// Usage
<List list={null} /> // <div></div>
<List list={[]} /> // <div><p>Sorry, the list is empty.</p></div>
<List list={['a', 'b', 'c']} /> // <div><div>a</div><div>b</div><div>c</div><div>
```
It works. However I would recommend to keep the nested conditional renderings to a minimum. It makes it less readable. My recommendation would be to split it up into smaller components which themselves have conditional renderings.
```js
function List({ list }) {
  const isList = list && list.length;
  return (
    <div>
      { isList
        ? <div>{list.map(item => <ListItem item={item} />)}</div>
        : <NoList isNull={!list} isEmpty={list && !list.length} />
      }
    </div>
  );
}
function NoList({ isNull, isEmpty }) {
  return (!isNull && isEmpty) && <p>Sorry, the list is empty.</p>;
}
```
Still, it doesn't look that appealing. Let's have a look at higher order components and how they would help to tidy it up.

### WITH HIGHER ORDER COMPONENTS ###
Higher order components (HOCs) are a perfect match for conditional rendering in React. HOCs can have multiple use cases. Yet one use case could be to alter the look of a component. To make the use case more specific: it could be to apply a conditional rendering for a component. Let's have a look at a HOC that either shows a loading indicator or a desired component.
```js
// HOC declaration
function withLoadingIndicator(Component) {
  return function EnhancedComponent({ isLoading, ...props }) {
    if (!isLoading) {
      return <Component { ...props } />;
    }
    return <div><p>Loading...</p></div>;
  };
}
// Usage
const ListWithLoadingIndicator = withLoadingIndicator(List);
<ListWithLoadingIndicator
  isLoading={props.isLoading}
  list={props.list}
/>
```
In the example, the List component can focus on rendering the list. It doesn't have to bother with a loading state. Ultimately you could add more HOCs to shield away multiple conditional rendering edge cases.

A HOC can opt-in one or multiple conditional renderings. You could even use multiple HOCs to handle several conditional renderings. After all, a HOC shields away all the noise from your component. If you want to dig deeper into conditional renderings with higher order components, you should read the conditional rendering with HOCs article.

### EXTERNAL TEMPLATING COMPONENTS ###
Last but not least there exist external solutions to deal with conditional renderings. They add control components to enable conditional renderings without JavaScript in JSX. Then it is not question anymore on how to use if else in React.
```js
<Choose>
  <When condition={isLoading}>
    <div><p>Loading...</p></div>
  </When>
  <Otherwise>
    <div>{list.map(item => <ListItem item={item} />)}</div>
  </Otherwise>
</Choose>
```