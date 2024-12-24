---
tags:
  - Linux/コマンド
overview: パスワードの設定
---
- 一般ユーザーは自分のパスワードのみ設定できる。rootはすべてのユーザーのパスワードを設定できる。
```bash
passwd [user]

$ passwd sato
passwd: Only root can specify a user name.

$ sudo passwd sato
Changing password for user sato.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.
```
