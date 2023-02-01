# Notes

When State changes of a component that component re-runs.

To increase performance React only updates Real DOM when there is change in previous Virtual DOM & updated Virtual DOM.

Even though Real DOM not changes code inside child components still executed. (like console.log() )

to prevent child components reload we can use React.memo()

```
import React from 'react';

import MyParagraph from './MyParagraph';

const DemoOutput = (props) => {
  console.log('DemoOutput RUNNING');
  return <MyParagraph>{props.show ? 'This is new!' : ''}</MyParagraph>;
};

export default React.memo(DemoOutput);
```

React.memo() prevents re-execution of a component by matching its previous props with new props. and if the props are same it returns the previous stored returned value.

but React.memo() comes with a cost of using memory to stored returned values & matching previous & new props values. Which sometimes not worth to use in small application with small DOM tree.

Remember! - by default React.memo() only work with Primitive value datatype. [Read More](https://www.w3schools.com/react/react_usecallback.asp)

[Reference vs Primitive Values Article](https://academind.com/tutorials/reference-vs-primitive-values)

To make it work with non Primitive values(like function, array, object, etc) we have to use **useCallback()** Hook.

## useCallback() -

useCallback() is a React Hook That lets you cache a function definition between re-renders.

**Syntax** -

```
const memoizedCallback = useCallback(() => {
        doSomething(a, b);
    }, [a, b] // here [a, b] is a dependency array where a & b are dependencies.
);
```

**Example 1** - without dependency

```
function App() {
  const [showParagraph, setShowParagraph] = useState(false);

  console.log('APP RUNNING');

  const toggleParagraphHandler = useCallback(() => {
    setShowParagraph((prevShowParagraph) => !prevShowParagraph);
  }, []);

  return (
    <div className="app">
      <h1>Hi there!</h1>
      <DemoOutput show={false} />
      <Button onClick={toggleParagraphHandler}>Toggle Paragraph!</Button>
    </div>
  );
}
```

**Example 2** - with dependency

```
function App() {
  const [showParagraph, setShowParagraph] = useState(false);
  const [allowToggle, setAllowToggle] = useState(false);

  console.log('APP RUNNING');

  const toggleParagraphHandler = useCallback(() => {
    if (allowToggle) {
      setShowParagraph((prevShowParagraph) => !prevShowParagraph);
    }
  }, [allowToggle]);

  const allowToggleHandler = () => {
    setAllowToggle(true);
  };

  return (
    <div className="app">
      <h1>Hi there!</h1>
      <DemoOutput show={showParagraph} />
      <Button onClick={allowToggleHandler}>Allow Toggling</Button>
      <Button onClick={toggleParagraphHandler}>Toggle Paragraph!</Button>
    </div>
  );
}
```

**Usage** -

1. [Skipping re-rendering of components](https://beta.reactjs.org/reference/react/useCallback#skipping-re-rendering-of-components)

2. [Updating state from a memoized callback](https://beta.reactjs.org/reference/react/useCallback#updating-state-from-a-memoized-callback)

3.[Preventing an Effect from firing too often](https://beta.reactjs.org/reference/react/useCallback#preventing-an-effect-from-firing-too-often)

4. [Optimizing a custom Hook](https://beta.reactjs.org/reference/react/useCallback#optimizing-a-custom-hook)

## useMemo -

useMemo is a React Hook that lets you cache the result of a calculation between re-renders.

The React useMemo Hook returns a memoized value.

_Think of memoization as caching a value so that it does not need to be recalculated._

The useMemo Hook only runs when one of its dependencies update.

This can improve performance.

_The useMemo and useCallback Hooks are similar. The main difference is that useMemo returns a memoized value and useCallback returns a memoized function. You can learn more about useCallback in the useCallback chapter._

**Syntax** -

```
const cachedValue = useMemo(calculateValue, dependencies)
```

Call useMemo at the top level of your component to cache a calculation between re-renders:

```
import { useMemo } from 'react';

function TodoList({ todos, tab }) {
  const visibleTodos = useMemo(
    () => filterTodos(todos, tab),
    [todos, tab]
  );
  // ...
}
```

**\*Usage** -

1. Skipping expensive recalculations
   To cache a calculation between re-renders, wrap it in a useMemo call at the top level of your component:

```
import { useMemo } from 'react';

function TodoList({ todos, tab, theme }) {
  const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
  // ...
}
```
