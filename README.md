#HEAD 指向的版本就是当前版本
origin/master 远程分支指针（与master类型类似）
FETCH_HEAD： 是一个版本链接，记录在本地的一个文件中，指向着目前已经从远程仓库取下来的分支的末端版本。
#配置：
git的配置文件分为三个级别，分别是：
system：负责本机全部用户, PATH=$INSTALL_PATH/etc/gitconfig
global：本用户下所有仓库， PATH=~/.gitconfig
local：仅仅负责当前仓库，PATH=$REPO/.git/config
可以通过git config --[system|global|local] 命令，交互式修改配置文件
git config --global user.name "xiaolan"
git config --global user.email  2717110178@qq.com
git config -l  /--[system|global|local] --list
git init 
git clone 连接

#更新
git add ./git add -A #添加所有文件到暂存区(包括删除文件)
git status 	#查看文件状态：Untracked:未跟踪、
		#Unmodify:文件已经入库，未修改、
		#Modified:文件已修改，仅仅是修改，并没有进行其他的操作、
		#Staged：暂存状态
git commit -m "描述" #提交暂存区内容到版本库,
·		#-a 在 commit 的时候使用git add更新暂存区文件（仅对修改和删除文件有效）
git reflog #查看历史命令
git log #查看记录
git rm 文件名： 同时从工作区和暂存区中删除文件(在commit后版本中删除文件)。
git rm --cached 文件名： 从暂存区中删除文件(在commit后版本中删除文件)，工作目录文件将被保留（变为Untracked状态）
#删除文件：
rm 文件（但提示:在Changes not staged for commit中(没有使用add添加文件)） && git add 文件 &&git commit -m
#比较
git diff 文件名 #比较暂存区和工作区文件
git diff HEAD --文件名 #比较工作区和版本库

#回退版本
git checkout -- <file> 命令来把暂存区文件拉到工作区
git reset --hard HEAD /HEAD^ / ^^ / ~121 /部分版本号 #版本库->暂存区->工作区
git reset HEAD <file> 拉取最近一次提交到版本库的文件到暂存区==git restore --staged file

#远程仓库
git clone 连接 #克隆仓库（包括推送和分支）
ssh-keygen -t rsa -C "youremail@example.com" #密钥类型可以用 -t 选项指定,-C来指定密钥中的注释字段的注释
设置github  ssh key
git remote add origin（远程仓库名字） git@github.com:qwerters/gittest.git(使用git remote rm origin解除与远程仓库的绑定)
git push <远程主机名> <本地分支名>:<远程分支名> 或git push -u（=push一次+设置远程分支与本地分支关联） origin<远程主机名> master<分支名>(此时本地分支名与远程分支名相同) 或 git push(推送当前分支,git push --set-upstream origin dev关联当前本地分支与远端的dev分支后可使用git push)
推送分支到远程仓库（包括路径上的提交一并上传），在远端产生多个节点推送。#推送远程不存在的master分支时，可以加-u参数(验证时输入yes)（和远端仓库合并，可能存在冲突）
git pull <远程主机名> <远程分支名>:<本地分支名> 即将远程主机的某个分支的更新取回，并与本地指定的分支合并
git pull #将远程仓库中的更改合并到当前分支中（目标分支的名称必须和本地分支完全相同）（产生一次提交）==git fetch + git merge 或 git fetch + git rebase（根据配置的不同）
git rebase:
             A---B---C master on origin
           /
        D---E---F---G master
                |
                |变化
               \/
            A---B---C master on origin
          /
        D----A----B----C---E'---F'---G' master
git fetch <远程主机名> //这个命令将某个远程主机的更新全部取回本地
git fetch <远程主机名> <分支名> //只想取回特定分支的更新
git fetch <远程主机名> <远程分支名>:<本地分支名> #建立本地分支保存远程分支

git remote #查看远程库，-v显示更详细的信息
git checkout -b dev origin/dev #创建远程origin的dev分支到本地
#分支管理
git branch dev #创建dev分支
git checkout dev ==git switch master#切换dev分支
git checkout -b dev ==git switch -c dev#创建dev分支，然后切换到dev分支
git merge dev #合并dev分支到当前分支，分三种情况：
1.Fast forward模式，快进：当前 master 分支所指向的提交是你当前提交（dev的提交）的直接上游，所以 Git 只是简单的将 master 指针向前移动。
#使用git merge --no-ff -m "merge with no-ff" dev 强制禁用Fast forward模式，合并产生新的节点
2.非“快进”，修改不同文件。合并后 master 自动 commit 提交了一次，产生了新的提交包含两个分支内容
3.非“快进”，修改相同文件。合并失败，合并失败文件中被标记需要修改提交（Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容）。
git stash 切换分支前可使用git stash保存未提交的现场
git stash list查看保存的工作现场
git stash pop恢复并删除stash内容==git stash apply stash@{0}&& git stash drop stash@{0}
git stash clear #删除所有的暂存库
git cherry-pick 4c805e2 #把bug提交的修改4c805e2分支应用于当前分支到当前分支（产生一个新的提交），避免重复劳动。
# 查看本地分支
git branch | git branch -l
# 查看远程分支
git branch -r
# 查看所有分支
git branch -a
# 重命名当前分支为dev
git branch -m dev
# 重命名release分支为dev
git branch -m release dev

git branch -d dev #删除dev分支
git branch -D <name>强行删除一个没有被合并过的分支
git log --graph #命令可以看到分支合并图

#创建标签
git tag v1.0 [HEAD/id] [-m "version 0.1 released"（说明）,git show <tagname>可以看到说明文字]#给HEAD（省略[HEAD/id] ）或指定提交打标签
git tag 查看标签
git tag -d v0.1#删除标签
git push origin master --tags/v1.2 或git push [origin] [分支] --tags#--tag：推送所有标签到远程。
忽略特殊文件:建立文件.gitignore *.py !.gitignore（不能忽略已经在缓存区的文件）
git config --global alias.unstage 'reset HEAD' 设置reset HEAD命令别名为unstage
