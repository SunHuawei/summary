Summary for [JSX in Depth](https://facebook.github.io/react/docs/jsx-in-depth.html)

JSX will be converted into JavaScript

```javascript
React.createElement(component, props, ...children)
```

Try out [the online Babel compiler](https://babeljs.io/repl/#?babili=false&evaluate=true&lineWrap=false&presets=es2015%2Creact%2Cstage-0&code=function%20hello()%20%7B%0A%20%20return%20%3Cdiv%3EHello%20world!%3C%2Fdiv%3E%3B%0A%7D).

Specifying The React Element Type
---------------------

#### React Must Be in Scope

Don't forget to import `React` and your `CustomComponent`.

```javascript
import React from 'react';
import CustomComponent from './CustomComponent';
```

#### Using Dot Notation for JSX Type

You can also refer to a React component using dot-notation from within JSX.

```javascript
<MyComponents.DatePicker color="blue" />
```

#### User-Defined Components Must Be Capitalized

JSX code
```javascript
<CustomComponent />
<customComponent />
```

Converted into JavaScript
```javascript
React.createElement(
  CustomComponent,
  null
)
React.createElement(
  "customComponent", // Notice this
  null
)
```

1. `CustomComponent` is good
2. `customComponent` is not converted as expected

You always can rename your component to be capitalized.
```javascript
let Renamed = customComponent;
...
<Renamed />
```

#### Choosing the Type at Runtime

```javascript
<components[props.storyType] />
```

Wrong! JSX type can't be an expression.

Fix it like this:

```javascript
let MyComponent = components[props.storyType];
<MyComponent />
```

Props in JSX
------------------

#### JavaScript Expressions 

Forget HTML, JSX is just JS. So,

1. You can pass any JavaScript expression as a prop
2. You can extract `if` statements and `for` loops into a variable or use `a ? b : c` and `[].map` or `[].reduce` instead


#### String Literals 

You can pass a string literal as a prop. These two JSX expressions are equivalent:
```javascript
<MyComponent message="hello world" />
<MyComponent message={'hello world'} />
```

It is not necessary to escape for HTML, these three JSX expressions are equivalent:

```javascript
<MyComponent message="&lt;3" />
<MyComponent message='<3' />
<MyComponent message={'<3'} />
```

#### Props Default to "True"

These two JSX expressions are equivalent:
```javascript
<MyTextBox autocomplete />
<MyTextBox autocomplete={true} />
```

Don't mess with  [ES6 object shorthand](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#New_notations_in_ECMAScript_2015). For example, `{foo}` is short for `{foo: foo}` rather than `{foo: true}`.

#### Spread Attributes

Use `...` to reduce code, but use it sparingly, it can also make your code messy.
```javascript
<Greeting {...props} />
```

Children in JSX
-----------------

#### String Literals 
1. Pass string as child(props.children)
2. You are not required to care about escape, `&amp;` and `&` are the same.
3. Trim string
    1. JSX removes whitespace at the beginning and ending of a line.
    2. It also removes blank lines.
    3. New lines adjacent to tags are removed; new lines that occur in the middle of string literals are condensed into a single space.
    4. Multiple whitespace between words will be keep

#### JSX Children 

1. JSX can have one or more children
2. Children could be different types
3. `render()` can only return one element, you could wrap multiple components in `<div>`

#### JavaScript Expressions

User `{}`, write any JavaScript expression.

#### Functions as Children 

You can pass anything to children, even function, Regex. `props.children` works just like any other prop.

#### Booleans, Null, and Undefined Are Ignored 

These JSX expressions will all render to the same thing:
```javascript
<div></div>
<div>{false}</div>
<div>{true}</div>
```
They will be ignored, even `true`.

But some ["falsy" values](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) would be render. Like `0`
```javascript
<div>{0}</div>
```

One of the useful cases is render with condition
```javascript
{showHeader && <Header />}
```
But, you should be aware of the `array.length` case, it may render `0`
```javascript
{array.length && <List />}
```
To fix, you may rewrite it like this:
```javascript
{array.length > 0 && <List />}
```
Conversely, if you want a value like `false`, `true`, `null`, or `undefined` to appear in the output, you have to [convert it to a string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#String_conversion) first:
```javascript
<div> My JavaScript variable is {String(myVariable)}.</div>
```
