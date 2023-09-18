---
tags:
  - コンピュータ
  - CPU
---
[[CPU]]の性能を表す要素

要素名|意味|単位
-|-|-
[[クロック周波数]]|1秒あたりのクロック数|Clock/sec
[[CPI]]|1命令あたりのクロック数|Clock/Instruction
[[MIPS]]|1秒あたりの命令数|Million Instruction/sec
[[命令ミックス]]|命令を出現頻度で重みづけした値|Clock

$$
MIPS = \frac{1}{CPI \times (1/クロック周波数)}
$$
