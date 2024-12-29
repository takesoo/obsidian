---
tags:
  - Linux
---
- プロセスの状態
- D
	- IO待ち
- I
	- idle状態
- R
	- 実行中
- S
	- 完了待ち
- T
	- 
- t
- W
- X
- Z
```bash
$ man ps
PROCESS STATE CODES
       Here are the different values that the s, stat and state output specifiers (header "STAT" or
       "S") will display to describe the state of a process:

               D    uninterruptible sleep (usually IO)
               I    Idle kernel thread
               R    running or runnable (on run queue)
               S    interruptible sleep (waiting for an event to complete)
               T    stopped by job control signal
               t    stopped by debugger during the tracing
               W    paging (not valid since the 2.6.xx kernel)
               X    dead (should never be seen)
               Z    defunct ("zombie") process, terminated but not reaped by its parent
```
