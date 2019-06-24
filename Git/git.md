> failed to push some refs to git

```
git pull --rebase origin master
git push -u origin master
```

> error: cannot pull with rebase: Your index contains uncommitted changes.
error: cannot pull with rebase: You have unstaged changes.
error: please commit or stash them.

- git stash: #可用来暂存当前正在进行的工作
- git stash pop: #从Git栈中读取最近一次保存的内容

```
先执行git stash 
再执行git pull –rebase 
最后再执行git stash pop
```