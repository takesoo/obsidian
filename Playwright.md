---
tags:
  - npm
---
Chrome, Safari, Firefoxをコマンドライン上で実行できる[[Node.js]]ライブラリ
[[Puppeteer]]の後継であり、PuppeteerはChromeだけだったが、PlaywrightはSafari, Firefoxも操作できる

```javascript
const pw = require('playwright');

(async () => {
  const browser = await pw.webkit.launch(); // or 'chromium', 'firefox'
  const context = await browser.newContext();
  const page = await context.newPage();

  await page.goto('https://www.example.com/');
  await page.screenshot({ path: 'example.png' });

  await browser.close();
})();

```

