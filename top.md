---
tags:
  - Linux/コマンド
overview: 実行中のプロセスの状態をリアルタイムで表示する
---

| キー    | 動作                       |
| ----- | ------------------------ |
| ?     | ヘルプ                      |
| space | 表示を更新する                  |
| 1     | CPU別にCPUの使用率を表示する        |
| P     | CPU使用率でプロセスをソート          |
| M     | メモリ使用率でプロセスをソート          |
| <>    | ソートのための項目を左右に選択する        |
| x     | ソートのために選択している項目をハイライト    |
| b     | ソートのために選択している項目をわかりやすくする |
| q     | 終了                       |
```bash
top

# 現在時刻　 OSが起動してからの経過時間 ログイン中のユーザー数 load-average
$ top - 04:47:57 up 12:50,  1 user,  load average: 1.51, 1.61, 1.59
Tasks:   5 total,   1 running,   4 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni, 99.9 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :  11949.3 total,  10493.6 free,    903.1 used,    735.6 buff/cache
MiB Swap:   3072.0 total,   3072.0 free,      0.0 used.  11046.2 avail Mem 
  scroll coordinates: y = 1/5 (tasks), x = 1/12 (fields)
  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                                 
    1 root      20   0   16312   7636   6484 S   0.0   0.1   0:00.01 sshd                                                    
    7 root      20   0   21728  10072   8408 S   0.0   0.1   0:00.50 sshd                                                    
   11 linuc     20   0   21728   6336   4596 S   0.0   0.1   0:27.76 sshd                                                    
   12 linuc     20   0    6024   4828   2908 S   0.0   0.0   0:08.10 bash                                                    
25938 linuc     20   0    7600   3060   2548 R   0.0   0.0   0:00.03 top 
```
- load average
	- ディスクI街のプロセス数
	- 左から順に、1分平均、5分平均、15分平均
	- 数値が高いとパフォーマンスに影響を及ぼしている可能性が高い