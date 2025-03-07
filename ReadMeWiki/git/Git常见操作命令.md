#### 目录介绍
- 01.git环境配置说明
- 02.如何和GitHub连接
- 03.最简单使用命令
- 04.查看代码库信息
- 05.拉取，提交与推送操作
- 06.分支操作
- 07.工作区操作
- 08.远程同步
- 09.增加/删除/撤销
- 13.回滚已提交commit
- 15.遇到一些问题思考
- 20.git帮助help分析






### 00.专有名次说明
- 专有名次说明【结合图来看】
    - Workspace：工作区
    - Index / Stage：暂存区
    - Repository：仓库区（或本地仓库）
    - Remote：远程仓库




### 01.git环境配置说明
- 1.下载最新git和安装
    - http://git-scm.com/download/
- 3.验证是否成功，输入命令行。输出git版本表示git安装成功。
    - git --version
- 4.在本地git中添加你得git账户和邮箱，用于每次提交时记日志(log）
    - 注册用户名和邮箱
    ```
    git config --global user.name "你的注册用户名"
    git config --global user.emall "你的注册邮箱"
    ```
    - 获取注册用户名和邮箱
    ```
    git config --global user.name
    git config --global user.email
    ```
- 5.生成SSH key
    - 在终端输入命令：ssh-keygen -t rsa -C 你的注册邮箱。
    - 注意：这里要改成你自己的邮箱
    - 需要直接回车三下，就会生成需要的ssh-key。执行成功后，会在主目录.ssh路径下生成两个文件：id_rsa私钥文件；id_rsa.pub公钥文件； 
- 6.查找SSH
    - 第一种方式：open ~/.ssh，然后打开一个文件夹，选中id_rsa.pub用记事本打开
    - 第二种方式：
        - 终端查看.ssh/id_rsa.pub文件：open .ssh/id_rsa.pub 
        - 回车后，就会新弹出一个终端，然后复制里面的key。或者用cat命令查看：cat .ssh/id_rsa.pub
- 7.常见操作命令
    ```
    创建.bash_profile文件(命令输入 touch .bash_profile);
    打开.bash_profile文件(命令输入 open -e .bash_profile)
    执行命令 source .bash_profile;
    
    vi ~/.zshrc
    // 如果根目录没有.zshrc的话，执行下面的命令，输入:wq保存文件
    touch  ~/.zshrc
    vi ~/.zshrc
    ```


### 02.如何和GitHub连接
- 1.登录GitHub（默认你已经注册了GitHub账号），添加ssh key，点击Settings
- 2.点击New SSH key
- 3.添加key
- 4.链接验证：ssh -T git@github.com 
    - 终端输出结果，说明已经链接成功。
    ```
    Last login: Sat Jan  6 14:42:55 on ttys000
    WMBdeMacBook-Pro:~ WENBO$ ssh -T git@github.com 
    Hi wenmobo! You've successfully authenticated, but GitHub does not provide shell access.
    WMBdeMacBook-Pro:~ WENBO$ 
    ```



### 03.最简单使用命令
- 将项目克隆到本地仓库
    - git clone URL（项目的SSH地址）
- 更新远程更新到本地：
    - git pull
- 提交代码
    - git add .
    - git commit -m “描述”
    - git push origin master
    - git push -u origin +master   【如果还是无法提交，可以尝试强制更新】
- 查询提交状态
    - git status
- 如何忽略文件
    - 使用.gitignore忽略文件，在.gitignore文件当中，一行代表一条忽略规则，如果是一个带“.”这种有后缀的字符串那么git就会忽略这个文件
    - .gitignore 文件的用途，只能作用于 Untracked Files，也就是那些从来没有被 Git 记录过的文件（自添加以后，从未 add 及 commit 过的文件）。
    - 注意：.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。
    - 对于已经提交过文件，想要让ignore生效， 也是有办法的：
        > 使用git rm --cached 从 Git 的数据库中删除对于该文件的追踪；(有一点需要注意的，git rm --cached 删除的是追踪状态，而不是物理文件)
        > 把对应的规则写入 .gitignore，让忽略真正生效；
        > 提交＋推送。
- 暂时忽略某个文件的修改
    - 开发过程中可能还会遇到这样的情况，某个文件没有修改好，但是又要提交代码， 想这次忽略这个文件，下一次提交时再去提交它。
        > git update-index --assume-unchanged
    - 当你的工作告一段落决定可以提交的时候，重置该标识：git update-index --no-assume-unchanged，于是 Git 只需要做一次更新，这是完全可以接受的了；
    - 提交并推送代码到远程库


### 04.查看代码库信息
- 显示有变更的文件
    > $ git status
- 显示当前分支的版本历史
    > $ git log [--pretty=oneline] //--pretty=oneline 参数可以简化输出信息
- 显示当前分支的最近几次提交
    > $ git reflog
- 显示暂存区和工作区的差异
    > $ git diff
- 显示暂存区和上一个commit的差异
    > $ git diff --cached [file]
- 显示工作区与当前分支最新commit之间的差异
    > $ git diff HEAD
- 查看文件内容
    > $ cat [file]
- 查看分支合并情况
    > $ git log --graph --pretty=oneline --abbrev-commit
- 查看分支合并图
    > $ git log --graph



### 05.拉取，提交与推送操作
- 下载远程仓库的所有变动
    > $ git fetch [remote]
- 取回远程仓库的变化，并与本地分支合并
    > $ git pull [remote] [branch]
- 添加文件到暂存区。文件的相对路径
    > $ git add [file1] [file2] ... //添加文件名
    > $ git add [dir]  //添加目录
    > $ git add . //添加当前目录的所有文件(不包括删除文件)
    > $ git add -A //(-A : --all的缩写)添加当前目录的所有文件
- 提交暂存区到仓库区
    > $ git commit -m [message]
- 推送到远程仓库
    > $ git push [remote] [branch]
    > 
    > $ git push [remote] --all
    > 
    > $ git push origin(远程仓库名称) master(分支名称) //将master分支上的内容推送到远程仓库，如果远程仓库没有master分支将创建



### 06.分支操作
- 查看本地所有的分支
    > git branch
- 查看远程仓库信息
    > $ git remote -v
- 列出所有本地分支和远程分支
    > $ git branch -a
- 新建一个分支，并切换到该分支
    > $ git checkout -b [branch]
    > $ git branch [branch-name] //新建一个分支，但依然停留在当前分支
    > $ git checkout [branch-name] //切换到指定分支，并更新工作区；如果是远程分支将自动与远程关联本地分支
- 新建一个分支，指向指定 commit
    > $ git branch [branch] [commit]
- 新建一个分支，与指定的远程分支建立追踪关系
    > $ git branch --track [branch] [remote-branch]
- 设置本地分支与远程origin分支链接
    > $ git branch --set-upstream [branch] origin/[branch]
- 合并指定分支到当前分支
    > $ git merge [branch]
- 查看分支合并情况
    > $ git log --graph --pretty=oneline --abbrev-commit
- 查看分支合并图
    > $ git log --graph
- 删除分支
    > $ git branch -d [branch-name]
    > 
    > $ git branch -D [branch-name]	//强行删除
- 删除远程分支
    > $ git push origin --delete [branch-name]
    > 
    > $ git branch -dr [remote/branch]


### 07.工作区操作
- 什么是工作区
- 查看工作区
    > $ git stash save [desc]
- 查看工作区列表
    > $ git stash list
- 取出工作区内容
    > $ git stash apply [stash-name] //取出不删除
    > $ git stash drop [stash-name] //删除
    > $ git stash pop [stash-name] //取出并删除


### 08.远程同步
- 默认 origin 代表远程仓库
- 下载远程仓库的所有变动
    > $ git fetch [remote]
- 显示所有远程仓库
    > $ git remote -v
- 显示某个远程仓库的信息
    > $ git remote show [remote]
- 增加一个新的远程仓库，并命名
    > $ git remote add [shortname] [url]
- 取回远程仓库的变化，并与本地分支合并
    > $ git pull [remote] [branch]
- 上传本地指定分支到远程仓库
    > $ git push [remote] [branch]
- 强行推送当前分支到远程仓库，即使有冲突
    > $ git push [remote] --force
- 推送所有分支到远程仓库
    > $ git push [remote] --all


### 09.增加/删除/撤销
#### 9.1 增加
- 添加指定文件到暂存区
    > $ git add [file1] [file2] ...
- 添加指定目录到暂存区，包括子目录
    > $ git add [dir]
- 添加当前目录的所有文件(不包括删除文件)到暂存区
    > $ git add .
- 添加当前目录的所有文件到暂存区
    > $ git add -A(--all的缩写)
- 添加每个变化前，都会要求确认
- 对于同一个文件的多处变化，可以实现分次提交
    > $ git add -p


#### 9.2 删除
- 删除工作区文件，并且将这次删除放入暂存区
    > $ git rm [file1] [file2] ...
- 停止追踪指定文件，但该文件会保留在工作区
    > $ git rm --cached [file]
- 改名文件，并且将这个改名放入暂存区
    > $ git mv [file-original] [file-renamed]



#### 9.3 撤销
- 恢复暂存区的指定文件到工作区
    > $ git checkout [file]
- 恢复某个 commit 的指定文件到暂存区和工作区
    > $ git checkout [commit] [file]
- 恢复暂存区的所有文件到工作区
    > $ git checkout .
- 重置暂存区的指定文件，与上一次 commit 保持一致，但工作区不变
    > $ git reset [file]
- 重置暂存区与工作区，与上一次 commit 保持一致
    > $ git reset --hard
- 重置当前分支的指针为指定 commit，同时重置暂存区，但工作区不变
    > $ git reset [commit]
- 重置当前分支的 HEAD 为指定 commit，同时重置暂存区和工作区，与指定 commit 一致
    > $ git reset --hard [commit]
- 重置当前 HEAD 为指定 commit，但保持暂存区和工作区不变
    > $ git reset --keep [commit]
- 新建一个 commit，用来撤销指定 commit
- 后者的所有变化都将被前者抵消，并且应用到当前分支
    > $ git revert [commit]
- reset命令有3种方式：
    ```
    git reset –mixed：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息
    git reset –soft：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可
    git reset –hard：彻底回退到某个版本，本地的源码也会变为上一个版本的内容
    ```
- 具体怎么操作
    ```
    1、 将本地的状态回退到和远程一样
    git reset --hard origin/master
    2、将暂存区里面的修改清空 , 回退到上一次提交的记录
    git reset --hard
    3、将本地的状态回退到 某个版本
    git reset --hard 5230bb6
    ```



### 13.回滚已提交commit
- 在git使用中如果提交错误的代码至远程服务器，可以使用git revert 命令回滚单次commit并且不影响其他commit。
    - 回滚最新一次的提交记录： git revert HEAD
    - 回滚前一次的提交记录 ： git revert HEAD^
    - 对历史上的commit回滚： git revert
- 回滚历史commit很容易产生文件冲突，需要做好冲突处理。
- 在准备revert 的commit上右键 选择 reverse commit。 
    - revert命令与reset命令不同，是生成一次新的commit冲抵原来的commit，reset直接删除某些commit的内容。
    - Revert历史上的commit 很容易产出文件冲突， 在这次回滚中，对于有冲突的文件都没有进行回滚，只将未产生文件冲突的文件进行了回滚。
- 确认生成的新commit编译成功，也没有文件冲突，可以push到服务器，完成回滚。



### 20.git帮助help分析
- 输入git help，会看到下面这些提示信息
    ```
    didi1@DIDI-C02F31XVML7H YCFlutterTool % git help
    usage: git [--version] [--help] [-C <path>] [-c name=value]
               [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
               [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
               [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
               <command> [<args>]
    
    These are common Git commands used in various situations:
    
    start a working area (see also: git help tutorial)
       clone      Clone a repository into a new directory
       init       Create an empty Git repository or reinitialize an existing one
    
    work on the current change (see also: git help everyday)
       add        Add file contents to the index
       mv         Move or rename a file, a directory, or a symlink
       reset      Reset current HEAD to the specified state
       rm         Remove files from the working tree and from the index
    
    examine the history and state (see also: git help revisions)
       bisect     Use binary search to find the commit that introduced a bug
       grep       Print lines matching a pattern
       log        Show commit logs
       show       Show various types of objects
       status     Show the working tree status
    
    grow, mark and tweak your common history
       branch     List, create, or delete branches
       checkout   Switch branches or restore working tree files
       commit     Record changes to the repository
       diff       Show changes between commits, commit and working tree, etc
       merge      Join two or more development histories together
       rebase     Reapply commits on top of another base tip
       tag        Create, list, delete or verify a tag object signed with GPG
    
    collaborate (see also: git help workflows)
       fetch      Download objects and refs from another repository
       pull       Fetch from and integrate with another repository or a local branch
       push       Update remote refs along with associated objects
    
    'git help -a' and 'git help -g' list available subcommands and some
    concept guides. See 'git help <command>' or 'git help <concept>'
    to read about a specific subcommand or concept.
    ```
- 然后这些是什么意思呢？
    ```
    didi1@DIDI-C02F31XVML7H YCFlutterTool % git help
    usage: git [--version] [--help] [-C <path>] [-c name=value]
               [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
               [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
               [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
               <command> [<args>]
    
    These are common Git commands used in various situations:
    
    start a working area (see also: git help tutorial)
       clone      Clone a repository into a new directory
       init       Create an empty Git repository or reinitialize an existing one
    
    work on the current change (see also: git help everyday)
       add        Add file contents to the index
       mv         Move or rename a file, a directory, or a symlink
       reset      Reset current HEAD to the specified state
       rm         Remove files from the working tree and from the index
    
    examine the history and state (see also: git help revisions)
       bisect     Use binary search to find the commit that introduced a bug
       grep       Print lines matching a pattern
       log        Show commit logs
       show       Show various types of objects
       status     Show the working tree status
    
    grow, mark and tweak your common history
       branch     List, create, or delete branches
       checkout   Switch branches or restore working tree files
       commit     Record changes to the repository
       diff       Show changes between commits, commit and working tree, etc
       merge      Join two or more development histories together
       rebase     Reapply commits on top of another base tip
       tag        Create, list, delete or verify a tag object signed with GPG
    
    collaborate (see also: git help workflows)
       fetch      Download objects and refs from another repository
       pull       Fetch from and integrate with another repository or a local branch
       push       Update remote refs along with associated objects
    
    'git help -a' and 'git help -g' list available subcommands and some
    concept guides. See 'git help <command>' or 'git help <concept>'
    to read about a specific subcommand or concept.
    ```














