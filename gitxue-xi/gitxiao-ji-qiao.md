# Git小技巧（常用）

##### 1、切换Git拉取源

```
git remote set-url origin 【git@github.com:someaccount/someproject.git】
```

##### 2、强制更新，放弃修改

```
git fetch --all
git reset --hard origin/master
```

##### 3、gitignore无效问题

解决后来添加的gitignore文件无效问题，原因是在此之前文件都加入了git源，所以gitignore无效，此处需要先接触绑定，再重新提交。

```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```



