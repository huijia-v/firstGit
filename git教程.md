### git优势

<a href="https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%85%B3%E4%BA%8E%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6">参考文档</a>

- 直接记录快照，而非差异比较

- 近乎所有操作都是本地执行

- Git 保证完整性

  Git 用以计算校验和的机制叫做 SHA-1 散列（hash，哈希）。 这是一个由 
  40 个十六进制字符（0-9 和 a-f）组成的字符串，基于 Git 中文件的内容或 目录结构计算出来

- Git 一般只添加数据

### git的三个区域
总结如下：

- 工作区：修改文件的地方，使用`git add`命令可以添加到暂存区
- 暂存区：暂存文件，将文件的快照放入暂存区域。使用`git commit`命令提交到本地库
- Git本地库：找到暂存区域的文件，将快照永久性存储到 `Git` 仓库目录。

### 创建版本库
什么是版本库？版本库又名仓库，英文名***repository***,你可以简单的理解一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改，删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻还可以将文件”还原”。

使用git打开创建的文件夹gitTest.  使用命令`git init`
![init](/upload/2021/08/init-48a40acb50854c08be373b309a151317.png)
### 初次使用
```bash
git add 文件名  #添加或更新文件到缓存区
git commit -m "提交信息注释" 文件名  #提交代码到本地仓库
```
##### 相关命令
<span style="color:red">`git status`</span> 当前 `git`仓库的状态
使用`git status -s`命令或<span style="color:red">`git status --short`</span>命令,可以得到更文紧凑的格式输出
<span style="color:red">`git rm --cached 文件名`</span>从缓存区中移除文件
<span style="color:red">`git checkout`</span>工作空间中修改了文件想要还原到之前的状态可以使用该命令
`git diff`可以查看文件修改了什么
##### 如何跳过使用暂存区域
在提交的时候,给<span style="color:red">`git commit`</span>加上<span style="color:red">`-a`</span>选项,Git就会自动把所有已经跟踪过的文件暂村起来一并提交,从而跳过<span style="color:red">`git add`</span>步骤
### 版本回退
<span style="color:red">`git log`</span>显示从最近到最远的显示日志,从这个命令的结果可以看到最近的提交记录
![59c1d34e0001a1ac06050304](bytes-stream.net:8090/upload/2021/08/59c1d34e0001a1ac06050304-0339320215e442dc88cb02b14c6691cb.jfif)

<span style="color:red">`git log –pretty=oneline`</span>可以美化一下输出结果
![59c1d3fc00013ad206040097](/upload/2021/08/59c1d3fc00013ad206040097-7310600d9866422fbb45bf307b784ca2.jfif)
 <span style="color:red">`git reset --hard HEAD^`</span>那么如果要回退到上上个版本只需把`HEAD^` 改成 `HEAD^^` 以此类推。那如果要回退到前100个版本的话，使用上面的方法肯定不方便，我们可以使用下面的简便命令操作：<span style="color:red">`git reset --hard HEAD~100`</span> 
如果想要回退到最新的版本,此时就可以使用版本号进行回退,这样更加精确,可以通过<span style="color:red">`git reflog`</span>查看版本号
![59c1d51a0001d5fc05100122](/upload/2021/08/59c1d51a0001d5fc05100122-4b13fe26427b47368abb1f62fe6e7b55.jfif)
<span style="color:red">`git reset --hard 6fcfc89`</span>来恢复了。演示如下：
![59c1d53a0001b8b305050153](/upload/2021/08/59c1d53a0001b8b305050153-7f33ee7850444d6a9f562809c652b83a.jfif)
### 理解工作区与暂存区的区别
工作区：就是你在电脑上看到的目录，比如目录下testgit里的文件(.git隐藏目录版本库除外)。或者以后需要再新建的目录文件等等都属于工作区范畴。
版本库(Repository)：工作区有一个隐藏目录`.git`,这个不属于工作区，这是版本库。其中版本库里面存了很多东西，其中最重要的就是stage(暂存区)，还有Git为我们自动创建了第一个分支`master`,以及指向master的一个指针`HEAD`。

我们前面说过使用Git提交文件到版本库有两步：
第一步：是使用<span style="color:red">`git add`</span>把文件添加进去，实际上就是把文件添加到暂存区。
第二步：使用<span style="color:red">`git commit`</span>提交更改，实际上就是把暂存区的所有内容提交到当前分支上。
<span style="color:red">`git commit`</span>
![20180603202659766 (1)](http://106.14.40.112:8090/upload/2021/08/20180603202659766%20(1)-380c47f256354204a71e8dfbefe0d1ad.png)

### 撤销修改
(1) 没有 `git add` 之前
可以手动删除最后一行，手动把文件恢复到上一个版本的状态。然后再用 `git checkout -- file` 命令丢弃工作区的修改
(2) `git add`了，但没有`git commit`
这时候的修改添加到了暂存区，但没有提交到分支，用 `git status`查看一下：
这时候我们可以使用 `git reset HEAD file` 命令把把暂存区的修改撤销掉，重新放回工作区：
(3) 既 `git add` 了，也 `git commit` 了
可以回退到上一个版本，见回退版本内容。
### 删除文件
- 确实要从版本库中删除该文件，那就用 `git rm` 命令删除，并且 `git commit`：
- 文件被删错了。因为版本库里有，所以很好恢复：
`git checkout -- test.txt` //用版本库里的版本替换工作区的版本。
### 远程仓库准备
在开始这部分之前，我们需要自行注册GitHub账号。而且，因为你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以需要设置：

(1) 创建SSH Key。在用户主目录下，看看有没有.ssh 目录，如果有的话，看此目录下有没有 id_rsa 和 id_rsa.pub 这两个文件，如果有，直接跳到下一步。如果没有，打开Git Bash，创建SSH
(2)登陆GitHub，打开"Account settings"然后点击"Add SSH and GPG Keys"，再点击"New SSH Key"进行SSH Key 的创建，填上任意 Title ，把 id_rsa.pub 中的内容复制到Key文本框内：
### 创建远程仓库
现在我们已经在本地创建了一个Git仓库了，又想在GitHub上创建一个Git仓库，然后让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作。那么我们应该怎么做呢？
首先，登陆GitHub，在右上角找到“Create a new reposity”按钮，创建一个新的仓库：
```shell
git remote add origin git@github.com:huijia-v/firstGit.git
```
添加后，远程库的名字就是 `origin` ，这是Git默认的叫法。

然后，我们就可以把本地库的所有内容推送到远程库上：
```shell
$ git push -u origin master
```
使用 `git push` 命令，就是把当前分支 master 推送到远程。

因为远程库是空的，所以我们在第一次推送 master 分支时，要加上 -u 参数，Git不但会把本地的 master 分支内容推送的远程新的 master 分支，还会把本地的 master 分支和远程的 master 分支关联起来，在以后的推送或者拉取时就可以简化命令。

推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

从现在起，只要本地作了提交，就可以通过命令：
```shell
$ git push origin master
```
把本地 master 分支的最新修改推送至GitHub。现在，我们拥有了真正的分布式版本库。