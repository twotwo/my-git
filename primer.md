# Git 入门篇
从账号配置，init、commit、push、pull到branch、tag的一个基本流程
## 账号配置
参考1 [初次运行 Git 的配置](https://git-scm.com/book/zh/v2/起步-初次运行-Git-前的配置)

参考2 [git 设置和取消代理](https://gist.github.com/laispace/666dd7b27e9116faece6)

### git config
	$ git help config

	## Set Your Identity
	$ git config --global user.name "twotwo"
	$ git config --global user.email twotwo@li3huo.com

	## Set/Unset Your Proxy: http&https
	$ git config --global http.proxy 'socks5://127.0.0.1:1080'
	$ git config --global --unset http.proxy


## 获取与创建项目
参考[Git 获取与创建项目](https://git-scm.com/book/zh/v2/Git-命令-获取与创建项目)

### git init
*创建项目* 运行 git init 就可以将一个目录转变成一个 Git 仓库，这样就可以开始对它进行版本管理了

### git clone
*获取项目*

	$ git clone https://github.com/twotwo/learn-git.git

## 提交与分支

*提交*
### git status

	$ git status
		modified:   primer.md

### git add & git commit

	$ git add primer.md
	$ git commit -m "update primer.md"
	[master 85c4f11] update primer.md
	 1 file changed, 24 insertions(+), 7 deletions(-)
	
*分支*

    ☞ 'git branch' is used to create and list branches
    ☞ 'git checkout' is used to switch branches
    ☞ 'git checkout -b' is used to create and then switch branches

## Pulling and pushing

## 参考
* [一个初始化、提交和同步的例子](http://wiki.eclipse.org/EGit/Git_For_Eclipse_Users#Worked_example) 基于命令行
