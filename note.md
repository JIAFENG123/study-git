- git init  初始化git环境，目录里会添加.git文件夹
- git add file -----file可以是文件名，可以是.提交所有文件
- git commit -m "some"   ---some是备注，提交的一些备注
- git reset --hard commit id ---版本回退，git reset --hard HEAD^ 是返回上一版本，上上版本为HEAD^^，以此类推，在cmd里^为特殊字符，表示没有输入完全，需要输入HEAD^^两个^来转义成一个，以此类推
- git log ---查看提交历史版本，可查看提交时的id，方便回退，git log --pretty=oneline 使打印结果简洁
- git reflog ---在回退时，有可能不是单向的，想退回最新版本，可以查看所有提交得commitid
- 工作区 ---我的文件     暂存区 ---stage .git文件
- git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别，就是这个文件已经提交到版本库，但是本地修改了，可查看差异
- 文件中修改了内容，但是还没有git add ，可以使用git checkout -- file 来进行回退，如果add了，先使用git reset HEAD file进行回退到工作区，然后使用git checkout -- file来恢复最初状态，如果内容commit了，则使用git reset --hard commitid 来进行回退版本
- 如果本地创建了一个文件，已经commit了，然后本地删了，
> 1. 如果版本库中也需要删除，使用git rf file删除，然后git commit，，
> 2. 如果想要还原本地文件，使用git checkout -- file回退回来
- 关联远程库，使用命令 git remote add origin git@server-name:path/repo-name.git,使用git push -u origin master提交内容，以后直接git push origin master推送最新修改
- 克隆远程项目，使用git clone git链接，  git支持多种协议，包括https和ssh，ssh协议速度最快
- 创建分支，使用git checkout -b dev 创建dev分支，并切换过去，相当于 git branch dev  git checkout dev 两个命令
在当前分支，修改内容，合并到主分支，使用git merge name
删除分支使用git branch -d name
切换分支：git checkout name或者git switch name
创建+切换分支：git checkout -b name或者git switch -c name
- 处理冲突
> 当在不同的分支上修改东西，都commit了，这时候合并分支的时候会冲突，这时候在master分支上处理冲突，然后add，commit，删除其他分支即可，使用git log --graph可以查看分支提交和合并图
- 分支合并时
> git merge --no-ff -m "merge with no-ff" dev，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并
git log --graph --pretty=oneline --abbrev-commit 查看合并图
- 处理bug
> 在dev分支进行开发时，先git stash暂存dev分支，然后可以切换到master分支新建一个处理bug的分支，然后处理完提交，删除bug分支，切换到dev分支，执行git stash pop回到最初状态，在master上修复的bug，复制到dev，使用git cherry-pick bug分支提交的commitid 进行复制
- 删除工作分支 --- 如果要丢弃一个没有被合并过的分支，可以通过git branch -D name强行删除

- 多人协作

1. 首先，可以试图用git push origin branch-name推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！
4. 如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to branch-name origin/branch-name
- git rebase
> rebase操作可以把本地未push的分叉提交历史整理成直线；
rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。
- 标签操作
> git tag tagname 默认为HEAD,也可以指定commitid，git tag tagname commitid,可以指定标签描述信息，git tag -a tagname -m "description"，，git tag查看所有标签
命令git push origin tagname可以推送一个本地标签；
命令git push origin --tags可以推送全部未推送过的本地标签；
命令git tag -d tagname可以删除一个本地标签；
命令git push origin :refs/tags/tagname可以删除一个远程标签。
