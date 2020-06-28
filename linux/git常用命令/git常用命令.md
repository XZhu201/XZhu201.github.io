# 这里只统计我自己运用中常用到的git命令

使用git管理自己的项目时，根据具体情况，注意该设置为私有的项目要**记得设置为私有**。


### 0. 推荐采用图形化的界面，比如[github desktop](https://desktop.github.com/)

github desktop在windows或者linux下都有，易于安装使用。学习成本较低，而且不容易犯错。
[github desktop也可以用来管理gitlab的项目](https://blog.csdn.net/towrabbit/article/details/95224578)。除了github desktop还有很多别的[GUI客户端](https://git-scm.com/download/gui/windows)。宇宙神器vscode可以实现git的操作。

有时候会在服务器上用到git[^matlab]，这个时候免不了用到命令行操作，所以以下统计一下我经常会用到的最简单的命令。注意：以下都是我自己瞎捉摸的，实际上这些命令的完整用法可以很容易上网查到。

### 1. 最最最基本的命令：
##### 拉取：
- `git clone`
- `git pull`

##### 推送三连：
- `git add .`
- `git commit -m "内容"`
- `git pull`

### 2. 查看状态
- `git show`: 详细显示之前的commit，包括改动。
- `git log`: 会列出之前一系列的commit。

[^matlab]: 其实在matlab命令行中，以`!`开头，也可以使用git命令，比如`!git push`。当然，如果是在windows中用matlab，需要先装一个windows中的git。

### 3. git免密码配置方法

`git config --global credential.helper store`

输入一次账号密码后第二次就会记住账号密码。可以在~/.git-credentials查看。