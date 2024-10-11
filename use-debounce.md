---
tags:
  - React
  - npm
---
https://github.com/xnimorz/use-debounce
[[debounce]]機能のライブラリ
```js
import { useDebouncedCallback } from 'use-debounce';

function Input({ defaultValue }) {
  const [value, setValue] = useState(defaultValue);
  // Debounce callback
  const debounced = useDebouncedCallback(
    // function
    (value) => {
      setValue(value);
    },
    // delay in ms
    1000
  );

  // you should use `e => debounced(e.target.value)` as react works with synthetic events
  return (
    <div>
      <input
        defaultValue={defaultValue}
        onChange={(e) => debounced(e.target.value)}
      />
      <p>Debounced value: {value}</p>
    </div>
  );
}
```