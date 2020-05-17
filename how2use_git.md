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
    //soft mixed hard 的区别
    soft后需要commit
    mixed后需要 add commit
    hard会更改工作区的文件，三个区域保持一致
```

## diff的用法

```java
git diff [filename] //将工作区和缓存区比较
git diff [本地库中的历史版本] [filename] //和本地库中的文件比较
```

## branch 

```java
//创建分支
git branch [branchname]

//查看分支
git branch -v
    
//进入分支
git checkout [branchname]
    
//合并分支
git merge [branchname] //先进入master分支，然后用merge将分支合并到主分支
    
//解决冲突
    //编辑文件删除特殊符号，修改到满意的程度，git commit 不能带文件名
```



