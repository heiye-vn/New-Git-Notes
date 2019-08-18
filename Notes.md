![git](https://www.liaoxuefeng.com/files/attachments/918921150461184/0)

# Git学习文档

------

## 一、Git简介

### 1.git是什么？

​			git是一种分布式`版本控制系统`，由Linus使用C语言开发



### 2.什么是版本控制系统

​			**举个栗子**：我们在管理文件时有时会对文件做一些修改更新，无论是自己或者别人来更新这个文件，最后都需要在某一个时间来查看这个更新之后的文档，那么具体怎么做呢？

​	以前的样子：

![](https://www.liaoxuefeng.com/files/attachments/918921393733152/0)

​		使用git之后样子：

| 版本 |        文件名         | 用户 |        说明         |    日期     |
| :--: | :-------------------: | :--: | :-----------------: | :---------: |
|  1   |       demo.html       |  A   |    删除了xxx内容    | 5/20  08:20 |
|  2   |       demo.html       |  A   |    添加了xxx内容    | 5/20 15:14  |
|  3   | demo.html & login.php |  B   | 新增了login.php文件 |  6/1 20:05  |
|  4   |       demo.html       |  C   |    修改了xxx内容    |  6/3 12:03  |



### 3.集中式 VS 分布式

- `集中式版本控制系统`

  ![](https://www.liaoxuefeng.com/files/attachments/918921540355872/0)

  ​     **定义**：版本库是集中存放在中央式的版本控制系统，而工作的时候，都是用的自己的电脑。故要先从中央服务器取得文件的最新版本，工作完了再把文件推送给中央服务器。最大的特点就是**必须联网**才能工作，常见集中式版本控制系统：CVS、SVN等。

- `分布式版本控制系统`

  ![](https://www.liaoxuefeng.com/files/attachments/918921562236160/0)

  ​     **定义**：分布式版本控制系统没有中央服务器，每个人的电脑就是一个完整的版本库，工作时就不需要联网，能够实现多人协作，即把修改的同一文件推送给对方完成修改整合，而且不用担心文件丢失损坏。在实际工作的时候通常也有一台充当 “ 中央服务器 ”的电脑。Git就是常用的分布式版本控制系统，不仅不需要在联网条件下工作，还具备强大的分支管理。



### 4.创建版本库

1. 创建文件目录（有文件的目录也可以）

2. 将该目录变为Git可以管理的仓库（本地仓库）

   ```cmd
   $ git init
   Initialized empty Git repository in F:/Git-Study/GitCourse-Liao Xuefeng version/learnGit/.git/
   
   ```

3. 添加文件：git add  xxx.txt

   **注**：可以添加多个文件

   ```cmd
   $ git add file1.txt
   $ git add file2.txt file3.txt
   $ git commit -m "add 3 files"
   ```

4. 使用 git commit 告诉Git，把文件提交到仓库

   ```cmd
   $ git commit -m "wrote a readme.txt"
   ```

   **注**：-m 后面输入的是本次提交的说明，可以输入任意内容，但最好是见名知意的内容，方便以后自己或者别人查看



## 二、时光机穿梭

### 1.时光机穿梭

1. 当文件被修改之后

   ​     `git status`命令可以时刻查看仓库当前状态，以下命令提示文件被修改但还没准备提交的修改

   ```cmd
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git checkout -- <file>..." to discard changes in working directory)
   
           modified:   readme.txt
   
   no changes added to commit (use "git add" and/or "git commit -a")
   
   ```

2. 查看被修改的内容

   ​     `git diff`，diff 即 difference，查看当前文件与之前文件的差异，---为之前文件，+++为当前文件，这样就能够清晰的看到修改的内容

   ```cmd
   $ git diff
   diff --git a/readme.txt b/readme.txt
   index 89253dd..be836b4 100644
   --- a/readme.txt
   +++ b/readme.txt
   @@ -1,3 +1,2 @@
   -Git is a version control system
   -
   -Git is free software
   +Git is a distributed version control system
   +Git is free software
   \ No newline at end of file
   
   ```

3. 再次`git add`，查看状态

   ```cmd
   $ git add readme.txt          // 无任何提示
   $ git status
   On branch master
   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)
   
           modified:   readme.txt
   // 表示将要被提交的修改包括 readme.txt，可以进行下一步的提交
   ```

4. 再次`git commit -m "xxx"`，查看状态

   ```cmd
   $ git commit -m "add distributed"
   [master 152646f] add distributed
    1 file changed, 2 insertions(+), 3 deletions(-)
    
   $ git status
   On branch master
   nothing to commit, working tree clean
   //表示当前没有需要提交的修改，而且工作目录是干净的（working tree clean）
   ```

**小结**：1.要随时掌握工作区的状态，`git status`

​			2.若`git status`命令说明有文件被修改过，则用`git diff`查看修改的内容

​			3.重新对文件进行提交，`git add`、`git commit -m`



### 2.版本回退

​	  **注**：当我们在对某个文件进行多次修改时，使用`git commit`命令后就会存在多个版本，类似多个版本快照，如果后面修改失误可以回退到最近修改的一个版本中重新进行修改

1. 查看文件版本：`git log`

   表示显示由近至远的提交日志   `git log --pretty=oneline`：在一行显示

   ```cmd
   $ git log
   commit 5b1426c7c167ac336d77b44a45b890718d2afc72 (HEAD -> master)
   Author: heiye-vn <1064239893@qq.com>
   Date:   Sat Aug 17 10:03:44 2019 +0800
   
       append GPL
   
   commit 152646f7d64151a6f61803f4059739084a8915bf
   Author: heiye-vn <1064239893@qq.com>
   Date:   Sat Aug 17 09:39:30 2019 +0800
   
       add distributed
   
   commit aa375793744933d67e224bdf88afdc83628635b4
   Author: heiye-vn <1064239893@qq.com>
   Date:   Sat Aug 17 09:04:15 2019 +0800
   
        worte a readme file
   
   $ git log --pretty=oneline
   5b1426c7c167ac336d77b44a45b890718d2afc72 (HEAD -> master) append GPL
   152646f7d64151a6f61803f4059739084a8915bf add distributed
   aa375793744933d67e224bdf88afdc83628635b4  worte a readme file
   
   // 注：前面的5b1...fc72为commit版本号，这样不会导致版本冲突（版本控制系统的特点）	
   ```

2. 回退到上一个版本（穿梭到过去），并查看版本  `git reset --hard HEAD^`

   ​	**注**：首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

   ```cmd
   $ git reset --hard HEAD^
   HEAD is now at 152646f add distributed
   
   ZSP@zsp-admin MINGW64 /f/Git-Study/GitCourse-Liao Xuefeng version/learnGit (master)
   $ cat readme.txt
   Git is a distributed version control system
   Git is free software
   ZSP@zsp-admin MINGW64 /f/Git-Study/GitCourse-Liao Xuefeng version/learnGit (master)
   $ git log
   commit 152646f7d64151a6f61803f4059739084a8915bf (HEAD -> master)
   Author: heiye-vn <1064239893@qq.com>
   Date:   Sat Aug 17 09:39:30 2019 +0800
   
       add distributed
   
   commit aa375793744933d67e224bdf88afdc83628635b4
   Author: heiye-vn <1064239893@qq.com>
   Date:   Sat Aug 17 09:04:15 2019 +0800
   
        worte a readme file
   //此时 append GPL 版本消失不见了
   
   
   ```

3. 返回最新版本（穿梭到未来），`git reset --hard commit-id`

   commit-id 不用输完，只需输入几位数（防止出现多个版本，尽量多写几位）就行，git会自动查找

   ```cmd
   $ git reset --hard 5b1426
   HEAD is now at 5b1426c append GPL
   
   ZSP@zsp-admin MINGW64 /f/Git-Study/GitCourse-Liao Xuefeng version/learnGit (master)
   $ cat readme.txt
   Git is a distributed version control system.
   Git is free software distributed under the GPL.
   
   ```

   **注**：Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把HEAD从指向`append GPL`：

   ```html
   ┌────┐
   │HEAD│
   └────┘
      │
      └──> ○ append GPL
           │
           ○ add distributed
           │
           ○ wrote a readme file
   ```

   改为指向 add distributed：

   ```html
   ┌────┐
   │HEAD│
   └────┘
      │
      │    ○ append GPL
      │    │
      └──>	○ add distributed
           │
           ○ wrote a readme file
   
   ```

4. 版本（commit）id记录：`git reflog`	

```cmd
$ git reflog
5b1426c (HEAD -> master) HEAD@{0}: reset: moving to 5b1426
152646f HEAD@{1}: reset: moving to 152646f
152646f HEAD@{2}: reset: moving to HEAD^
5b1426c (HEAD -> master) HEAD@{3}: reset: moving to HEAD
5b1426c (HEAD -> master) HEAD@{4}: commit: append GPL
152646f HEAD@{5}: commit: add distributed
aa37579 HEAD@{6}: commit (initial): worte a readme file
```
​		**小结**：通过`git rest --hard 版本号`能够进行任意版本的回退，`git log`查看提交历史，`git reflog`能够记录提交的每个commit版本id，这样就能够真正做到随心所欲的回退到想要的版本



### 3.工作区和暂存区

​	Git 和其他版本控制系统如 SVN 的不同之处就是存在暂存区这一概念

1. 工作区（Working Directory）

   **注**：即在电脑中能看到的目录，如：learnGit就是一个工作区

   ![](F:\Git-Study\GitCourse-Liao Xuefeng version\images/1.png)

2. 版本库（Repository）

   **注**：工作区中的 .git 目录不算在工作区中，而是Git版本库

   ​		Git的版本库里存了很多东西，其中最重要的就是称为stage（index）的暂存区，还有Git自动创建的第一个分支master，以及指向master的一个指针 HEAD

   ![](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

   ​		前面提到的文件提交中`git add`实际上就是把文件修改添加到暂存区，`git commit`实际上就是把暂存区的所有内容提交到当前分支，即所有需要提交的文件开始都需要放到暂存区，然后一次性提交暂存区的所有修改到master分支上

   **举栗子**：修改readme.txt文件并新建一个Hello-World.txt文本，并添加到暂存区

   ```cmd
   $ git add readme.txt Hello-World.txt
   
   ZSP@zsp-admin MINGW64 /f/Git-Study/GitCourse-Liao Xuefeng version/learnGit (master)
   $ git status
   On branch master
   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)
   
           new file:   Hello-World.txt
           modified:   readme.txt
   
   ```

   此时暂存区的状态图示：

   ![暂存区](F:\Git-Study\GitCourse-Liao Xuefeng version\images/2.png)

提交之后且未对工作区的文件再做修改，此时暂存区的状态：

![](F:\Git-Study\GitCourse-Liao Xuefeng version\images/3.png)

**总结**：暂存区是Git中很重要的概念，了解的暂存区的工作原理，就知道了 git 中很多操作的具体作用



### 4.管理修改

​	Git 相较于其他版本控制系统不同的是：Git跟踪并管理的是修改而非文件，修改即为对文件进行增删改等操作

​	**举个栗子**：对readme.txt进行修改 —> 添加`git add`，查看状态`git status` —> 再对readme.txt进行修改 —>提交`git commit -m "xxx"`，查看状态`git status`，出现问题（第二次修改没有被提交）

​	**小结**：提交修改有两种方式：

   1. 第一次修改 —> `git add` —> `git commit` —> 第二次修改 —> `git add`—> `git commit` ......

   2.  第一次修改 —> `git add` —> 第二次修改 —>`git add` ...... —> `git commit`

      故：每次在修改文件之后一定要进行添加，否则不会被提交到暂存区



### 5.撤销修改

​	  我们在对文件做修改时，有时需要撤销修改`git checkout -- file`，恢复到最近提交的版本，注：-- 参数很重要，没有的话就变成了切换到另一个分支的命令。总之，撤销修改就是让文件回到最近一次`git add`或`git commit`时的状态，**撤销修改的条件是在工作区或者暂存区或者版本库，无法撤销推送到远程仓库（GitHub）的修改**

​	`git checkout -- readme.txt`表示把readme.txt在工作区的修改全部撤销，存在两种情况：

  1. readme.txt 修改后还未添加到暂存区，此时撤销就回到和版本库一模一样的状态

     ```cmd
     $ cat readme.txt
     Git is a distributed version control system.
     Git is free software distributed under the GPL.
     Git has a mutable index called stage.
     Git tracks changes.
     Git tracks change of files.
     My stupid boss still prefers SVN.
     // 需要撤销第7行的修改
     $ git checkout -- readme.txt
     
     $ cat readme.txt
     Git is a distributed version control system.
     Git is free software distributed under the GPL.
     Git has a mutable index called stage.
     Git tracks changes.
     Git tracks change of files.
     ```

  2. readme.txt 已经添加`git add`到暂存区后，又做了修改，测试撤销修改就回到添加到暂存区后的状态

     `git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本

```cmd
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
Git tracks change of files.
My stupid boss still prefers SVN.

$ git add readme.txt

$ git reset HEAD readme.txt
Unstaged changes after reset:
M       readme.txt

$ git checkout -- readme.txt

$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes
.
Git tracks change of files.

```

**小结**：撤销修改三种情况：

1. 当错误修改了**工作区**某个文件的内容，想要撤销修改时，使用 `git checkout -- file` 命令。
2. 当错误修改了**工作区**某个文件的内容，并且还提交到了**暂存区**，撤销修改：先使用 `git reset HEAD <file>` 命令，就回到了第一种情况，按照第一种情况操作。
3. 当某个文件的错误修改被添加且提交到了版本库时，撤销本次修改：参考版本回退（前提是没有推送到远程仓库）



### 6.删除文件

​	在 Git 中，删除文件也是一种修改

1. 新文件添加并提交到了版本库进行删除（确定要删除），

   ```cmd
   $ rm test.txt				// 只是删除了工作区的文件，此时工作区和版本库就不一致
   
   $ git rm test.txt			// 使用 git命令删除
   rm 'test.txt'
   
   $ git commit -m "remove test.txt"	// 再进行commit提交，就彻底将工作区和版本库的文件删除
   [master 334cbda] remove test.txt
    1 file changed, 1 deletion(-)
    delete mode 100644 test.txt
   
   ```

2. 新文件添加并提交到了版本库进行删除（文件删错了，从版本库中恢复）

   ```cmd
   $ git checkout -- test.txt
   
   $ ls
   Hello-World.txt  readme.txt  test.txt
   ```

   ​      `git checkout` 是利用版本库里的版本替换工作区的版本，无论对工作区的文件进行修改还是删除都能够还原



## 三、远程仓库

### 1.概念及创建SSH key

1. 定义：Git 是分布式版本控制系统，同一个Git 仓库，可以分布到不同的机器上，即一台主机上一个原始版本库，其他主机可以“克隆”这个原始版本库，而且每个主机上的版本库都是一样的，我们可以自己搭建一台运行Git的服务器，或者使用GitHub提供远程仓库托管服务

   

2. 创建 SSH Key（密钥对）

   1. 在用户目录下查看是否有.ssh目录，再进入查看是否存在id_rsa 和 id_rsa.pub 两个文件，如果存在则忽略这一步，没有的话打开Shell（Windows下打开 Gti Bash），创建SSH Key

      ```cmd
      $ ssh-keygen -t rsa -C "邮箱地址"
      ```

   2. 打开GitHub的setting设置界面，进入SSH and GPG keys选项，点击 Add SSH Key，设置任意title，把id_rsa.pub里面的文本粘贴到key文本框里，最后点击添加key

      

3. 其他远程仓库知识

   1. 创建SSH Key的意义：因为GitHub需要识别出你推送的提交确实是本人推送的，并且Git支持SSH协议，所以GitHub只要知道你的公钥，就可以确认只有本人才能够推送
   2. 创建多个SSH Key：GitHub允许添加多个key，考虑到了一个人可能会在不同的电脑上进行工作，只要把每台电脑的key添加到GitHub上，就能够向GitHub上推送
   3. **公有仓库（Public）**和**私有仓库（Private）**：在GitHub上创建远程仓库分为公有仓库和私有仓库，公有仓库即所有人都能够看到提交的内容（但只有自己才能修改），最好别把敏感信息加进去；私有仓库即别人查看不到提交的内容，创建私有仓库的方法：一是支付一定的费用来创建，二是自己搭建一个git服务器，只为自己提供服务（公司内部开发必备）

​	

### 2.添加远程仓库

1. 在GitHub个人界面点击 `+` 里面的New repository来创建一个新的仓库

2. 本地仓库关联远程仓库

   ```cmd
   $ git remote add origin git@github.com:heiye-vn/learnGit.git
   或者
   $ git remote add origin https://github.com/heiye-vn/233.git
   ```

   ​		两种命令都能够关联远程仓库，只是方式不同：一种是HTTPS协议方式，另一种是SSH协议方式，其中origin参数表示为一个远程仓库

   ​		第三种关联方式是通过GitHub Desktop 桌面APP进行操作（了解）

3. 把本地仓库的内容推送到远程仓库

   ```cmd
   $ git push -u origin master
   Enumerating objects: 23, done.
   Counting objects: 100% (23/23), done.
   Delta compression using up to 4 threads
   Compressing objects: 100% (17/17), done.
   Writing objects: 100% (23/23), 1.93 KiB | 58.00 KiB/s, done.
   Total 23 (delta 4), reused 0 (delta 0)
   remote: Resolving deltas: 100% (4/4), done.
   To github.com:heiye-vn/learnGit.git
    * [new branch]      master -> master
   Branch 'master' set up to track remote branch 'master' from 'origin'.
   ```

   ​	    **注**：把本地仓库内容推送到远程仓库命令：`git push`，实际上是把当前分支`master`推送到远程，因为是第一次推送`master`分支，故需要添加 -u 参数。Git不但会把本地的master推送到远程新的master分支，还会把二者关联起来，这样在后面进行推送或者拉取时就能够简化命令。**以后只要本地做了提交**，就能够通过命令`git push origin master`把本地master的最新修改推送到GitHub

4. SSH警告

   ​		当第一次使用 Git 的`clone`或者`push`命令连接GitHub时，会出现一个警告

   ```cmd
   The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
   RSA key fingerprint is xx.xx.xx.xx.xx.
   Are you sure you want to continue connecting (yes/no)?
   ```

   ​		这是因为 Git 使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub服务器，输入yes回车即可

   **小结**：1.关联远程仓库：`git remote add origin git@server-name:path/repository-name.git`或者				`git remote add origin https://server-name/path/repository-name.git`

   ​			2.关联后使用`git push -u origin master`第一次推送master分支的所有内容

   ​			3.以后，每次在本地提交后，只要有必要就可以使用`git push origin master`推送最新修改







## 七、Git Bash 常用操作文件命令

### 1.Git Bash 下操作文件及文件夹命令

- `cd`：切换到哪个目录下，如 cd d:/xxx  切换到 D盘下的xxx目录，当我们用cd进入文件夹时，可以使用通配符*， cd f *，如果E盘下只有一个f开头的文件夹，就会进入到这个文件夹
- `cd..`：回退到上一个目录，注：cd和两个点之间有一个空格
- `pwd`：显示当前目录路径
- `ls（ll、dir）`：列出当前目录中的所有文件，只不过ll列出的内容更加详细
- `rm`：删除一个文件，如 rm index.js 就会把index.js文件删除
- `mkdir`：新建一个文件夹（目录），如 mkdir src  就会新建一个 src 目录
- `rm -r`：删除一个文件夹，如 rm -r src  就会把 src 文件删除
- `mv`：移动文件，mv index.js src ，index.js是要移动的文件，src是目标文件夹，这样写必须要保证文件和目标文件在同一目录下
- `reset（clear）`：把 git bash 命令窗口中内容清空
- `写文件（window中）`：echo xxx > readme.txt  （有内容的话会覆盖）、echo xxx >> readme.txt（追加到最后一行）
- `cat xxx.txt`：查看 xxx.txt 文件内容

