---
title: ubuntu下git&github使用
categories:
  - Linux
date: 2015-04-11 16:49:35
tags:
  - linux
---

记录下在Ubuntu下使用git与github的经历。 github基本功能： Repository：放项目的仓库； Fork：授权拷贝；（Fork 别人的项目） Branch：Fork之后，在个人的github上出现同名Repository，是源Repository的分支（Branch）； Pull Request：推送请求，将修改后的代码推送给原作者；

1.  git初始设置

$ git config --global user.name "Your Real Name" 
$ git config --global user.email you@email.address

如，我的设置如下：

git config --global user.name "wmk26"
git config --global user.email "1549359482@qq.com"

2. 初始化Git仓库（init）

//Mytest目录是工作目录，仅作示意
$ cd ~/Mytest        
$ git init  

\# 然后会显示： 
Initialized empty Git repository in $PROJECT/ .git/

上述结果表示在当前目录下创建了一个.git的隐藏目录，这个就是所谓的Git仓库了。此时的～/Mytest文件夹，我们也改名称之为工作树。 3. 提交(commit) 接下来在本地仓库里添加一些文件，比如README。进行如下操作

//将README添加到待提交的列表。
$ git add README 
//提交到本地仓库，并没有提交到GitHub上。
$ git commit -m "first commit"

1.  连接github并上传repository

首先在本地创建ssh key；

$ ssh-keygen -t rsa -C "your_email@youremail.com"

后面的your\_email@youremail.com改为你的邮箱，之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。成功的话会在~/下生成.ssh文件夹，进去，打开id\_rsa.pub，复制里面的key。 回到github，进入Account Settings，左边选择SSH Keys，Add SSH Key,title随便填，粘贴key。为了验证是否成功，在git bash下输入：

$ ssh -T git@github.com

如果是第一次的会提示是否continue，输入yes就会看到：You've successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上github。 设置远程连接github：

git remote add origin https://github.com/user_name/Mytest.git
#注：使用https连接之后每次都需要输入用户名和密码。可以改用如下方式
git remote add origin git@github.com:user_name/Mytest.git

将本地仓库的文件上传到github上：

$ git push origin master

git push命令会将本地仓库推送到远程服务器。 git pull命令则相反。 修改完代码后，使用git status可以查看文件的差别，使用git add 添加要commit的文件，也可以用git add -i来智能添加文件。之后git commit提交本次修改，git push上传到github。

1.  其他

将github上的repository下载到本地，可以这样做：

$ git pull origin master

如果github上已经有同名的repository，再把本地的repository上传会发生如下错误：

! \[rejected\] master -> master (non-fast-forward) 
error: 无法推送一些引用到 'https://github.com/lmdyyh/BitMap.git' 
提示：更新被拒绝，因为您当前分支的最新提交落后于其对应的远程分支。 
提示：再次推送前，先与远程变更合并（如 'git pull'）。详见 
提示：'git push --help' 中的 'Note about fast-forwards' 小节。

因为GitHub上有个README文件，就好比多人协同工作时其他人上传了一个新版本，此时需要执行 git pull https://github.com/username/Project.git 或git pull origin master 我们运行 git pull,目的就是要从原始克隆的远端仓库中抓取数据后,合并到工作目录中当前分支。 然后merge后执行git push即可。fast forward能够保证不会强制覆盖别人的代码，确保了多人协同开发。尽量不要使用non fast forward方法提交代码。

1.  可能遇到的一些问题

6.1. 在linux上使用SSH连接github时可能会遇到如下错误：

Agent admitted failure to sign using the key

解决：运行如下两个命令即可：

\# start the ssh-agent in the background
eval "$(ssh-agent -s)"
\# Agent pid 59566
ssh-add
\# Enter passphrase for /home/you/.ssh/id_rsa: \[tippy tap\]
\# Identity added: /home/you/.ssh/id\_rsa (/home/you/.ssh/id\_rsa)

6.2. 设置了SSH连接，可是每次push的时候都需要输入用户名和密码 原因：使用了https方式push

#输入
git remote -v 
#返回
origin  https://github.com/:wmk26/leetcode.git (fetch)
origin  https://github.com/:wmk26/leetcode.git (push)

解决：输入如下三行命令

git remote rm origin
git remote add origin git@github.com:wmk26/leetcode.git
git push origin

  参考文献：

1.  [github简单使用教程](http://wuyuans.com/2012/05/github-simple-tutorial/)
2.  [git命令的基本使用方法](http://blog.csdn.net/lmdyyh/article/details/19683721)
3.  [Installing and using Git and GitHub on Ubuntu: A beginner's guide](https://www.howtoforge.com/tutorial/install-git-and-github-on-ubuntu-14.04/)
4.  [Git与Github的使用(ubuntu)](http://www.pureweber.com/article/git-and-github/)
5.  [linux git ssh 无密码登陆 免密码登陆](http://jinchen.blog.51cto.com/6745578/1639230)
6.  [Error: Agent admitted failure to sign](https://help.github.com/articles/error-agent-admitted-failure-to-sign/)

注：个人认为第三篇参考文章比较简洁，很具有参考价值。其次是第一篇文章，很详细。
