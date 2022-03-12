# react-element-cloner

Read `notes.md` for help!

## Task 1

Create these components:

```jsx
<Breakfast>
  <Food foodName="toast" />
  <Food foodName="mushrooms" />
</Breakfast>
```

`Food` displays the `foodName` string passed to it

`Breakfast` - **overwrites** the `foodName` prop to be `"coffee"`

## Task 2

Allow `BreakFast` to take an `overwrite` prop

```jsx
<Breakfast overwrite="vegan sausages">
  <Food foodName="hash browns"
</Breakfast>
```

If `overwrite` is passed, all `foodName` props are overwritten to be this prop. If it is not passed then do not overwrite the `foodName` props.

## Task 3

Create a `ToggleInputs` component that displays a button which, when clicked, toggles whether or not it's inputs are `disabled`.

```jsx
<ToggleInputs>
  <input type="text" placeholder="tell me a fun fact!" />
  <select name="type" id="joke-type">
    <option>funny</option>
    <option>hilarious</option>
  </select>
</ToggleInputs>
```

## Task 4 [advanced]

Create a `Spy` component which wraps an `input` element. 
When the `onChange` event of the `input` is triggered the `Spy` component will `console.log()` the value before inputs `onChange` event is triggered

```jsx
<Spy>
  <input type="text" onChange={(e) => setInput(e.target.value)} />
</Spy>
```
