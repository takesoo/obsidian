---
aliases:
  - SLAP
---
- コードの抽象レベルを統一すること
- 抽象度を合わせることで目次のようになり、要約性と閲覧性をもたらす
```js
function highLevel() {
	middleLevel1();
	middleLevel2();
}

function middleLevel1() {
	lowLevel1()
	lowLevel2()
}

function lowLevel1() {
	something1()
}

function lowLevel2() {
	something2()
}

function middleLevel2() {
	lowLevel3()
}

function lowLevel3() {
	something3()
}
```
- 