Summary for [Handling Events](https://facebook.github.io/react/docs/handling-events.html).

React events vs DOM events. There are some syntactic differences:

1. CamelCase, for example, onClick
2. Pass function, rather than a string
3. Call `preventDefault` to prevent default behavior, `return false` doesn't work

React defines these synthetic events according to the [W3C spec](https://www.w3.org/TR/DOM-Level-3-Events/). See the [SyntheticEvent
](https://facebook.github.io/react/docs/events.html)reference guide to learn more.

Be careful about the meaning of `this` in JSX callbacks. To ensure `this` can be bound, we usually do like these:

- [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

```javascript
render() {
  return (<InnerComp onClick={(e) => this.handleClick(e)}> Click me </InnerComp> ); 
}
```

- [Function.prototype.bind()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind)

```javascript
render() {
  return (<InnerComp onClick={this.handleClick.bind(this, e)}> Click me </InnerComp> ); 
}
```

Think about this, to `<InnerComp>`, `this.props.onClick` would change each time, the components might do an extra re-rendering.

So, the better way maybe these:

1. The experimental [property initializer syntax](https://babeljs.io/docs/plugins/transform-class-properties/)
2. Binding in the constructor, for example:
```javascript
class Toggle extends React.Component { 
  constructor(props) { 
    super(props); 
    // This binding is necessary to make `this` work in the callback 
    this.handleClick = this.handleClick.bind(this); 
  }
}
```
