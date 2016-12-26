Summary for this [Summary for Composition VS Inheritance](https://facebook.github.io/react/docs/composition-vs-inheritance.html)

Recommend composition instead of inheritace.

Conainment
-----------------

Pass children elements via `props.children`(or any other prop).

Specialization 
---------------------

A more "specific" component renders a more "genderic" one, and configures it with props.
My explaination, build a wrapper to enhance the original component, just like you do to functions.

So What About Inheritance?
---------------------

1. No cases for component inheritance hierarchies.
2. Remember, components may accept arbitrary props, including primitive values, React elements, or functions.
3. Extract function, object, or class into a separate JavaScript module to share it, without extending it.
