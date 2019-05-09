### Git 之 恢复修改的文件

对于恢复修改的文件，就是将文件从仓库中拉到本地工作区，即 仓库区 ----> 暂存区 ----> 工作区。

对于修改的文件有三种情况：

1. 只是修改了文件，没有任何 git 操作
2. 修改了文件，并提交到暂存区（即编辑之后，git add了但没有 git commit -m ....）
3. 修改了文件，并提交到仓库区（即编辑之后，git add和 git commit -m ....）

**情况1**：只是修改了文件，<font color="red">没有任何 git 操作</font>，直接一个命令就可回退
````
git checkout -- a.txt // a是文件名字
````
**情况2**：修改了文件，<font color="red">并提交到暂存区（即编辑之后，git add但没有 git commit -m ....）</font>
````
$ git reset HEAD    // 回退到当前版本
$ git checkout -- a.txt // a.txt为文件名
````
**情况3**：修改了文件，<font color="red">并提交到了仓库区（即编辑之后，git add且 git commit -m ....）</font>
````
$ git reset HEAD^    // 回退到上一个版本
$ git checkout -- a.txt // a.txt为文件名
````
