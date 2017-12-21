### Git
- [Git官网](https://git-scm.com/)
- [Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
[###Git 基础图解、分支图解、全面教程、常用命令###](http://www.cnblogs.com/cheneasternsun/p/5952830.html)
[git常见命令和参数](http://blog.csdn.net/u012570105/article/details/50703907)
- git init                        # 初始化/创建仓库
- git add README.md               # 添加修改到仓库
- git commit -m "add/update xxx"  # 提交到仓库
- git status                      # 查看仓库状态
- git diff README.md              # 查看修改内容
- git log [--pretty=oneline]      # 查看历史记录
- git reset --hard HEAD           # 放弃修改, HEAD指向最后一次commit位置, 同时放弃暂存区和工作区的修改, --soft只是将HEAD指向最后一次commit位置
- git reset --hard HEAD^/commitId # 版本回退, HEAD表示当前版本, HEAD^和HEAD^^分别表示回退1和2个版本, HEAD~100表示回退100个版本; 回退之后, git log就只会显示该版本及以前的内容了, 这时如果希望再前进恢复, 且知道希望恢复的commitId值, 则可以用"git reset --hard commitId"这种方式.
- git reflog                      # 记录执行过的命令, 可以通过该方法查commitId
- git diff HEAD -- README.md      # 查看工作区和版本库里最新版本之间的区别
- git checkout -- README.md       # 放弃工作区的修改(git add之前), 但是不能撤销已git add到暂存区的修改, 可配合git reset撤销暂存区修改
- git reset HEAD README.md        # 撤销暂存区的修改(git commit之前), 但不是放弃工作区的修改, 可配合git checkout放弃工作区修改, 如果已经git commit, 需要使用版本回退(git reset --hard HEAD^)
- git rm README.md                # 删除版本库文件, 本地文件删除后, 如果也希望删除版本库该文件, 用git rm, 然后git commit, 如果是误删除, 用"git checkout -- README.md"恢复
- git remote add origin git@{server-name}:{path}/{repo-proj-name}.git  # 关联当前路径的本地库到远程库, 远程库命名为origin(适用于先有本地库后关联远程库, 如果是用clone则无需该操作)
- git remote rm origin            # 删除远程库连接           
- git push [-u] origin master     # 将本地库所有内容推送到远程库, "origin master"等同"origin master:master", 即将本地master分支关联到远程origin repository的master同名分支. 第一次提交用-u参数做关联, 以后可以省略该参数.
- git clone git@{server-name}:{path}/{repo-proj-name}.git   # 从远程库克隆一个本地库, Git支持多种协议, 默认git://使用ssh协议, 还支持https等协议.
- git checkout -b dev             # 创建新分支dev(创建dev指针), 并切换到dev分支(等于"git branch dev" + "git checkout dev")
- git checkout -b dev origin/dev  # 创建远程dev分支到本地, 默认clone只是将远程的master创建到本地
- git branch                      # 查看本地分支, 其中带有*号的表示当前分支
- git merge dev                   # 将dev分支的工作成果合并到当前分支(假设当前分支是master, 只是将master指针指向dev指针相同位置)
- git branch -d dev               # 删除dev分支(删除dev指针), 当dev分支有commit, 但是并没有merge到其它分支前, 删除是会报错的, 需要更改为"-D"表示强制删除
- git stash                       # 隐藏当前工作现场(工作区和暂存区), 通常用于修复bug时, 从master新建bugfix分支前, 将当前的工作现场隐藏, 以保存更新的工作, 以便bug修复后在恢复隐藏工作
- git stash list                  # 查看隐藏的工作
- git stash apply                 # 恢复隐藏的工作, 但不删除stash内容, 需要用"git stash drop"删除; 用"git stash apply stash@{0}"恢复指定的stash
- git stash pop                   # 恢复隐藏的工作, 并且删除stash内容
- git remote                      # 查看远程库(是库, 不是分支, 比如,是origin, 不是master或dev), "git remote -v" 显示更详细的信息
- git push origin master          # 将本地master分支提交的内容全部推送到远程库origin的master分支, 如果需要推送其它分支, 就更改分支名称, 如"git push origin dev".
- git tag                         # 查看标签, tag跟分支一样也是指针, 只不过tag指针不会随着commit移动, 虽然用commit号可以回滚, 但不容易记忆, tag就是便于记忆的commit号
- git tag {tagname}               # 创建标签, 参数-a指定标签名, -m指定说明; 对历史上的某次提交打标签(当时忘记打标签了), 则用"git tag {tagname} {commit号}"
- git show {tagname}              # 查看某标签详细信息
- git push origin --tags          # 将本地所有尚未推送的标签一次性全部推送(打标签都在是本地)
- git push origin {tagname}       # 推送某标签到远程
- git tag -d {tagname}            # 删除本地标签
- git push origin :refs/tags/{tagname}  # 本地删除某标签后, 也删除远程库标签

### 常用的配置别名
- config文件路径在 ~/.gitconfig(global) 或者 .git/config(project)
- git config --global alias.st status
- git config --global alias.co checkout
- git config --global alias.ci commit
- git config --global alias.br branch
- git config --global alias.unstage 'reset HEAD'  # 撤销暂存区修改
- git config --global alias.last 'log -1'         # 显示最后一次提交
- git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

### 工作区和版本库(暂存区+分支)

![工作区和暂存区](../att/git-workspace-stagespace.jpeg)
- 电脑里能看到的目录是工作区
- 工作区的隐藏目录.git是版本库
- Git版本库中有很多东西, 其中stage(或者叫index)是暂存区, 还有Git为我们自动创建的第一个分支master, 以及指向master的指针叫HEAD
- 文件往Git版本库提交的时候分两步, "git add"是把文件从工作区**添加**到暂存区, "git commit"是从暂存区**一次性提交**到当前分支

### 是否推送分支到远程库的实践
- master是主分支, 时刻与远程同步;
- dev是开发分支, 团队开发也需要与远程同步;
- bug分支没必要推送, 除非老板想看到bug修复记录;
- feature分支是否推送取决于是否团队合作开发;
- 其他分支都可以在本地藏着玩, 不必推送.

### 解决分支合并冲突
- 创建新分支feature1, 并在该分支上提交了代码(git checkout -b feature1; git add readme.md; git commit -m "update line5";)
- 然后切换回master分支, 并修改了同文件同行内容(git checkout master; git add readme.md; git commit -m "update line5 too";)
- 现在master和feature分支都有了新提交, 效果是这样的: ![](../att/git-merge-conflict.png)
- 这时把feature分支merge到master是会有冲突的(git merge feature1 或者 git merge --no-ff -m "merge with no-ff" feature1), 合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并
- 可以用"git status"查看到冲突文件, 打开文件后<<<<<<<, =======, >>>>>>>标记出不同分支的内容, 需手动修改该文件
- 手动修改后, 重新提交(git add readme.md; git commit -m "conflict fixed";), 这时分支效果是这样的: ![](../att/git-merge-conflict-fixed.png)
- 使用git log --graph [--pretty=oneline --abbrev-commit] 可以看到合并情况
- 最后, 删除无用的feature1(git branch -d feature1)

### 解决提交冲突(同文件错时更新)
- git clone默认的是指克隆远程master分支到本地, 可以用git branch检查一下
- 一般会需要把dev分支也克隆到本地, 可以用git checkout -b dev origin/dev
- 如果有人先在dev分支做了修改, 并且执行了"git commit -m "和"git push origin dev"
- 之后, 其他人在dev分支的相同文件做修改, 再推送时就会报错"error: failed to push"
- 这时, 需要先做"git pull", 把最新的提交从远程origin/dev抓取下来, 然后再推送
- 但是, "git pull"可能也会失败, 提示"There is no tracking information for the current branch.", 原因是本地dev和远程origin/dev分支没有做链接, 需要**先执行"git branch --set-upstream dev origin/dev"**
- 建立连接后, 再执行"git pull", "git commit -m"和"git push origin dev", 其中"git pull"时, 或许会有合并冲突, 需要手动解决

