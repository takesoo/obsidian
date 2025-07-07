---
tags:
  - npm
---
## what
- ReactおよびReact Native用の[[i18next]]
```jsx
...
<div>{t('simpleContent')}</div>
<Trans i18nKey="userMessagesUnread" count={count}>
  Hello <strong title={t('nameTitle')}>{{name}}</strong>, you have {{count}} unread message(s). <Link to="/msgs">Go to messages</Link>.
</Trans>
...
```