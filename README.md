# v1 配置 git 用户及mail
git config --global user.email wanglei@xkeshi.com
git config --global user.name wanglei@xkeshi.com

# v2 添加 提交 上传
git add git.txt
git commit -m 'version 1'
git push

# v3 查看提交命令 查看历史命令 重置commitid
git log
git log --pretty=oneline
git reflog
git reset --hard commit-id
git reset --hard HEAD       (HEAD指当前版本)
git reset --hard HEAD~1     (回到前一个版本)
git reset --hard HEAD~10    (回到前十个版本)

# v4 git 工作区与版本库 
1.工作区(git工作目录/) 
2.版本库(git工作目录下的.git/目录)

创建Git版本库时,Git自动创建了唯一一个master分支,git commit 就是往master分支上提交更改
需要提交的文件修改暂时通通放到暂存区,然后,一次性提交暂存区的所有修改

1.将当前工作区修改的文件添加到缓存区
git add
git status

2.将缓存区的所有内容提交到当前分支master
git commit -m 'info'
git status

3.将当前master分支的所有内容push到远程仓库
git push
git status

# v5 git 撤销修改
1. git checkout -- git.txt
   丢弃当前工作区的修改

2. git reset HEAD git.txt
   将暂存区的修改撤销掉,重新放回工作区

3. git reset --hard commitid
   回退版本,回退到以前的commitid版本号中

# v6 删除文件
1. rm file.txt
2. git rm file.txt
3. git commit -m 'delete file.txt'
4. git push

误删文件回滚操作-1
1. rm file.txt
2. git checkout -- file.txt

误删文件回滚操作-2
1. git rm file.txt
2. git reset HEAD
3. git checkout -- file.txt

# v7 github 添加密钥
1. 本地生成密钥文件
2. 将公钥上传至github
3. git clone 项目(use ssh protocol)

