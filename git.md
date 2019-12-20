1. **仓库初始化：**
   命令：`git init`
   作用：将会生成.git 的文件，是用来对我们代码进行备份的隐藏地方，但是用来获取备份文件
   的命令又是一样的，怎么来区分那份代码是自己想获取的备份的文件呢，看第二点。
2. **git 配置账户**
   命令：`git config --global user.name "名字"`
   `git config --global user.email "邮箱"`
   作用：每一次是谁备份的代码，可以找到归属
3. **把代码文件放入备份仓库里面**
   命令：`git add ./文件.js` （简化：`git add .`/或者`git add .`（将当前目录下所有文件打包））
   `git commit -m "注释信息"` //一定要加-m，否则会进入编辑器
4. **把备份仓库的代码传入正式仓库**
   命令：
5. **查看当前状态**
   命令：`git status`
   作用：如果有修改的文件就可以把他放入备份仓库中，用于查看是否有需要备份的东西
   （如果提示红色字：有更改，却没有 add(或者可以合并一次性使用`git commit --all -m "注释信息"`);
   如果是绿色：add 了可以 commit 了）
6. **查看日志文件**
   命令：`git log（git log --oneline）`
   作用：显示提交记录(以一行的形式显示 log)
7.  **版本回退（如下两种方式都可以）**
   命令：`git reset --hard Head~0`
   作用：回退到下标为 0 的版本（下标规则[2019.04,2019.03,2019.01]，直接改变下标就可以了）
   命令 2：`git reset --hard 版本号`
   作用：版本号可以通过查看日志行来取得
8. **查看版本切换记录**
   命令：`git reflog`
   作用：通过这种方式可以看到以前的版本号
9. **创建分支**
   命令：`git branch 分支名字`
10. **切换分支**
    命令：`git checkout 分支名字`
11. **合并分支（切换到主分支下）**
    命令：`git merge 想要合并的分支名字`
    作用：把两个分支内容进行合并了，在这之前必须要在`开发的分支上`面进行提交`git add .` `git commit -m "xxx"` `git push origin 开发分支名字`，然后再切换到主分支下面进行合并分支
12. **查看有哪些分支**
    命令：`git branch`
13. **上传、拉取、克隆代码**
    （我们在 push 的地址后面加个“-u”就会和远程分支进行关联，后面 push\pull 就直接 git push/pull）
    命令：`git push https地址 master`（或者是其他远端分支）
    `git pull https地址 master`（或者是其他远端分支）//多次使用会合并代码
    `git clone https地址`//多次使用会覆盖当前目录
    `git pull origin master --allow-unrelated-histories`//强制拉取代码
14. **SSH**（在任意目录下，当用 gits 地址的时候就需要用到 SSH 配置）
    命令：`ssh-keygen -t rsa -C "邮箱"`
    就在网上搜索 git 生成 ssh 密钥，，密钥在 ZD 中`.pub`文件（公钥）中，复制到 github 网站上中的 ssh Key 中
15. **简写地址**
    命令：`git remote add 重命名 仓库地址`
16. **查看当前仓库名字有哪些**
    命令：`git remote -v`
17. **修改已经提交的最后一次 commit 文本**
    命令：`git commit --amend`
    然后按`insert`键，然后在之前的文本上面进行操作，然后`:wq!`
    然后`git log`查看是否修改成功，然后`git push origin master`

1) git add .//存储这个目录下面所有文件
2) git commit -m "完成 login。html 并在 js 里加入 jq"//完成项目时提交
3) git push origin master//让组头的其他人可以看见
4) git pull origin master//把他们 push 的项目同步到自己这里来

gitk 查看修改的内容
fetch 和远程仓库同步，
m 和仓库合并
pull=fetch+m；

git remote add origin-local http://192.168.20.199:90/datav/cqhic-datav-spring-service.git

SSH：
git config user.name "ccq"//这个名字就是 git 上面显示提交者的显示的名字
git config user.email "qing.chen@zdmedical.com.cn"
ssh-keygen -t rsa -C "qing.chen@zdmedical.com.cn"

pull 出错报 access 问题，就在网上搜索 git 生成 ssh 密钥，，密钥在 ZD 中.pub 文件中，复制到 github 网站上中的 ssh Key 中
如果遇到重复输入密码的，可能是仓库地址不对

ccq
c801314q%

github 作为仓库的时候，，初次上传代码会让输入用户名密码（github 官网登陆的账号密吗），然后提示 Username for 'https://github.com':
输入 GitHub 的用户名，及密码。
====================================================

Prefix

Emoji

Emoji code

Commit 说明
