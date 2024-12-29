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
# 各状態のプロセス数
Tasks:   5 total,   1 running,   4 sleeping,   0 stopped,   0 zombie
# 前回表示から今回までの間(デフォルトでは3秒)の、各種プロセスの実行時間のパーセンテージ
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni, 99.9 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
# メモリ使用状況
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
	- ディスクIO待ちのプロセス数（[[Status Code]]がRもしくはDのプロセス数）
	- 左から順に、1分平均、5分平均、15分平均
	- 数値が高い＝プロセスからディスクへの書き込みが積滞している状態であり、パフォーマンスに影響を及ぼしている可能性が高い
- Tasks
	- total
		- トータルのプロセス数
	- running
		- 実行中のプロセス数
	- sleeping
		- スリープ中のプロセス数
	- stopped
		- 強制終了されたプロセス数
	- zombie
		- ゾンビプロセス数
		- 子プロセス自身は終了するつもりなのに、親プロセスからそれを承認するwait()システムコールが戻ってこないもの
- Cpus
	- CPU使用率
	- us
		- ==us==er
		- [[un-niced]]なユーザー空間プロセス（OSj上で動作するように作られたプログラム）が実行された時間のパーセンテージ
	- sy
		- ==sy==stem
		- カーネル空間プロセス（OSとしての機能を司るプログラム）が実行された時間のパーセンテージ
	- ni
		- ==ni==ce
		- [[nice]]や[[renice]]などにより[[nice値]]が変更された、ユーザー空間プロセスが実行された時間のパーセンテージ
	- id
		- ==id==le
		- [[idle]]状態を過ごしたプロセスの時間のパーセンテージ
	- wa
		- IO-==wa==it
		- Disk IOが終わるのを待っているプロセスの実行時間のパーセンテージ
	- hi
		- ==h==ardwear ==i==nterrupts
		- [[ハードウェア割り込み]]の時間のパーセンテージ
	- si
		- ==s==oftwear ==i==nterrupts
		- [[ソフトウェア割り込み]]の時間のパーセンテージ
	- st
		- ==st==olen
		- ハイパーバイザに徴収された実行時間のパーセンテージ
- Mib
	- Mem
		- メモリ使用状況
		- total
			- 物理メモリの総量
			- total = used + buff/cache + free
		- free
			- 空きメモリ量
		- used
			- OSやアプリケーションに割り当て済みのメモリ量
		- buff/cache
			- バッファやキャッシュに割り当て中のメモリ量
	- Swap
		- [[スワップ領域]]
		- total
			- スワップ領域の総量
		- free
			- 空き容量
		- used
			- OSやアプリケーションに割り当て済みのスワップ容量
		- avail Mem
			- **==SwapではなくMemの情報==**
			- 物理メモリの実質的な空き容量
			- avail Mem = free + buff/cacheのうち解放可能なメモリ量 = 実質すぐに割り当てられるメモリ量
			- メモリ空き容量を確認するならこの値を見る
- PID
	- プロセスID
- USER
	- 実行ユーザー
- PR
	- Priority
	- プロセスの優先度
	- real-timeは最優先
- NI
	- nice値
	- 低いほど優先される
	- NI = PR - 20
- VIRT
	- 割当済みの仮想メモリ容量(KiB)。スワップも含む。
- RES
	- ==Res==sdent Memory ==S==ize(KiB)
	- スワップを含まない、実際に消費されているメモリ容量
- SHR
	- ==Sh==a==r==ed Memory Size(KiB)
	- RESのうち、共有メモリとして消費されているメモリ容量
- S
	- Process State Code
	- 
- %CPU
	- CPU使用率
- %MEM
	- メモリ使用率
- TIME+
	- プロセスが起動してからCPUにより処理された総時間
	- +は10ms単位で表示している意味
- COMMAND
	- 実行プロセス名

