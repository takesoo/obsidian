---
tags:
  - use-immer
---
[[use-immer]]フックのひとつ
```js
import React from "react";
import { useImmerReducer } from "use-immer";

const initialState = { count: 0 };

function reducer(draft, action) {
  switch (action.type) {
    case "reset":
      return initialState;
    case "increment":
      return void draft.count++;
    case "decrement":
      return void draft.count--;
  }
}

function Counter() {
  const [state, dispatch] = useImmerReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: "reset" })}>Reset</button>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
    </>
  );
}
```