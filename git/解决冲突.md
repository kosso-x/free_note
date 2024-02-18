# 解决冲突

## 1. 自己的分支与自己分支冲突

```shell
  * 切换到分支 （test）
  git checkout test
  * 拉取远程分支并推送
  git pull --rebase
  * 推送
  git push
```

## 2. 自己的分支与别人的分支冲突

```shell
  * 切换到分支 （test）
  git checkout test
  * 变基
  git rebase develop
  * 强制推送
  git push --force origin test
```

## 3. 关于 git push

* 之前的机器 git push -f 是强制推送所有分支到远程分支
```shell
  执行
  git config push.default upstream  或  
  git config push.default simple
```
* 修改为只推送当前分支到远程