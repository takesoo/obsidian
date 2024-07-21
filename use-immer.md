---
tags:
  - npm
---
[[immer]]を使用するためのreact hook
戻り値にタプルを返す。タプルの最初の値は現在のstate、2番目の値は更新メソッド。

```js
import React from "react";
import { useImmer } from "use-immer";


function App() {
  const [person, updatePerson] = useImmer({
    name: "Michel",
    age: 33
  });

  function updateName(name) {
    updatePerson(draft => {
      draft.name = name;
    });
  }

  function becomeOlder() {
    updatePerson(draft => {
      draft.age++;
    });
  }

  return (
    <div className="App">
      <h1>
        Hello {person.name} ({person.age})
      </h1>
      <input
        onChange={e => {
          updateName(e.target.value);
        }}
        value={person.name}
      />
      <br />
      <button onClick={becomeOlder}>Older</button>
    </div>
  );
}
```

[immerjs/use-immer: Use immer to drive state with a React hooks](https://github.com/immerjs/use-immer)