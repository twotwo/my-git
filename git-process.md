# Git 分支篇

Git 的分支模型是一个“Killing Feature”，它以一种难以置信的轻量方式处理分支，因此，鼓励在工作流程中频繁地使用分支与合并。
理解和精通这一特性，你便会意识到 Git 是如此的强大而又独特，并且从此真正改变你的开发方式。

## 需要掌握的概念
- ☞ 为啥建分支这么快？ **分支本质上就是一个41字节的文件**
- ☞ 为啥合并这么快？ **三方合并机制**
- ☞ **本地的分支合并操作与远端的分支管理**

## Command List

### git branch - 查看、创建及删除分支

	☞ 'git branch -a'list both remote-tracking branches and local branches.

### git checkout - 切换分支

	☞ 'git checkout' is used to switch branches
	☞ 'git checkout -b' is used to create and then switch branches

### git fetch - 抓取远程仓库内容，放入本地库

	☞ 'git fetch' fetches down all the information that is in that repository 
	that is not in your current one and stores it in your local database

### git pull - 抓取远程仓库内容，合并入当前分支

	☞ 'git pull' update for the repository cloned from, 
		   then merge one of them into current branch

### git rebase - [分支整合](https://git-scm.com/book/zh/v2/Git-分支-变基)

	☞ 'git rebase' reapply commits on top of another base tip

## 举个例子

### 第一步：新建分支
首先，每次开发新功能，都应该新建一个单独的分支（参考 [Git 分支管理](git-branch.md)）



	# 获取主干最新代码
	➜  my-git-fork git:(master) git checkout master     
	Already on 'master'
	Your branch is up-to-date with 'origin/master'.
	➜  my-git-fork git:(master) git pull
	Already up-to-date.

	# 新建一个开发分支myfeature
	➜  my-git-fork git:(master) git checkout -b dev-li3huo
	Switched to a new branch 'dev-li3huo'
	➜  my-git-fork git:(dev-li3huo) 

### 第二步：提交分支commit
分支修改后，就可以提交commit了

	➜  my-git-fork git:(dev-li3huo) ✗ git add .
	➜  my-git-fork git:(dev-li3huo) ✗ git status
	On branch dev-li3huo
	Changes to be committed:
	  (use "git reset HEAD <file>..." to unstage)

		new file:   git-process.md

	# 提交(在编辑器中填写提交信息)
	➜  my-git-fork git:(dev-li3huo) ✗ git commit --verbose
	[dev-li3huo 470a772] Git 标准使用流程
	 1 file changed, 94 insertions(+)
	 create mode 100644 git-process.md

### :point_right: 提交信息的格式

提交commit时，必须给出完整扼要的提交信息，下面是一个范本([提交信息书写规则](git-commit.md))


	Present-tense summary under 50 characters

	* More information about commit (under 72 characters).
	* More information about commit (under 72 characters).

	http://project.management-system.com/ticket/123

第一行是不超过50个字的提要，然后空一行，罗列出改动原因、主要变动、以及需要注意的问题。最后，提供对应的网址（比如Bug ticket）

### 第三步：与主干同步
分支的开发过程中，要经常与主干保持同步

	➜  my-git-fork git:(dev-li3huo) git fetch origin 
	➜  my-git-fork git:(dev-li3huo) git rebase origin/master
	Current branch dev-li3huo is up to date.

### 第四步：合并commit
分支开发完成后，很可能有一堆commit，但是合并到主干的时候，往往希望只有一个（或最多两三个）commit，这样不仅清晰，也容易管理。

那么，怎样才能将多个commit合并呢？这就要用到 git rebase 命令。

	$ git rebase -i origin/master

git rebase命令的i参数表示互动（interactive），这时git会打开一个互动界面，进行下一步操作

### 第五步：推送到远程仓库

合并commit后，就可以推送当前分支到远程仓库了

    $ git push --force origin myfeature

git push命令要加上force参数，因为rebase以后，分支历史改变了，跟远程分支不一定兼容，有可能要强行推送

### 第六步：发出Pull Request(Only for Github)

提交到远程仓库以后，就可以发出 Pull Request 到master分支，然后请求别人进行代码review，确认可以合并到master

## 参考
- 《Pro Git V2》 3. Git 分支
	
	* [3.1 分支简介](https://git-scm.com/book/zh/v2/Git-分支-分支简介) 分支的基本原理，建议掌握一下，否则估计会搞不懂下面的例子
	* [3.2 新建与合并](https://git-scm.com/book/zh/v2/Git-分支-分支的新建与合并) 一个应用的例子
	* [3.4 常见工作流介绍](https://git-scm.com/book/zh/v2/Git-分支-分支开发工作流) 

		- `长期分支` 适用于对稳定性要求很高、复杂庞大的项目
		- `特性分支` 短期分支，对任何规模的项目都适用 - Git 特有的工作流

	* [3.5 远程分支](https://git-scm.com/book/zh/v2/Git-分支-远程分支) 远程分支的管理

		- `跟踪分支` git checkout --track origin/remote_branch
		- `拉取` pull == fetch + merge到跟踪分支
		- `删除远程分支` git push origin --delete remote_branch

	* [3.6 rebase](https://git-scm.com/book/zh/v2/Git-分支-变基) 一种更高级的合并方法，一下看不懂的可以放放；看不懂的时候尽量别用，滥用产生的副作用很大！

		- 确保在向远程分支推送时能保持提交历史的整洁
		- `变基的风险` 不要对在你的仓库外有副本的分支执行变基
		- `变基 vs. 合并` 总的原则是，只对尚未推送或分享给别人的本地修改执行变基操作清理历史，从不对已推送至别处的提交执行变基操作，这样，你才能享受到两种方式带来的便利

- [Github 跨 Repository 开发流程](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html) @阮一峰 2015年8月5日 没有远端版本库权限的一个开发流程