

### [玩转Git三剑客](https://time.geekbang.org/course/intro/145)

#### 03 最小配置
user.name、user.email

#### 04 init

#### 05 add commit

#### 06 用mv重命名

#### 07 log
checkout 切换分支

--online -n4

--all -graph

#### 08 gitk

GUI

#### 09 .git内容说明

tags与heads并列

cat-file -t sha： 输出种类

cat-file -p sha： 输出内容

#### 10 commit tree blob

commit对应快照

tree对应文件夹

blob对应文件

#### 11 数tree个数

#### 12 分离头指针， 用于基于某个commit临时更改

即head未和分支绑定

避免了建立和删除分支的麻烦

#### 13 HEAD和branch

HEAD 可以指代commit

HEAD^ HEAD^1 HEAD^^ HEAD~2

diff commit1 commit2

#### 14 删除不必要的branch

-d 删除经过合并的分支
-D 未合并

#### 15 修改最新commit的message
git commit --amend

#### 16 修改任意一次commit的message
git rebase -i 上一次提交的id


#### 17 合并连续commit
git rebase -i 后squash

#### 18 合并不连续commit
修改第一个commit，添加 pick 第一个commit的id

在合并命令文件中，要合并的两个要挨着

#### 19 比较暂存区和HEAD文件的区别
git diff --cached

#### 20 比较工作区和暂存区的区别
git diff (-- 文件名)

#### 21 放弃全部暂存区
git reset HEAD

#### 22 工作区文件恢复
checkout -- index.html

#### 23 取消暂存区部分文件的更改
git reset HEAD -- 文件名

#### 24 消除最近的几次提交
git reset --hard 目标提交id

#### 25 比较提交见文件的差异
git diff 

#### 26 正确的删除文件
git rm

#### 27 暂存手上任务，做别的临时任务
git stash 存放
git stash list 查看
git stash apply 放入工作区且不删除
git stash pop

#### 28 指定不需要git
gitignore

#### 29 git的备份
智能协议和哑协议

git remote -v

git remote add

#### 30 github注册

#### 31 非对称加密配置

#### 32 github创建个人仓库
git remote add

#### 33 本地仓库同步到github

远端有文件，本机也有文件

合并

#### 34 不同人修改不同文件