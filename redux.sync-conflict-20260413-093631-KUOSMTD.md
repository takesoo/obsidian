---
tags:
  - npm
  - React
---
- state管理ライブラリ
- 登場人物
	- store: stateとreducerを保持する場所
	- action: 何が起きたのかの情報を持つオブジェクト
	- dispatch: 呼び出されたactionをstoreに通知する関数
	- reducer: storeから受け取ったactionと、store内のstateをもとに、変更されたstateを返す純粋関数
https://redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif
