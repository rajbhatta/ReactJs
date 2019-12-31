## Event Handling in ReactJS ##
```js
class LoudButton extends Component {
  handleClick = () => {
    alert('clicked.');
  }
  render() {
    return <button onClick={this.handleClick}>Click Me</button>
  }
}
```
### Point to be noted during the Event handling

Firstly:
  camelCase vs lowercase

You probably noticed that the onClick attribute is camelCase instead of lowercase:
```js
// React Code
<button
  onclick={handleClick()}     // ok method
  onClick={this.handleClick}  // Excellent method
>
```
