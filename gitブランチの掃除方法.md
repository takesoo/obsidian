## merged
```
git branch --merged|egrep -v '\*|develop|main|staging|order|stock|factory'|xargs git branch -d

```

## リモート追跡ブランチの削除
```
git fetch --prune
```