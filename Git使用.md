### Gitignore

提交时忽略某些文件

在git目录下创建.gitignore文件

模板地址：https://github.com/github/gitignore

用法：

- #表示注释
- 忽略文件夹：写路径 例：src/build
- ！表示取反 （在之前被忽略的文件夹中，扔希望提交的）
- glob模式匹配

### 提交

git作为支持分布式版本管理的工具，它管理的库（repository）分为本地库、远程库。

git commit操作的是本地库，git push操作的是远程库。



### 具体命令

![image-20210729202800197](Git使用.assets/image-20210729202800197.png)

- git commit 提交到本地仓库 （-m "本次提交信息"）
- git branch demo 创建分支
- git checkout demo 切换到demo分支      可以移动head指向，通过hash值或相对引用  /撤销未add的修改
- git merge main  / git rebase main     合并/ 变基  到main分支上

![图片](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghxahaqaouj30qw0qmwgy.jpg)

- git init 初始化
- git clone [url]  从远程仓库克隆
- git status 
- git diff :  比较未暂存区和暂存区的代码差异
- **git** **push origin master**  将本地代码提交到远程仓库的`master`分支
- **git** **pull origin master **  从远程仓库拉取其他人的`更新到本地仓库`（`本地分支和远端分支都更新合并`）
- git fetch：把远端分支的更新拉到本地的`远端分支`，此时还没有和本地分支进行合并。 可以使用 merge合并或 rebase变基合并
- git commit --amend：修改 commit注释

`work dir`和`stage`区域的状态，可以通过命令`git status`来查看，`history`区域的提交历史可以通过`git log`命令来查看

只要你不乱动本地的`.git`文件夹，任何修改只要提交，提交到History，都永远不会丢失。若查看不到某些commit，只是因为它们不是我们当前`HEAD`位置的「历史」提交，可通过 git reflog 查看所有记录

若代码冲突：

1. git stash  先将本地的修改保存的栈中
2. **git** **pull**  拉取远程仓库代码
3. **git** **stash pop**  把刚刚修改的代码放出来
4. 会报出冲突 更改 
5. 重新`add -> commit -> push`即可

代码回滚

- reset ： 取消添加   将HEAD指针指到指定提交，历史记录中不会出现放弃的提交记录。
  - 参数：--mixed：（默认） 不删除工作空间改动代码，撤销commit，并且撤销git add . 操作
  - --soft：不删除工作空间改动代码，撤销commit，不撤销git add .
  - --hard：删除工作空间改动代码，撤销commit，撤销git add .
-  revert



工作原理：http://szufrank.top/#/./docs/git-work

Git 可视化 https://learngitbranching.js.org