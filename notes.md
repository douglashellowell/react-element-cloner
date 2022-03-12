# Cloning Children in React

Composition is a useful pattern when we want to make re-usable containers for other components

When nesting components inside a JSX tag we can access and render the elements on `props.children`

```jsx
const App = () => {
  return (
    <PrettyBorder>
      <p>sunflowers</p>
      <p>lilies</p>
      <p>roses</p>
    </PrettyBorder>
  );
};

const PrettyBorder = ({ children }) => {
  // destructuring children off of props
  return <div className="pretty-border">{children}</div>;
};
```

if we were to `console.log(children)` we would either see a single component object or an array of component objects - depending on how many we have passed as children

> something like this...

```
Object { "$$typeof": Symbol("react.element"), type: "p", key: null, ref: null, props: {…}, _owner: {…}, _store: {…}, … }
```

React provides a safe way to interact with and manage children - including passing/overwriting props and children - which gives us an opportunity to write some more advanced React Components!

---

In this example we're going to create a `FlowerList` component that passes a new `flower` prop to each of its children 🌻

Before we go any further we need to ensure that `children` is an array - even if one child is passed.

To do this we can use the `React.Children.toArray()` method

```jsx
import React from 'react';

const FlowersList = ({ children }) => {
  const childrenArray = React.Children.toArray(children);

  Array.isArray(childrenArray); // true - it's an array alright!
};
```

[React docs - React.Children.toArray](https://reactjs.org/docs/react-api.html#reactchildrentoarray)

Now that we have the elements in an array we can `map` through them and pass new props one-by-one

While mapping we can use the `cloneElement` function and pass in new props on the second argument

`cloneElement` takes these arguments:

```js
React.cloneElement(
  element, // element to clone
  config, // optional - inject props, refs, keys and more here
  ...children // optional - you can even change the children!
);
```

```jsx
import React, { cloneElement } from 'react';

const FlowersList = ({ children }) => {
  const childrenArray = React.Children.toArray(children);

  const clonedChildren = childrenArray.map((child) => {
    const clone = cloneElement(child, {
      flower: 'tulips',
    });

    return clone;
  });

  return <div>{clonedChildren}</div>;
};
```

> (I'm over-declaring variables for demonstration purposes - feel free to chain these methods/return straight away)

[React Docs - cloneElement](https://reactjs.org/docs/react-api.html#cloneelement)

In the above code we have passed a `flower` prop to each child with `'tulips'` hard-coded in.

Now any child we pass to `<FlowerList>` will have that prop!

```jsx
const Flower = ({ flower }) => <p>{flower}</p>;

const App = () => {
  return (
    <FlowerList>
      <Flower flower="sunflowers" />
      <Flower flower="lilies" />
      <Flower flower="roses" />
    </FlowerList>
  );
  // even though we have passed different flowers ALL will be overwritten by `FlowerList`!
};
```

The page now shows: "tulips tulips tulips" 🌷🌷🌷
