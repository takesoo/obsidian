`file:///Documents/path/to/file`
```ts
// ファイルURLからファイルパスに変換
// パス操作はパスオブジェクトに変換した方が楽にできる
import { fileURLToPath } from "url";

const filePath = fileURLToPath(fileUrl);
```
