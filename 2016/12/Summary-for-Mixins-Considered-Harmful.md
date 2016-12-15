Here is my summary for this [article](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).

Mixins are unnecessary and problematic in React codebases. Here’s why.

Mixins introduce implicit dependencies
----------------------

JavaScript is a dynamic language so it’s hard to enforce or document the dependencies.

1. Component and mixin maybe depends on each other
2. It is hard to rename the state key(or function names or any others) which mixin reads too
3. Component might reference some method that isn’t defined on the class, and it will be hard to find the definition
4. Mixin might depend on mixin, it is tricky to tell the data flows and dependency graph

Mixins cause name clashes
----------------------

1. For the developers, you have to avoid clashes
2. For mixin authors, hard to remove or change or refactory

Mixins cause snowballing complexity
----------------------

Mixins tend to become complex over time. Mixins may be tightly coupled.

Hard to split a “simpler” part of the mixin. Hard to change or remove the existing mixins, they keep getting more abstract until nobody understands how they work.


Migrating from Mixins
----------------------
mixins are not technically deprecated, but we won’t recommend using them in the future.


#### Performance Optimizations

##### Mixin solution:
PureRenderMixin

##### Recommend solution:
'react-addons-shallow-compare' or 'React.PureComponent'

#### Subscriptions and Side Effects

##### Mixin solution:
SubscriptionMixin

##### Trouble
If several components used this mixin to subscribe to a data source, a nice way to avoid repetition is to use a pattern called “higher-order components”.

##### Recommend solution:
HOC

Rendering Logic
-----------------

##### Mixin solution:
Component calls the function which is in mixin, and mixin calls the function which is in component.

##### Trouble
1. Implicit dependencies
2. Component and mixin may depend on each other

##### Recommend solution:
Extract a Component


## Context

Mixin solution:
Hide the context in Mixin

Trouble
Implicit dependencies

Recommend solution:
HOC


Related
-----------------

#### Higher-Order Components Explained
Explain Higher-Order Function and Higher-Order Components

How to make a HOC?
1. Extract the WrappedComponent
2. Build a function which takes the WrappedComponent as an argument and returns a new Component

So, actually Higher-Order Component is not a Component. It is a function, and a pattern.


#### Pitfalls of Higher-Order Comp onents
Refs, not able to foward the refs to the WrappedComponent. But
1. We discourage using refs for component communication
2. In the future, we might consider adding [ref forwarding](https://github.com/facebook/react/issues/4213) to React to solve this annoyance.