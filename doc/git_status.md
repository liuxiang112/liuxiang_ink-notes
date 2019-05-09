### git 常用命令：git status

各种版本控制工具层出不穷，有时候这种还没学会，新的又出来了，所以决定回归本质：命令行。so，**git status**作为最常用到命令之一就出场了。

#### 描述
git status命令用于显示工作目录和暂存区的状态。使用此命令能看到以下3种情况：

1. 哪些修改被暂存到了
2. 哪些没有被暂存到
3. 哪些文件没有被Git tracked（跟踪）到。
   
git status不显示已经commit到项目历史中去的信息。看项目历史的信息要使用git log.

<font color="#f00">**注意**:</font>第一种是通过运行git commit来提交的; 第二种和第三种是你可以通过在运行git add，git commit来提交的。

举例说明：
````
# On branch master
# Changes to be committed:  (已经在stage区, 等待添加到HEAD中的文件）
# (use "git reset HEAD <file>..." to unstage)
#
#modified: hello.txt
#
# Changes not staged for commit: (有修改, 但是没有被添加到stage区的文件（还不在暂存区）)
# (use "git add <file>..." to update what will be committed)
# (use "git checkout -- <file>..." to discard changes in working directory)
#
#modified: main.txt
#
# Untracked files:(没有tracked（跟踪）过的文件, 即从没有add过的文件)
# (use "git add <file>..." to include in what will be committed)
#
#hello.txt
````

**说明**：
- 上面输出结果中”Changes to be committed“中所列的内容是在索引中的内容，git commit提交之后进入Git工作目录。
- 上面输出结果中“Changes not staged for commit”中所列的内容是在工作目录中的内容，git add之后将添加进入索引，git commit提交之后进入Git工作目录。
- 上面输出结果中“Untracked files”中所列的内容是尚未被Git跟踪的内容，git add之后进入添加进入索引，git commit提交之后进入Git工作目录。
- 通过git status -uno







