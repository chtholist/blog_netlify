# Git 初学笔记






# Git 

最先进的分布式版本控制系统，底层是c语言  
git在本地保存着完整的历史版本，可以脱机查看开发的历史版本  



#### 连接GitHub

> 本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以要有一个自己的ssh key，然后放在github上去

先看下本地是否有ssh key：本地的ssh key在 C://Users/[我]/.ssh 中，是rsa文件。若无，则进行下面的步骤

1. `cd` —— 进到git的主目录
2. `ssh-keygen -t rsa -C "github邮箱"`  —— 生成ssh key
3. `clip < ~/.ssh/id_rsa.pub` —— 复制ssh key
4. Github - settings - new SSH key - 粘贴






#### 用户信息  
`git config --global user.name "chtholist"`
`git config --global user.email [email]`  
- 用了 --global 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。  
- 要在某个特定的项目中使用其他名字或者电邮，只要去掉 --global 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里（第3级）。


#### 远程地址  
- ` git remote add origin [url] `

  加完之后进入.git，打开config，这里会多出一个remote "origin"内容，这就是刚才添加的远程地址，也可以直接修改config来配置远程地址。
  可以用 ` git remote -v ` 来检查是否关联成功
  此外，Git 还会尝试找寻 /etc/gitconfig 文件，只不过看当初 Git 装在什么目录，就以此作为根目录来定位。   



---
### 流程

1. **新建仓库**  
    ` git init `   当前目录下会出现.git目录，跟踪版本库，存放Git的数据和资源    
   ( 版本库 = 仓库 = repository )  
	- 所有的版本控制系统，只能跟踪**文本文件**的改动
	- 创建版本库时，Git自动创建了唯一一个 master 分支

2. **添加文件到本地库**   
    ` git add [filename]`  ( + ` git add * ` —> 将文件添加到暂存区 )  
    ` git add . `添加所有文件  
    （添加的文件必须在当前目录中）

3. **提交文件到本地库**  
    ` git commit -m "提交说明" ` —> 将文件提交到本地仓库（分支）中，但还没提交到远端仓库   
    ` git commit -a -m "提交说明" `  —> 自动提交本地修改  
    ( -a 可将所有被修改or已删除的且已被git管理的文档提交到仓库，但不会提交新文件 )  
   - 可以多次add不同的文件，然后再一次性commit很多文件 
   - `git add` 把工作区的修改提交暂存区，`git commit` 把暂存区的修改保存到本地库，`git push` 把本地库的记录推送到远程库   
4. **关联远程库**		` git remote add branch_Name(master) [url] `    
   
5.  **推送**	` git pull origin master `
---

- ` git status ` 查看当前本地库的状态 (是否有修改)  
 - ` git diff ` 查看前后的改动  
	   - `-`改动前，`+`改动后 / index后接git哈希值 
	   - 在commit后git diff 不会显示任何东西  
- `git log` 查看提交过的所有版本信息
	- commit id是一个SHA1计算出来的一个非常大的数字，用十六进制表示
	- 最后一行显示commit时写的提交说明
	- 简化显示，加上参数`--pretty=oneline`

- **退回版本**	`git reset --hard HEAD/[commit id]`	（commit id不必写全）
	- Git内部有个指向最后提交结果（当前版本）的HEAD指针，用reset退回版本时，HEAD指向commit id指定的版本

	-`git reflog`	查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）

1. **发布版本**  
    ` git clone ssh://……………….git ` —> 从服务器克隆一个库并上传  
    ` git push ssh://……………….git(分支) ` —> 修改后推送到服务器(远端仓库)  
    ` git remote add origin server ` —> 未克隆仓库时，将仓库连接到远程服务器  


- **克隆仓库**  
  1. **创建一个本地仓库的克隆版本**  
      ` git clone /path/to/repository `  
  2. **远端服务器上的仓库**  
      ` git clone username@host:/path/to/repository `

- **取回 & 更新**  
  ` git pull ` —> 将当前分支自动与唯一一个追踪分支进行合并  
  ` git pull http://……………….git ` —> 从非默认位置更新到指定的url  
- **撤销修改**  
  ` git checkout -- [FILE] ` 	把FILE文件在工作区的修改全部撤销
	  使用暂存区中最新内容替换掉工作目录中的文件
  ` git fetch origin ` + ` git rest --hard origin/master ` —> 丢弃在本地的所有改动和提交，可以到服务器上获取最新的版本历史，并将本地主分支指向它
- **删除**  
  ` git rm file `  
- **分支branch & 合并merge**  
  分支是用来将特性开发绝缘开来的。在你创建仓库的时候，master 是"默认的"分支。在其他分支上进行开发，完成后再将它们合并到主分支上。  
  ` git branch test ` —> 创建一个新的分支 test  
  ` git checkout test ` —> 更改分支  ( 主分支 / 第一个分支 ： master )  
  ` git checkout -b test ` —> 创建分支并切换过去
  ` git merge test ` —> 合并其他分支到当前分支  
  ` git branch -d test ` —> 删除分支  
  ` git diff <source_branch> <target_branch> ` —> 预览差异(git自动合并改动时可能出现冲突conflicts)  


- ` gitk ` —> 内建图形化git  
- ` git config color.ui true ` —> 彩色的git输出  
- ` git config format.pretty oneline ` —> 显示历史记录时，每个提交的信息只显示一行  
- ` git add -i ` —> 交互式添加文件到暂存区  

---


#### 同步GitHub Pages  （在git bash 中）  
1. clone 项目  
$ git clone https://GitHub.com/chtholist/chtholist.github.io   
**// 输入上面这个（完整的网址）**   
Cloning into 'chtholist.github.io'...  
remote: Enumerating objects: 17, done.  
remote: Counting objects: 100% (17/17), done.  
remote: Compressing objects: 100% (16/16), done.  
remote: Total 17 (delta 5), reused 0 (delta 0), pack-reused 0  
Unpacking objects: 100% (17/17), 4.67 KiB | 88.00 KiB/s, done.  
2. 进入项目的文件中，创作文章  



### git fetch
`git fetch`：将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中
- eg. `git fetch https://github.com/chtholsit/c.git`	
	```
	From https://github.com/chtholist/c
	 * branch            HEAD       -> FETCH_HEAD
	```

## 问题

- 出问题没法解决就`git remote rm master` & `git remote rm origin`  

1. 无法回到Git Bash中开头为$的操作界面  
   *按下Esc，接着输入wq保存退出（连按两次大写Z）*  

2. > warning: LF will be replaced by CRLF  

  Git Bash模拟linux环境，会默认把Windows下的回车CRLF替换成linux下的换行LF，让git忽略这个换行符： ` git config core.autocrlf false `

3. > git push后出现错误 ![rejected] master -> master(non-fast-forward) error:failed to push some refs to XXX   

    1. 在添加远程仓库` git remote add origin [url] `后先` git push origin master `再` get add . `  
    2. 先 ` git pull --rebase origin master ` 再 ` git push -u origin master ` 
    3. 把远程仓库和本地同步消除差异`git pull origin master --allow-unrelated-histories` ，再重新add & commit
    4. 强制上传（**慎用**）： `git push -f origin master`

4.  Authentication failed for  

git的账号密码有问题，先查看用户信息` git config --list `，不行就重新配置` git config --global user.name [username] ` & ` git config --global uer.email [email] `   

3. > fatal: remote origin already exists   

先` git remote rm origin `，再重输` git remote add origin [url] `  



6. ***git push时提示git master branch has no upstream branch***

   直接原因：没有将本地的分支与远程仓库的分支进行关联

   根本原因：git没法判断push的目标（repository和branch）

   方法：确定这两个值

   命令：`git push -u origin master` （替换这两个值）（若远程无分支，则创建）



- master：默认创建的分支branch
- origin：默认的远程仓库repository的名字

`git remove -v` —— 查看master和origin指代啥

`git branch -a` —— 查看所有的分支

