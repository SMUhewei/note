## 授人以鱼不如授人以渔
`git help` 显示有关`git`的帮助信息

 - 如果`git help`后面没有参数，命令行将会打印出`git命令的概要`和`最常用的git命令`。
 - 如果`git help`后面有`-all`或者`-a`参数，命令行将会打印出`所有可用的git命令`。
 - 如果`git help`后面有`-guide`或者`-g`参数，命令行将会打印出`有用的git指南`。
 - `git --help` 与 `git help` 相同，因为前者被内部转换为后者。
 - 举栗子：如果想要查看`pull命令`如何使用，则可以使用`git help pull`命令,按键盘中的`q键`可退出
    英文看着难受？没关系，[这里有一些中文版的git指令教程！](https://www.yiibai.com/git)
## git的区域划分
`git`分为三个区域，分别是工作区（`workspace`），暂存区/缓存区（`staging area`）和本地仓库区/版本库（`local repository`）
工作区：工作区就是不包括`.git`文件在内的整个项目目录
暂存区/缓存区：一般存放在`.git`目录下的`index`文件中，所以我们把暂存区也叫索引`index`.
本地仓库区/版本库：工作区的隐藏目录`.git`文件
	![git不同区域的划分](https://img-blog.csdnimg.cn/f666faf384874ca2af5478e52d99c5be.png#pic_center)
	注意点：

1. `Changes to be committed`:表示已经从工作区`add`到暂存区的`file`（文件或文件夹）。
	可以通过`git restore --staged filename` 命令将该`file`从暂存区移除，只有工作区有该文件，该文件就为`Untracked files`。
2. `Changes not staged for commit`:表示工作区，暂存区都存在的`file`，在工作区进行修改或删除，但是没有`add`到暂存区。

   通过`git add filename`命令将变更（修改，删除）的`file` ，`add`到暂存区，此刻该`file`就没有`Changes not staged for commit`状态，也就是`Changes not staged for commit`将没有改`file`的记录了。
   通过`git restore filename`的命令取消`file`在工作区的变更，那么暂存区的`file`内容还是以前的，并且`file`在`Changes not staged for commit`状态下没有记录。

3. `Untracked files`: 表示只在工作区有的`file`，也就是在暂存区没有该`file`。

## 绝对常用的git指令
**`实际应用中，git的提示命令和提示信息非常有用，一定要仔细看！`**

1. `git config` 命令用于获取并设置存储库或全局选项（一般刚下载的`git`需要配置用户名和用户邮箱，以获取代码的权限）
    配置用户名：`git config --global user.name "tom"`
    配置用户邮箱：`git config --global user.email "tom@qq.com"`
    检查所有配置：`git  config --list`
    检查用户名：`git config user.name`
    检查用户邮箱：`git config user.email`
    配置全局文本编辑器：`git config --global core.editor`
    使用`vim`作为全局文本编辑器：`git config --global core.editor vim`
2. `git init` 创建一个空的`Git`仓库或重新初始化一个现有仓库

  举个例子：如果在`/path/to/my/codebase` 目录下，输入`git init`命令，那么就会创建一个`/path/to/my/codebase/.git`目录
3. `git clone`命令将远程仓库的资源克隆到本地。
    举例子：克隆`jQuery`的版本库到本地的当前目录下`git clone http://github.com/jquery/jquery.git`
    如果想指定某个分支`git clone -b 分支名 远程仓库http地址`
    [成功拉取代码步骤](https://www.jianshu.com/p/2790a860f151)：
    1）[在官网中下载git](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)
    2）配置用户名和用户邮箱
    3）生成密钥`ssh-keygen -t rsa -C "youremail@example.com"`
    4）把`[c盘->用户->自己的用户名->.ssh]`目录下生成好的公钥"`id_rsa.pub`"文件，以文本形式打开复制并添加到版本管理仓库中
    5）`git clone`拉取代码
    [如下报错处理流程](https://blog.csdn.net/itanping/article/details/104415899)：
    ![报错图片](https://img-blog.csdnimg.cn/84afa4359e054fa5b79bd7912fc60cc7.png#pic_center)

  1）在`[c盘->用户->自己的用户名->.ssh]`目录下创建一个没有扩展名的文件`config`

  2）输入并保存以下内容

  ```
  Host *
  PubkeyAcceptedKeyTypes +ssh-rsa
  HostKeyAlgorithms +ssh-rsa
  ```

3. `git pull`命令将远程仓库的资源拉取到本地。

  常用的用法，切换到主分支，然后`git pull`就可以了，此命令相当于本地主分支的资源与远程的仓库资源做了同步。
  ```git pull --rebase```切换基底
  ```git pull = git fetch + git merge```
  ```git pull --rebase = git fetch + git rebase```
4. `git fetch` 从远程仓库中下载资源，且和`pull`不一样，`pull`会合并资源，`fetch`不会。
5. `git status` 查看当前项目中源代码的状态

  在提交代码时，一定输入`git status`命令，查看所在分支是否正确，所在分支代码是否与远端仓库保持一致

  ```shell
  PS E:\web\project> git status
  On branch develop
  Your branch is up to date with 'origin/develop'.
  
  nothing to commit, working tree clean
  ```

  `git status -s`或`git stauts --short`以精简的方式显示文件状态
  `git status --ignored` 列出被忽略的文件
6. `git add `把所有的文件都提交到暂存区（提交到暂存区的所有文件都变成绿色的了）

    `git add .`可以将所有修改过和新增的文件提交到暂存区中

    `git add 某个文件的路径`可以将某个文件单独添加到暂存区，也可以是多个，如`git add foo.txt bar.txt`。
7. `git rm`用于删除文件
    如果只是简单地从工作目录中手工删除文件，运行`git status`时就会在`changes not staged for commit`的提示
    `git rm <file>` 将文件从工作区和暂存区删除
    `git rm -f <file>`强行将文件从工作区和暂存区中删除，如果删除之前修改过并且已经放到暂存区的话，则必须要用强制删除选项`-f`
    `git rm --cached <file>`将文件从暂存区中移除，但仍然希望保留在当前工作目录中，使用`--cached`选项即可
8. `git commit ` 将暂存区中的内容添加到本地仓库中
    `git commit -m [message]`，其中`[message]`可以是一些备注信息。举个例子`git commit -m "fix: the error of license_pid has been feixed"`
    `git commit -a`，`-a`参数设置修改文件后不需要执行`git add`命令，直接提交
    `git commit --amend`增补提交，会使用与当前提交节点相同的父节点进行一次新的提交，旧的提交将会被取消
    如果`git commit`没有设置`-m`选项，`Git`会尝试为你打开一个编辑器以填写提交信息。如果`Git`在你的配置中找不到相关信息，会默认打开`vim`编辑器。
    1）输入`i`进入编辑文本模式
    2）输入需要提交的信息
    3）按`Esc`键退出文本编辑模式，进入命令模式
    4）输入`:wq`保存文本并退出
9. `git branch` 查看本地的分支，其中`*`号的分支为当前所在分支

    `git branch -a`查看本地分支和远程分支

    如果在本地创建新分支，没有和远程分支进行关联，那么`git pull`的时候就会报错，执行以下代码，使得本地分支和远程分支进行关联

    `git branch --set-upstream-to=origin/<remote_branch> <local_branch>`

    执行完上面的命令后会有如下提示

    ```
    branch '<branch_name>' set up to track 'origin/<branch_name>'
    ```

    如果开始的时候，执行的是，拉去某个远程分支，然后再切换到本地的该分支，那么就不会出现，本地分支和远程分支需要关联的问题了

    ```shell
    git pull <remote> <remote_branch>
    #拉取远端的develop分支
    git pull origin develop
    ```
10. `git checkout `切换分支
    `git checkout -b 分支名` 新建一个分支并切换到该分支。
    切换的分支本地不存在，但是远程存在，如何关联本地和远程分支？
    举例子：可以使用这样的命令`git checkout -b dev origin/dev`，这个例子的意思是说，切换到`dev`分支上，接着跟远程的`origin`地址上的`dev`分支关联起来，这里要注意`origin`代表是一个路径，可以用`git remote -v` 查看，说白了，`origin/dev`有点像是`git@github.comm:xxx/yyy.git/dev`
11. `git merge ` 将某个分支合并到当前所在分支

     举例子：将`develop`分支合并到`release`分支，首先将分支切换到`release`分支，然后`git pull`获取最新的`release`分支的代码，最后将`develop`分支合并到`release`分支`git merge develop`

     `git merge --no-ff`这个选项用于禁用快进合并`fast-forward merge`。默认情况下，如果目标分支可以直接移动到源分支的最新提交，`Git`会使用快进合并，即直接将目标分支指向源分支的提交，而不会创建新的合并提交。使用`--no-ff`选项可以强制创建一个新的合并提交，即使快进合并也是可行的。这样可以保留分支历史，更清晰地表示分支的合并情况。

     举例子：假设有一个特性分支`feature`和主分支`master`，`feature`分支已经完成开发，可以直接移动到`master`分支的最新提交。使用`--no-ff`选项进行合并时，将创建一个新的合并提交，表示`feature`分支的更改已经被合并到`master`分支。

     `git merge --squash`是用来把一些不必要的`commit`进行压缩
     举例子：假设有一个特性分支`feature`，其中有3个提交。使用`--squash`选项进行合并时，将创建一个新的合并提交，其中包含`feature`分支的所有更改。这个合并提交可以添加到目标分支上，但不会保留原始分支的提交历史。
12. `git rebase` 命令在另一个分支基础之上重新应用，用于把一个分支的修改合并到当前分支
     这样如果代码冲突的话，就会在本地做`git commit`操作时报错，如果不做`git rebase`操作的话，那么就会在将本地分支推向远程仓库的时候报错，这时候报错，错误更难处理。
     1）所以建议在将修改好的本地子分支推向远程仓库之前，先从本地子分支切换到本地主分支		
     2）在本地主分支做一次`git pull`操作，更新本地的主分支代码
     3）然后再从本地主分支切换到本地子分支中，在子分支中执行`git rebase`操作
     4）最后，再做`git commit`的操作
13. `git push <远程主机名> <本地分支名>:<远程分支名>`  将本地分支的更新，推送到远程仓库中。
     举例子：`git push origin login` 将本地的`login分支`推送到`origin`主机的`login分支`，如果`login分支`不存在，则会被新建。
     举例子：`git push origin head:refs/for/xxx`，其中`xxx`为远程分支名，`head`是一个特别的指针，它是一个指向你正在工作的本地分支的指针，可以把它当做本地分支的别名，git这样就可以知道你工作在哪个分支，`refs/for`的意义在于我们提交代码到服务器之后是需要经过`code review`之后才能进行`merge`的，而`refs/heads`不需要经过`code review`就可以进行`merge`，且这个不是`git`的规则而是`gerrit`的规则。
     `git push`报错：`missing Change-Id in commit message footer`解决步骤
     1）使用`gerrit`后，提交代码后出现如下图的报错
     ![使用gitpush报错](https://img-blog.csdnimg.cn/7b9c21c8694c421cba59a8b02747858e.png#pic_center)

     2）根据报错提示输入以下命令，其中`xxx@xxx`为自己的邮箱。如果没有报错则调到5）。
     `
     gitdir=$(git rev-parse --git-dir); scp -p -P 29418 xxx@xxx:hooks/commit-msg ${gitdir}/hooks/
     `
     3）如果报了如下图的错误
     ![在这里插入图片描述](https://img-blog.csdnimg.cn/697c189accf0435ca38fe5be36dcbca4.png#pic_center)
     4）则输入以下命令
     `
     gitdir=$(git rev-parse --git-dir); scp -O -P 29418 xxx@xxx:hooks/commit-msg ${gitdir}/hooks/
     `
     5）输入`git commit --amend`，可重新编辑提交信息，若没有可重新提交信息则保存退出即可。
     ![在这里插入图片描述](https://img-blog.csdnimg.cn/8b8975d8afbc4dc099ffed4972d6e0ad.png#pic_center)
     6)再次推送代码即可`git push origin head:refs/for/xxx`


14. `git diff` 可以通过`diff`命令来显示文件被修改的行。如`git diff foo.txt`为查看`foo.txt`文件中被修改的行。
15. `git log` 用于显示提交的日志信息
	`git reflog` 用于显示所有的操作记录（常用于恢复本地的错误操作）
	`git log --oneline` 用一行的简洁形式展示提交历史记录
	`git log --oneline --graph` 可以快速浏览提交历史记录，并且可以通过分支图，直观的了解分支的结构和合并情况（仅展示当前分支）
	`git log --oneline --graph --all` 可视化图（展示所有分支）
	![git log 和 git reflog的区别](https://img-blog.csdnimg.cn/c33302c5c8654f4b89e6d5b8bc7b5709.png)

16. `git remote` 用于管理远程仓库
    `git remote`列出已存在的远程仓库
    `git remote -v`列出已存在的远程仓库详细信息，在每一个名字后面列出其远程`url`
    `git remote add [shortname] [url]`要添加一个新的远程仓库,可以指定一个简单的名字,以便将来引用 

    ```shell
    #将一个项目同时与gitee和github关联起来
    git remote add gitee <gitee-repository-url>
    git remote add github <github-repository-url>
    #上面的命令中，gitee和github是云端仓库地址的简写
    #其中gitee和github的地址需要登录gitee和github去获取
    #复制地址的时候需要复制那个git地址而不是http的地址
    #提交代码
    git push gitee
    git push github
    #向github远端仓库,推送main分支
    git push github main
    #github默认的主分支是main，gitee默认的主分支是mast
    ```

18. `git stash` 会把所有未提交的修改都保存起来，用于后续恢复当前工作目录。
	实际应用中推荐给每个`stash`加一个`message`，用于记录版本，使用`git stash save "message"`取代`git stash`命令
	`git stash push` 保存当前工作目录的修改到`stash`中，保存指定文件的修改到stash中，如`git stash push file1.txt file2.txt`
	`git stash list`: 查看stash里面储存了哪些文件
	`git stash pop`: 命令恢复之前缓存的工作目录，将缓存堆栈中对应stash删除，并将对应修改应用到当前的工作目录下。
	`git stash apply n`： 和`git stash pop`的效果类似，但不会将缓存栈中对应stash删除,`n` 为`git stash list` 结果里面的序号 `0、1、2、3`
	`git stash clear`: 删除所有缓存的`stash`
	举例子：当你对文件做了些修改，但是现在你需要去云端`git pull`最新的分支。这个时候，如果本地更改与云端仓库的更改不冲突，则`git pull`没有问题。但是有些情况下，本地更改与云端仓库中的修改相冲突，`git pull`拒绝覆盖你的更改。这种情况下，你可以将更改隐藏起来，执行`git pull`,然后解压缩。
	具体操作，可以依次执行
	1）`git stash` 执行该命令，将当前修改的文件保存起来
			注：执行之前最好先`git add .`一下，确保所有文件都能存到`git stash`中
	2）`git pull 或  git pull --rebase(慎用）`从云端重新拉代码
	3）`git stash apply 或 git stash pop（慎用）`将之前保存的文件从stash中取出
			注：如果遇到不想提交的文件，可以执行`git reset <文件名>`命令，将文件退回stash中
19. `git restore`命令使得在工作空间但是不在暂存区的文件撤销更改（内容恢复到没有修改之前的状态）
    `git restore --staged`命令使得暂存区的文件从暂存区撤出，但不会更改文件的内容
20. `git reset [--mixed | --soft | --hard] [HEAD | HASH值]` 命令用于回退版本，可以指定退回某一次提交的版本
	其中:
	`HEAD`表示当前版本，`HEAD^`上一个版本，`HEAD^^`上一个版本...以此类推
	`HEAD`表示当前版本，`HEAD^1`上一个版本，`HEAD^2`上一个版本...以此类推
	`HEAD~0`表示当前版本，`HEAD~1`上一个版本，`HEAD~2`上一个版本...以此类推
	`HASH值`为每次`commit`时的`唯一HASH值`，该`HASH值`可以通过`git log`和个`git reflog`命令获得
	`git reset HEAD^`所有内容回退到上一个版本
	`git reset HEAD^ hello.vue`文件`hello.vue`回退到上一个版本
	`git reset 版本号` 回退到指定版本
	`git reset --hard 版本号` 硬回退到指定版本
	`git reset --mixed  HEAD`其中`--mixed`参数为默认参数，用于重置暂存区的文件与上一次`commit`保持一致，工作区文件内容保持不变
	`git reset --soft HEAD`其中--soft参数用于回退到某个版本，`git reset --soft HEAD~3`回退到上上上个版本
	`git reset --hard HEAD`其中`--hard`参数撤销工作区中所有未提交的修改内容，将暂存区与工作区都回到上一次版本，并删除之前的所有信息提交
	
	```shell
	git reset HEAD^             #代码在commit之后失败了，可以回退到之前节点，重新提交
	
	git status                  #1.查看工作区状态，发现所在分支有分叉
	git log --oneline --graph   #2.图像化展示提交记录
	git reset --soft 16539cd    #3.软回退到版本提交之前
	git status                  #4.查看状态，发现所在分支还是有分叉
	git stash                   #5.将所有桌面信息提交到stash中
	git status                  #6.查看分支状态
	git reset --hard HEAD~6     #7.硬回退到6个版本之前
	git pull                    #8.拉一下代码 发现还是没有拉取到最新的代码
	git reset --hard HEAD~20    #9.回退的再狠一点，硬回退到20个版本之前
	git pull                    #10.再次拉去一下代码
	git status                  #11.查看一下状态，发现现在本地代码和远端代码保持一致了
	```

20. `git clean` 命令用来从你的工作目录中删除所有没有track过的文件
    `git reset --hard`和`git clean`一起使用，放弃所有本地修改

## git分支命名规范

`git` 分支分为集成分支、功能分支和修复分支，分别命名为 `develop`、`feature` 和 `hotfix`，均为单数。不可使用 `features`、`future`、`hotfixes`、`hotfixs` 等错误名称。	
举个例子：增加一个`icons`的功能分支，我们可以把它命名为`feature/icons`

## husky的commit提交规范
husky的类型：
`feat` : 新增了功能
`fix`: 修复了BUG 
`docs`: 仅仅修改了文档
`style`: 修改了空格、格式缩进、逗号等，不改变代码逻辑
`perf`: 优化相关，比如提升性能、体验
`test`: 测试用例，比如单元测试、集成测试等
`chore`: 改变构建流程、或者增加依赖库、工具等
`revert`: 回滚到上一个版本
husky的格式： `husky类型加冒号加空格`
举个例子：`git commit "feat: icons have been added"`