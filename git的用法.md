# git 的使用

## 查看git状态

``` java 
git status
```

## git的添加，提交，删除

```java
//添加文件
vim newfile.md

//添加修改
git add newfile.md    or   git add .
    
//提交修改
git commit -m "修改的描述信息" newfile.md
    
//撤回提交：回回复到未添加修改的状态
git reset head newfile.md
git rm --cached newfile.md
```



## git的回滚

```java
//git 的回滚
//查看日志
git log		//美化： git log --pretty=oneline
git reflog

//移动至指定版本
git reset --hard [reflog中的hash值]
```



