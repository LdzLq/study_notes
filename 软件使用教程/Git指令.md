## Git使用心得
### Git应用
#### 安装软件
1. 安装地址:[git下载链接](https://git-scm.com/download/win)
2. 使用方法: 安装后，选择自己项目根文件夹，右键鼠标，打开git bash，之后当作正常的命令窗使用，直接输入各种git、linux指令即可
#### 创建仓库
##### 0.本地仓库和远程仓库连接
1. 设置本地开发者姓名和邮箱
```
git config --global user.name "姓名"
git config --global user.email "邮箱地址"
```
2. 生成本地ssh密钥
```
ssh-keygen -t ras -C "邮箱地址"
```
3. 找到公钥：`C:\Users\电脑用户文件夹\.ssh\id_rsa.pub`
4. 打开git远程仓库网站，注册并登录
5. 打开个人信息页面，找到Settings页面
6. 找到SSH设置，点击新建SSH Key
7. 输入SSH Key的标题（可以区分不同本地仓库），和key（当前本地仓库的公钥：步骤3）
##### 1.初始化本地仓库
1. 新建/打开项目文件夹
2. 使用`git init` 完成初始化
###### 特别注意
本地初始化仓库，上传至远程仓库步骤：
1. 必须先在远程仓库平台，新建一个空的仓库
2. 本地git bash
```
git remote add origin 远程仓库ssh路径
```
3. 再执行上传指令
##### 2.克隆远程仓库
1. 打开远程仓库网站，找到需克隆的项目
2. 找到SSH克隆指令
3. 在本地git bash中输入
```
git clone ssh克隆路径
```
#### 管理仓库
##### 1.分支管理
1. 查看全部分支
```
git branch -a
```
2. 新建分支
```
git branch 分支名 
```
3. 切换分支
```
git checkout 分支名
或
git switch 分支名
```
4. 删除分支
```
git branch -D 分支名
```
5. 新建分支并切换到新分支
```
git checkout -b 分支名
或
git switch -c 分支名
```
6. 同步本地分支和远程分支
```
git remote prune origin
```
7. 合并分支
```
git merge 分支名
```
8. 查看分支情况图
```
git log --graph --pretty=oneline
```
9. 本地分支和远程分支有分叉，且都有提交
```
git rebase 分支名
```
10. 继续rebase 
```
git rebase --countine
```
11. 本地分支和远程分支建立链接
```
git branch --set-upstream-to=origin/分支名 本地分支名
```
12. 强制合并两个没有共同祖先的分支
```
git pull origin 分支名 --allow-unrelated-histories
```
13. 重名本地分支名
```
git branch -m 新的分支名
```
14. 查看当前仓库的远程连接URL
```
git remote -v
```
##### 2.工作区管理
1. 查看git状态
```
git status
```
2. 取消工作区的修改（git add之前）
```
git restore ./
```
3. 将工作区修改添加到暂存区
```
git add ./
```
##### 3.暂存区管理
1. 将暂存区的更改添加到当前分支
```
git commit -m "改动的简单概述"
```
2. 取消暂存区的改动，恢复至最新的工作区目录
```
git restore --staged ./
```
##### 4.本地仓库管理
1. 将本地分支的改动推送到远程仓库
```
git push origin 分支名
```
2. 拉取远程仓库的最新代码到本地仓库
```
git pull origin 分支名
```
3. 将推送至本地分支的改变，回退至暂存区（可以修改commit内容）
```
git reset --soft HEAD^
```
4. 将推送至本地分支的改变，回退到工作区（直接清空本次的工作提交）
```
git reset --hard HEAD^
```
5. 查看历史提交记录
```
git log 
简单的输入：
git log --pretty==oneline
```
#### 删除仓库
**删除仓库直接将项目文件夹全部删除**


### Git网站
#### 网站汇总
* [github](https://github.com/dashboard)
* [gitlab](https://gitlab.com/users/sign_in)
* [中国gitee](https://gitee.com/)
#### 创建仓库
##### 以github为例
- 直接点击[新建仓库](https://github.com/new)，填写信息即可
- **或者**点击[fork别人的项目](https://github.com/kangpeiqin/bilivideo_down/fork)， 也可以
#### 管理仓库
1. 新建issue
点击[某个项目的new issue](https://github.com/kangpeiqin/bilivideo_down/issues/new/choose)，创建并提交issue
1. 管理分支
点击[查看分支信息](https://github.com/kangpeiqin/bilivideo_down/branches)
1. 提交PR/MR
点击[PR](https://github.com/kangpeiqin/bilivideo_down/pulls)，再点击[new pull request](https://github.com/kangpeiqin/bilivideo_down/compare)





