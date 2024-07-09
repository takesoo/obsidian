CSSのグローバルスコープ汚染問題を回避するための仕組み
[[Webpack]]や[[vite]]などの[[モジュールバンドラ]]がCSSファイルをバンドルするモードがあり、ファイル名を`*.module.css`にしておくと自動でバンドルしてくれる
バンドルするとクラスセレクタがユニークになるように変換してくれるので、クラスセレクタの干渉を防げる

```css
/* src/Counter.module.css */
.count {
  color: red;
}
@media (max-width: 12450px) {
  .count {
    font-size: 1.5rem;
  }
}
.button {
  border-radius: 50%;
}
.button:hover {
  color: red;
}
```
```tsx
// src/Counter.tsx
import styles from './Counter.module.css';

function Counter() {
  const [count, setCount] = useState(0);
  const increment = useCallback(() => setCount((count) => count + 1), []);
  return (
    <div>
      <span className={styles.count}>
        {count}
      </span>
      <button onClick={increment} className={styles.button}>
        increment
      </button>
    </div>
  );
}
```

