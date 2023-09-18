---
Tags:
  - CS50
Status: Not started
Last edited time: Invalid date
Created time: Invalid date
---
16進数

ポインタ：メモリ内のアドレス

```
int a = 50; // aに代入されるのは50を格納したメモリ領域のポインタint b = a;  // bにaのポインタが代入されるb = 100;    // bの指し示す値を100にするprintf("b: %i\n", b); // 100printf("a: %i\n", a); // 100// aの指し示している値はbの値でもあるので100になっている 
```

mallocとfree

バッファオーバーフロー

```
int *x = malloc(3 * sizeof(int));x[1] = 72;x[2] = 73;x[3] = 33; //xの3byte目はmallocしてないので触れるとエラーになる
```

  

プログラム実行時のメモリ領域