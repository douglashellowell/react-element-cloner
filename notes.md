# Cloning Children in React

Composition is a useful pattern when we want to make re-usable containers for other components

When nesting components inside a JSX tag we can access and render the elements on `props.children`

```jsx
const App = () => {
  return (
    <PrettyBorder>
      <p>mario</p>
      <p>link</p>
      <p>ness</p>
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

We can do so much more that just display these child components!

React provides a safe way to interact with and manage children - including passing/overwriting props and children - which gives us an opportunity to write some more advanced React Components!

---

In this example we're going to create a `FlowerList` component that passes a new `flower` prop to each of its children 🌻

Before we go any further we need to ensure that `children` is an array - even if only one child is passed.

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

---

Because we have access to each `child` object we have access to their properties. This is useful if we want to use values already on the child before creating the clone

Let's allow the `Flower` children to have a `locked` prop - stopping `FlowerList` from passing a new `flower` prop

```js
const Flower = ({ flower }) => <p>{flower}</p>;

const App = () => {
  return (
    <FlowerList>
      <Flower flower="sunflowers" locked />
      <Flower flower="lilies" />
      <Flower flower="roses" locked />
    </FlowerList>
  );
  // we now need to refactor FlowerList to acknowledge `locked`
};
```

```jsx
import React, { cloneElement } from 'react';

const FlowersList = ({ children }) => {
  const childrenArray = React.Children.toArray(children);

  const clonedChildren = childrenArray.map((child) => {
    // we can access `child`s id, className, eventHandlers, children...
    if (child.props.locked) {
      return child;
    } else {
      const clone = cloneElement(child, {
        flower: 'tulips',
      });
      return clone;
    }
  });

  return <div>{clonedChildren}</div>;
};
```

Now only "lilies" will be changed to tulips

sunflowers tulips roses! 🌻 🌷 🌹

---

So far we have been using the `children` prop which could be any number of components and it would be tricky to distinguish between them or target specific ones.

Instead of nesting children inside our component we can instead provide components on named keys.

> Note we are **not** just passing the reference to a _function_ but rather a created component using angle brackets - `<LikeThis />`

```jsx
const App = () => {
  return <FlowerDisplay flower={<Flower />} container={<Vase />} />;
  // NB: vase is pronounced "vase"
};
```

With this setup we can target these components directly

> This time we know it's only ever going to _one_ component so we don't need use `React.Children.toArray`

```jsx
const FlowerDisplay = ({ flower, container }) => {
  // lets pass a 'flower' prop to the flower
  const flowerClone = createClone(flower, { flower: 'lilies' });

  // and a water and plantFood prop to the container (which is the <Vase /> from above)
  const containerClone = createClone(container, {
    water: true,
    plantFood: true,
  });

  return (
    <>
      {flowerClone}
      {containerClone}
    </>
  );
};
```
