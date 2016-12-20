Summary for this [article](https://facebook.github.io/react/docs/optimizing-performance.html).

React already did some "magic" to speed up your applications. But you still can do more.

Use The Production Build
----------------

The development build includes extra warnings that are helpful when building your apps, but it is slower due to the extra bookkeeping it does.

Avoid Reconciliation
----------------

React checkes ***virtual DOM*** to decide whether the elements will be re-rendered into ***actual DOM***.

The default implementation of the lifecycle function `shouldComponentUpdate` returns `true`.

You can override `shouldComponentUpdate` to return `false`, to skip the whole rendering process, including calling `render()` on this component and below.

shouldComponentUpdate In Action
----------------

SCU = shouldComponentUpdate

vDOMEq = whether the rendered React elements were equivalent

![shouldComponentUpdate In Action](http://upload-images.jianshu.io/upload_images/3835536-9c0feebd500d97bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. C2, SCU == false, no need to check vDOMEq. And no SCU for C4 or C5. Similar for C7.
2. C1, C3, SCU == true, React had to go down to the leaves and check them (Check SCU and vDOMEq)
  1. C6, SCU == true, no children, check vDOMEq, update DOM
  2. Update DOM for C3
  3. Update DOM for C1
3. C8, SCU == true, check vDOMEq, no DOM change, no update DOM.

Examples
---------------

1. Compare each props and states to determine if the component should update
2. Extract a "shallow comparison" function
3. Inherit from React.PureComponent

Problem, it only does a shallow comparison, it would not re-render if the props or state have been mutated in a way that a shallow comparison would miss.

The Power Of Not Mutating Data
----------------

Avoid mutating values

- Always return a new object
```javascript
var newObject = {
    words: this.state.words.concat(['marklar']),
    others: this.state.others,
    //...
};
this.setState(newObject);
```
OR
```javascript
this.setState(prevState => ({
    words: prevState.words.concat(['marklar']),
    others: this.state.others,
    //...
}));
```

- ES6 syntax sugur, [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)
```javascript
this.setState(prevState => ({
    ...prevState,
    words: [
        ...prevState.words,
        'marklar'
    ]
}));
```

- [Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign), ES6 or polyfill
```javascript
this.setState(prevState => Object.assign({}, prevState, {
    words: [
        ...prevState.words,
        'marklar'
    ]
}));
```

Using Immutable Data Structures
-------------------

[Immutable.js](https://github.com/facebook/immutable-js) is another way to solve this problem. It provides immutable, persistent collections that work via structural sharing:

1. *Immutable*: cannot be altered
2. *Persistent*: create new collections from a collection, the original collection will still be valid and not changed
3. *Structural Sharing*: share the same structure as much as possible

Summary
-------------------

Immutability makes tracking changes cheap. A change will always result in a new object so we only need to check if the reference to the object has changed.

Attachments
-------------------

Here is the pseudo implementation of `React.PureComponent.prototype.shouldComponentUpdate`

```javascript
shouldUpdate =
    !shallowEqual(prevProps, nextProps) ||
    !shallowEqual(inst.state, nextState);
```
Here is the pseudo implementation of `shallowEqual`
```javascript
function shallowEqual(objA, objB) {
    if objA === objB
        return true
    if each props of objA === each props of objB
        return true
    return false
}
```

Two other libraries that can help use immutable data are [seamless-immutable](https://github.com/rtfeldman/seamless-immutable) and [immutability-helper](https://github.com/kolodny/immutability-helper).