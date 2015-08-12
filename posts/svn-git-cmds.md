---
title: 我的svn git常用命令及配置
date: '2013-06-27'
description: svn,git,常用命令
categories: 'tool'
tags:
  - svn
  - git

---
参考:  
[SVN常用命令](http://blog.csdn.net/sunboy_2050/article/details/6187464)  
[svn diff](http://www.subversion.org.cn/svnbook/1.4/svn.ref.svn.c.diff.html)  

## svn 
### 常用命令
* `svn co remote-svn-path local-dir --username tiemei`签出
* `svn cp base-svn-path new-svn-path -m "commit log"`新建分支
* `svn diff -r m path/file` 比较工作拷贝和某个版本的区别
* `svn diff -c m path/file` 查看某个版本修改的内容
* `svn diff --diff-cmd /usr/bin/diff -x "-i -b" COMMITTERS` 指定外部diff程序 
* `svn diff -r m:n path` 比较两个版本文件差异
* `svn diff -r m:n path |grep "Index:"` 比较两个版本间变化的文件
* `svn info path` 列出某个path svn信息
* `svn log path` 查看path提交log
* `svn log -l4`  查看前四个提交log
* `svn ls path`  列出某个路径下内容(与远程交互)
* `svn mkdir path` 远程增加某个path
* `svn cat /path/to/file` 打印某个文件内容
* `svn st path` 只输出状态有变化的文件
* `svn st -v path` 列出所有文件详细状态信息
* `svn add file/dir``svn ci m "comment log"` 添加文件，提交变动
* `svn rm path/file` 移除某个分支/文件
* `svn merge URL local_dir/url`合并分支，先将原分支cm，然后merge到本地新开发分支，
或者merge到远程svn新分支
* `svn update -r version`更新到指定版本
* `svn checkout -r r974 URL`checkout指定版本代码

### 回滚

**改动没有被commit**:  

* `svn revert [-R] path/file` 放弃没有commit的修改，没法恢复。

**改动已经被commit**:  

```bash
svn merge -r 28:25 path/file # 从最新版本28回滚到25版本
svn diff path/file # 确认差异
svn commit -m "comment" # 提交回滚，版本升级到29
```


### 配置
#### 排除
**全局排除**  
~/.subversion/conf文件中global-ignores  
**局部排除**  
* `svn ps svn:ignore ignore_pattern .` 设置当前目录排除ignore_file脱离版本控制
* `svn ps svn:ignore -F .svnignore .` 指定文件.svnignore
* `svn pe svn:ignore . --editor-cmd=vim` 用vim来编辑需排除的pattern列表，与.svnignore方式互斥

## git

[Git 分支 - 何谓分支](http://git-scm.com/book/zh/Git-%E5%88%86%E6%94%AF-%E4%BD%95%E8%B0%93%E5%88%86%E6%94%AF)  

* `git remote add origin remote-url` 将本地仓库与远程仓库remote-url绑定，可以绑定多个。一般用origin，因为git clone默认设置origin
* `git push -u origin master` 提交到别名为origin的远程仓库的master分支
* `git remote -v` 列出本地绑定的所有远程仓库
  

![git提交状态](http://farm4.staticflickr.com/3701/9147517685_85e9e01925.jpg)  

* `git add file/dir``git commit -m 'commit log'`上图很形象的说明了git命令和文件所处状态的变化关系。
* `git commit -a -m "commit log"` 将add和commit动作合并，不过只作用域变化的文件，对新增文件无效
* `git log`查看commit历史 `git log --pretty=oneline --abbrev-commit`查看commit历史，简明格式

  
![git回退](http://farm6.staticflickr.com/5331/9149824940_25f19bf055.jpg)  

* `git checkout file/dir` 用index中文件覆盖working tree中文件
* `git reset HEAD file`   用commit中文件覆盖index中文件,HEAD为commit log前几位字符(唯一就行)
* `git revert HEAD`       undo某次commit动作，你也可以undo更老些的commit
* `git show HEAD`         show diff from one commmit compared to its parent
* `git diff HEAD1..HEAD2` 两个commit之间的变化

  
![git tag](http://farm8.staticflickr.com/7443/9149898002_3c3513af25.jpg)  

* `git tag wokring 3720b35``git tag broken` 给某次commit加tag
* `git diff working..broken` tag可以被用在HEAD出现的任何地方

### branche
![git branche](http://farm8.staticflickr.com/7384/9150086081_dfe8ef4bce.jpg)  
<<<<<<< HEAD
![Git 分支 - 远程分支](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF)  
=======
这里有一个多个分支创建于切换的典型例子，说的比较详细：https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6    
>>>>>>> 98d4643bd95cbf846411e2339f24a4c4ec6ec42c

* `git checkout -b branchname` 新建分支，等价于`git branch branchname; git checkout branchname`
* `git push origin branchname` 将本地分支推送到远程仓库
* `git push origin -d branchname` 删除远程分支
* `git merge branchname` 将另一个分支branchname与当前分支合并，如果有冲突需要手动解决
* `git branch -d branchname` 删除某个分支
* `git branche` 查看有几个分支，以及活动分支
* `git branch -r` 列出服务器仓库所有分支
* `git branche <name> <commit>` commit指定新branche的起始点，默认最新一次commit
* `git checkout <branchname>` 指定当前活动分支，与从Index同步文件命令意义不同
* `git fetch origin` 将远程仓库origin最新的内容下载下来，包括最新的分支，注意这里本地只是建了一个新分支的指针，还需要执行`git checkout -b [分支名] [远程名]/[分支名]` 来新建一个本地分支，并让本地分支跟踪远程分支。  
* `git push origin :[远程分支名]` 删除远程分支  
* `git checkout --track origin/[远程分支名]` 让当前分支跟踪某个远程分支
  
在新分支上commit后，然后又切回master，状态如下图：  
![git branche 2](http://farm8.staticflickr.com/7350/9150130937_6908be5258.jpg)  
接着，你又在master分支上commit，状态变成:  
![git branche 3](http://farm6.staticflickr.com/5547/9152373240_ee693532ee.jpg)

### Stashing

![Git 工具 - 储藏（Stashing）](https://git-scm.com/book/zh/v1/Git-%E5%B7%A5%E5%85%B7-%E5%82%A8%E8%97%8F%EF%BC%88Stashing%EF%BC%89)   

应用场景：  

* 有些变动没有commit，想拉远程内容，或者切换分支了，需要先stash
* stash的内容，切换到另一个分支后，能够合并回来（注意解决冲突）

* `git stash` 暂存当前快照，包括所有未commit内容
* ` git stash list`
* `git stash apply [stash@{n}]` 应用某个stash，不加stash@{0}默认最新的
* `git stash pop [stash@{n}]` 应用的同时移除
* `git stash drop [stash@{n}]` 删除
* `git stash show -p [stash@{0}] | git apply -R` 取消stash，也可以通过别名来实现：

        $ git config --global alias.stash-unapply '!git stash show -p | git apply -R'
        $ git stash apply
        $ #... work work work
        $ git stash-unapply

* `git stash branch testchanges` 从stash上创建一个新分支

这些stash的内容有单独的Git Staging area存储，就跟git status 看到的变更存在Git Index区一样。  
  
### merging
你在新branche开发完毕，要将变动合并到master，你将执行`git merge newfeature`，如果没有冲突，最终状态如下图：  
![git merging](http://farm6.staticflickr.com/5529/9152407936_c8898f484e.jpg)  
如果发生冲突，你将看到这样提示：  

    CONFLICT (content): Merge conflict in somefile.txt
    Automatic merge failed; fix conflicts and then commit the result.
你需要手动解决冲突，冲突的文件中冲突处类似下面格式：  

    <<<<<<< HEAD:somefile.txt
    this change was done in master
    =======
    this change was done in newfeature
    >>>>>>> newfeature:somefile.txt
改好后git add 有冲突的文件，然后commit，你就可以删掉branche了`git branche -d newfeature`  

### 配置

* `git config --global color.ui auto`
* `git config user.name "Bob"`
* `git config user.email "bob@example.com"`
* `git config --global user.name "Bob"`
* `git config --global user.email "bob@example.com"`
* 直接编辑.git/config

