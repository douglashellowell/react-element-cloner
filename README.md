# react-element-cloner

Read `notes.md` for help!

fork this repo and create a react app in this directory 

`yarn create react-app .` or `npx create-react-app .`

Ask Doug for a hint if you want one!

## Task 1

Create these components:

```jsx
<Breakfast>
  <Food foodName="toast" />
  <Food foodName="mushrooms" />
</Breakfast>
```

`Food` displays the `foodName` string passed to it

`Breakfast` - passes a `foodName` prop to each of its children - changing each to be `"coffee"`

## Task 2

Allow `BreakFast` to take an `overwrite` prop

```jsx
<Breakfast overwrite="vegan sausages">
  <Food foodName="hash browns" />
</Breakfast>
```

If `overwrite` is passed, all `foodName` props are overwritten to be this prop. 

If it is not passed then do not overwrite the `foodName` props.

## Task 3

Create a `ToggleInputs` component that displays a `button` which, when clicked, toggles whether or not `ToggleInput`s children (`input`/`button`/`select`/etc...) are `disabled`.

```jsx
<ToggleInputs>
  <input type="text" placeholder="tell me a joke!" />
  <select name="type" id="joke-type">
    <option>funny</option>
    <option>hilarious</option>
  </select>
</ToggleInputs>
```

## Task 4 [advanced]

Create a `Spy` component which wraps an `input` element. 
When the `onChange` event of the `input` is triggered the `Spy` component will `console.log()` the value **before** inputs `onChange` function is invoked. The functionality of the `input` must be unchanged.

```jsx
<Spy>
  <input type="text" onChange={(e) => setInput(e.target.value)} />
</Spy>
```


## Task 5 [very advanced]

Create a `DeepRename` component that takes a `message` prop. This component overwrites any text with this message **no matter how deeply nested in the component tree**

```jsx
<DeepRename message="gotcha!">
  <div>
    <div>
     <div>
       <p>change me!</p>
     </div>
    </div>
  </div>
</DeepRename>
```
hint: `change me!` is a child of the `<p>` tag and it's data type is a `string`! For this challenge assume this string has no siblings

tip: practice replacing `change me!` in a non-nested `p` tag before trying to solve it with nesting

## Extras - add your own ideas below and put in a pull request!

## MovieList

Create a `MovieList` component which renders `Movie` components from an array. `Movie`s **only** functionality is to display the movie name. `MovieList` adds functionality to select a movie - the selected movie will have it's background highlighted

```jsx
<MovieList>
  {movies.map(movie => <Movie movie={movie}/>)}
</MovieList>
```
