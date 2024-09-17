---
tags:
  - React
---
[[React]]でコンポーネントの状態やライフサイクルを扱うための仕組み。`useXxx`という形で提供される。
また、サードパーティのライブラリが機能をhooksの形で提供している。

```dataview
table
from #react/hooks 
```

## 例
```tsx
import { useState, useEffect } from 'react';

function MyComponent() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(res => res.json())
      .then(data => {
        setData(data);
        setLoading(false);
      })
      .catch(error => {
        setError(error);
        setLoading(false);
      });
  }, []);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error loading data</div>;

  return <div>{data.title}</div>;
}

```
## [[useMemo]]と[[useCallback]]の違い
いずれもキャッシュを駆使してパフォーマンス改善のためのhooks。
useMemoは関数の実行結果をキャッシュするのに対して、useCallbackは関数自体をキャッシュするという違いがある。