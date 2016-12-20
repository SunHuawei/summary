Summary for [Context](https://facebook.github.io/react/docs/context.html)

Context
-------------

Context can let you pass data through the component tree without having to pass the props down manually at every level.


Why Not To Use Context
-------------

1. The vast majority of applications do not need to use context.
2. It is an experimental API
3. Redux may be your right solution
4. If you aren't an experienced React developer, don't use context.


Try to isolate your use of context, if you insist on using context.

How To Use Context
-------------

By adding `childContextTypes` and `getChildContext` to the context provider, and any component in the subtree can access it by defining `contextTypes`.

If `contextTypes` is not defined, then context will be an empty object.


Parent-Child Coupling
-------------

Context can also let you build an API where parents and children communicate.

For example, in [React Router V4](https://react-router.now.sh/basic), by passing down some information from the Router component, each Link and Match can communicate back to the containing Router.


Referencing Context in Lifecycle Methods
-------------

If `contextTypes` is defined within a component, the following lifecycle methods will receive an additional parameter, the `context` object:

```
constructor(props, context)
componentWillReceiveProps(nextProps, nextContext)
shouldComponentUpdate(nextProps, nextState, nextContext)
componentWillUpdate(nextProps, nextState, nextContext)
componentDidUpdate(prevProps, prevState, prevContext)
```

Referencing Context in Stateless Functional Components
-------------

Stateless functional components are also able to reference context if contextTypes is defined as a property of the function.

Updating Context
-------------

**Don't do it.**

The `getChildContext` function will be called when the state or props changes.

**Notice**, if a context value provided by component changes, descendants that use that value won't update if an intermediate parent returns false from shouldComponentUpdate. See [this blog](https://medium.com/@mweststrate/how-to-safely-use-react-context-b7e343eff076) for details and work around.



