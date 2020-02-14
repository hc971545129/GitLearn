# Git Learn
#### 初始化/创建仓库 `git init`
#### 添加(到暂存区) `git add`
#### 提交(到版本库) `git commit -m "注释"`
#### 查看仓库当前状态(包括修改记录和待提交项目) `git status`	
#### 查看修改内容difference `git diff`		
#### 查看提交记录 `git log`			
```
$ git log
commit 29971f9f31f728dd16e5984aa584a72b64079067 (HEAD -> master)
Author: hc18220829434 <hc971545129>
Date:   Thu Feb 6 18:39:21 2020 +0800

    add stage

commit 48355fd105e67a6a959b4886ed5d01adef5fc68a
Author: hc18220829434 <hc971545129>
Date:   Thu Feb 6 18:37:46 2020 +0800

    add LICENSE

commit 7e868f2e99fc547f5407545a720c7b0472c94f70
Author: hc18220829434 <hc971545129>
Date:   Thu Feb 6 17:55:35 2020 +0800

    append GPL

commit caf216bdb81e62a658cda69a4fc82d9dcf2ebc9d
Author: hc18220829434 <hc971545129>
Date:   Thu Feb 6 17:53:15 2020 +0800

    add distributed

commit 8c0663a8d1ae412b8dcff5647cba8bc1c9740547
Author: hc18220829434 <hc971545129>
Date:   Thu Feb 6 17:41:53 2020 +0800

    wrote a readme file
```
+ `HEAD` 表示当前版本
+ `HEAD^` 表示上个版本
+ `HEAD^^` 表示上上个版本
+ `HEAD~100` 表示第前100个版本
#### 回退版本或把暂存区的修改回退到工作区(当用HEAD时，表示最新的版本。) `git reset`
```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```
`Git` 的版本回退速度非常快，因为 `Git` 在内部有个指向当前版本的 `HEAD` 指针，当你回退版本的时候，`Git` 仅仅是把 `HEAD` 从指向 `append GPL`：
```
由->append GPL 改为 ->add distributed：
┌────┐                        ┌────┐
│HEAD│                        │HEAD│
└────┘                        └────┘
   │                            │
   └──> ○ append GPL            │    ○ append GPL
        │                       │    │
        ○ add distributed       └──> ○ add distributed
        │                            │
        ○ wrote a readme file        ○ wrote a readme file
```
#### 查看历史命令 以便确定要回到未来的哪个版本。 `git reflog`		
```
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

#### 版本库
`Git` 的版本库里存了很多东西，其中最重要的就是称为 `stage`（或者叫 `index` ）的暂存区，还有 `Git` 为我们自动创建的第一个分支 `master`，以及指向 `master` 的一个指针叫 `HEAD` 。

![版本库](https://www.liaoxuefeng.com/files/attachments/919020037470528/0 "工作区 `learngit` 与版本库 `.git` ")

把文件往 `Git` 版本库里添加的时候，是分两步执行的：

+ 第一步是用 `git add` 把文件添加进去，实际上就是把文件修改添加到暂存区；

+ 第二步是用 `git commit` 提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建 `Git` 版本库时，`Git` 自动为我们创建了唯一一个 `master` 分支，所以，现在，`git commit` 就是往 `master` 分支上提交更改。

#### 撤销修改
+ 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令 `git checkout -- file` 。

+ 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令 `git reset HEAD <file>` ，就回到了场景1，第二步按场景1操作。

+ 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

#### 删除文件
+ 确定删除 `git rm <filename>`
+ 误删恢复 `git checkout -- <filename>`
+ 注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！

#### 远程仓库 Github
由于你的本地 `Git` 仓库和 `GitHub` 仓库之间的传输是通过 `SSH` 加密的，所以，需要一点设置：
+ 第1步：创建 `SSH Key` 。在 `用户主目录~` 下，看看有没有 `.ssh` 目录，如果有，再看看这个目录下有没有 `id_rsa` 和 `id_rsa.pub` 这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开 `Shell` ，创建 `SSH Key`：
    ```
    $ ssh-keygen -t rsa -C "myemail@example.com"
    ```
    一路回车
    如果一切顺利的话，可以在 `用户主目录~` 里找到 `.ssh` 目录，里面有 `id_rsa` 和 `id_rsa.pub` 两个文件，这两个就是 `SSH Key` 的秘钥对，`id_rsa` 是私钥，不能泄露出去，`id_rsa.pub` 是公钥，可以放心地告诉任何人。
+ 第2步：登陆 `GitHub`，打开`Settings”`，`SSH Keys`页面，点`“Add SSH Key”`，填上任意`Title`，在 `Key` 文本框里粘贴 `id_rsa.pub` 文件的内容。

### 添加远程库
+ 在本地 `Git` 仓库中
    `git remote add origin git@github.com:hc971545129/learngit.git`
    添加后，远程库的名字就是 `origin`，这是 `Git` 默认的叫法，也可以改成别的，但是 `origin` 这个名字一看就知道是远程库。
+ 把本地库的所有内容推送到远程库上
    `git push -u origin master`
    从现在起，只要本地作了提交，就可以通过命令
    `$ git push origin master`
    把本地master分支的最新修改推送至GitHub

### 从远程库克隆
+ 先创建一个远程库，然后
` git clone git@github.com:hc971545129/GitLearn.git
`
+ `Git` 支持多种协议，包括 `https` ，但通过 `ssh` 支持的原生 `git` 协议速度最快。