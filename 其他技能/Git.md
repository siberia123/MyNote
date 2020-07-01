# 版本控制工具的演变

1. 集中式VCS

   <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200528185145786.png" alt="image-20200528185145786" style="zoom: 67%;" />

1. 分布式VCS

   <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200528185253208.png" alt="image-20200528185253208" style="zoom:67%;" />





# 配置Git信息

## 配置基本的用户user信息

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200528190543993.png" alt="image-20200528190543993" style="zoom:50%;" />

通过这个配置，每次进行commit时，控制系统都知道提交者是谁以及知道他的邮箱。如果提交的版本有问题，就可以通过邮件联系。



## config的作用域

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200528191110226.png" alt="image-20200528191110226" style="zoom: 50%;" />



## 查看config配置

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200528191239508.png" alt="image-20200528191239508" style="zoom: 50%;" />

```cmd
git config --list --global

通过这个命令就可以看到配置global信息的有哪些（在本地上显示如下）
user.name='siberia'
user.email='siberiacurrent@gmail.com'
```



## 清除config配置

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200528192417881.png" alt="image-20200528192417881" style="zoom:50%;" />



## 作用域的优先级

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200529110718217.png" alt="image-20200529110718217" style="zoom:50%;" />





# 常用Git命令

## 建立Git仓库

建立Git仓库一般有两种方式：

- 将已有的项目添加到Git仓库中

  <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200529111612641.png" alt="image-20200529111612641" style="zoom:50%;" />

- 直接用Git创建一个新的仓库

  <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200529111733210.png" alt="image-20200529111733210" style="zoom:50%;" />

## 向仓库提交

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200529114845968.png" alt="image-20200529114845968" style="zoom:50%;" />

1. `git add 文件或者文件名`：添加文件或者文件夹到stage区。

   如果文件修改过，也需要使用`git add xxx`命令将其重新追踪。

   多个之前已追踪过的文件发生修改，可以用`git add -u`将所有已修改过的文件进行提交到暂存区。

2. `git status`：可以查看工作区和暂存区的状态（哪些没有存到暂存区，哪些没有提交）。

3. `git commit `：向仓库提交（将stage区的所有内容提交到仓库）。

   * `--amend`：对最新一次的提交备注进行变更。（如果commit后发现备注不是很准确就可以使用这个命令）

4. `git rebase -i 分支哈希值`

   * r：对历史一次的提交备注进行变更。

   <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200530011522015.png" alt="image-20200530011522015" style="zoom:80%;" />

   ​			如果要对某一个commit的注释进行修改，就要选择该commit的上一次commit的哈希值。

   ![image-20200530011801587](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200530011801587.png)

   ![image-20200530011910213](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200530011910213.png)

   * s：对多个连续commit进行合并。

     <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200530014203754.png" alt="image-20200530014203754" style="zoom:80%;" />

     ![image-20200530014509292](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200530014509292.png)

   * 对间隔的commit进行合并：

     ![image-20200530112225895](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200530112225895.png)

     ![image-20200530111635203](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200530111635203.png)

     （注意这些哈希值的顺序，第一个一定要是pick，否则会报错）

     

5. `git log`：查看**当前分支**所有commit的提交记录。

   <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200529225023759.png" alt="image-20200529225023759" style="zoom: 80%;" />

   log后面可以附加的条件：

   * `--oneline`：只显示提交时的备注。

   * `-n2`：显示最近的2次commit，-n3：显示最近的3次commit，-n4,-n6...

   * `--all --graph`：查看**所有分支**commit的提交记录，以图形化的方式展示。

     <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200529231705397.png" alt="image-20200529231705397" style="zoom: 80%;" />



## git命令

* `git branch -av`：查看本地有多少分支。

* `git branch -D 分支哈希值/分支名`：强制删除分支。

* `git checkout -b 新分支的别名 分支的哈希值`：创建分支，此时head指向当前分支。（工作目录就会回到以前的状态）

  ![image-20200529230202349](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200529230202349.png)

* `git checkout 分支名`：切换分支。（head会指向切换后的分支）

* `gitk`：Git工具可视化界面。

* `git help 指令名称`：查看该指令的使用方法。

* `git cat-file -t 节点哈希值`：查看节点的类型。

* `git cat-file -p 节点哈希值`：查看节点保存的内容。

  <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200529234157870.png" alt="image-20200529234157870"  />

* `git mv 原文件名 更改后的文件名`：将暂存区的和工作区的文件重命名。之后可以直接commit，不需要用add来追踪。

* `git diff 哈希值 哈希值`：用于版本的比较。也可以用HEAD来指代，`git diff HEAD HEAD~2`。HEAD~2表示头指针爷爷的版本。后面也可以追加参数，来指定查看特定文件的版本差异。比如（`git diff master temp -- index.html`）

  * `--cached（git diff --cached）`：暂存区与HEAD所指区域的版本比较。

    <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200531115430459.png" alt="image-20200531115430459" style="zoom:50%;" />

  * 不带参数（`git diff`）：工作区与暂存区的版本比较。（也可以在后面指定文件名，git diff -- t1.txt：就是比较工作区中t1.txt与暂存区中t1.txt版本的差异）

    <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200531115802231.png" alt="image-20200531115802231" style="zoom:50%;" />

* `git reset HEAD`：将暂存区恢复到与HEAD所指区域版本。

  * `-- 文件名1 文件名2`：可以将暂存区的文件1，文件2恢复到与HEAD所指版本。（取消暂存区的部分变更）

* `git reset --hard 节点哈希值`：消除最近几次提交,将暂存区与工作区恢复到与HEAD所指版本。（这里的节点哈希是你想要返回到的commit节点）

  ![image-20200531143148458](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200531143148458.png)  	                                ![image-20200531143246410](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200531143246410.png)

* `git checkout -- 文件名`：将工作区恢复到暂存区的版本。

* `git rm 文件名`：删除文件，git会将工作区和暂存区的该文件删除，最后只需commit，仓库的该文件也会删除。

---

- [ ] 如果在某种情况下，你需要加急完成一项临时加塞的新任务。

* git stash：会将当前的工作区内容保存，同时清理掉暂存区。
* git stash list：查看保存的stash内容。
* git stash apply：取出栈顶的stash内容但不删除。
* git stash pop：弹出栈顶的stash内容。

# Git目录文件的认识

当项目使用git进行托管时，一般会在项目里创建一个.git的文件夹，该文件夹就是git目录文件夹。

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200529232407819.png" alt="image-20200529232407819" style="zoom: 67%;" />

1. HEAD：保存的是当前的分支。例如：ref: refs/heads/master
2. config：git的一些配置文件，例如之前保存的用户信息就保存这里面。
3. refs：存放的是head（分支）和tag（标签），如果分支有temp、master，那么head中就存放了这些信息。
4. objects：存放git的对象。

### HEAD与brach的比较

当你在某一分支上工作时，HEAD就会指向该分支。branch可以有很多个，每次切换分支时，HEAD都会指向该分支。

# Git的对象

Git的对象类型有三类：commit、tree（相当于文件夹）、blob（文件内容）

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200529234706033.png" alt="image-20200529234706033" style="zoom:50%;" />

【思考题】：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200529234945837.png" alt="image-20200529234945837" style="zoom:50%;" />



# 什么是分离头指针

分离头指针：`git checkout 某次提交的哈希值`。此时，就处于分离头指针状态。

`git checkout -b 新分支别名 分支哈希值`不会出现分离头指针状态。

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200530003258942.png" alt="image-20200530003258942" style="zoom: 80%;" />

如果出现分离头指针状态就会出现如下警告:warning:

> "仓库正处于分离头状态，你可以查看当前仓库的内容，做一些试验性的修改并提交，但是<u>当你通过运行 git checkout 命令切换到其他分支上时，你所做的所有修改都会被抛弃！</u>如果你认为你当前的修改有价值，需要保留，可以通过运行命令 **git checkout -b NewBranchName** 基于当前仓库的内容创建一个分支 。"



# 文件忽略（.gitignore）

必须在项目下，创建一个叫做.gitignore的文件，里面写上不用管理文件的名称。

下面是GitHub上.gitignore关于Java的不去管理某些文件的配置

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200531153715358.png" alt="image-20200531153715358" style="zoom: 67%;" />

针对不同语言的文件忽略可以参考[Github](https://github.com/github/gitignore)



# 多人协作

 常见传输协议：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200531232502584.png" alt="image-20200531232502584" style="zoom:50%;" />

哑协议与智能协议的直观区别：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200531232621154.png" alt="image-20200531232621154" style="zoom:50%;" />

## 克隆仓库

* `git clone --bare 远程仓库地址 克隆后起的名字.git`：将远程仓库克隆到本地

  ```cmd
  克隆本地的仓库
  git clone --bare file://D:\CODE\Git_test backup_git_test.git
  
  克隆GitHub上的仓库
  git clone --bare https://github.com/siberia123/AlgorithmAndStructure.git back_up_github.git
  
  如果不为克隆后的仓库起别名，那么克隆后的本地仓库会默认是GitHub上仓库的名字。
  git clone --bare git@github.com:siberia123/git_learning.git
  ```

在克隆（备份）后{A B}，如果A发生变更后，要想B也同步更新，那么A与B要建立联系。即{A -> B}，要将B设置为A的上游

* `git remote add 为备份仓库起别名 备份仓库的地址`：添加备份仓库（前提是该仓库已克隆过）

  ```git
  克隆该仓库
  git clone --bare file://D:\CODE\Git_test backup_git.git  
  指定备份仓库
  git remote add backup_git file://D:\CODE\backup_git.git
  ```

* `git push`：向远程仓库发送更新

  ```git
  第一次
  git push --set-upstream backup_git master  第一次发送更新时会报错，跟着git的提示来就好
  
  第二次发送更新时，git会记住之前的操作直接push就可以更新了
  git push
  ```

* `git remote -v`：查看远程仓库有多少（包括本地备份仓库）

## 通过SSH协议与Github建立代码托管

1. 打开git bash 终端。
2. 输入命令：`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`。

3. 得到公钥 id_rsa.pub。
4. 复制到Github中。



### 把本地仓库同步到GitHub

* `git remote add 远程仓库别名 远程仓库地址`：添加远程仓库，这里的远程仓库地址以后就可以直接用别名来代替。

  例如：`git remote add git_learning git@github.com:siberia123/git_learning.git`
  
  ![image-20200601005902637](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200601005902637.png)

* `git push 远程仓库别名`：向远程仓库更新。

  `git push git_learning`，也可以这样提交更新`git push git_learning --all`会将所有的分支提交给GitHub。

  提交的过程中可能会出现这种情况：

  ![image-20200601013533819](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200601013533819.png)

  遇到这种情况是因为远程仓库和本地仓库除了本地仓库有的更新，而远程仓库没有。还有就是远程仓库有的东西，本地仓库没有。比如说，远程仓库有readme而本地仓库没有，本地仓库做了更新而远程仓库没有。

  > 遇到这种情况通常有两个做法：
  >
  > 1. 先 git fetch git_learing/master（把远程仓库的master分支拉下来），再 git merge git_learing/master（再与本地仓库进行合并）。
  >
  > 2. 直接 git pull git_learing/master（fetch + merge）。

  在merge的过程中，可能还会出现这样一种情况：

  ![image-20200601015244265](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200601015244265.png)

  这是因为这两个分支没有关联性（没有交集）

  `git merge --allow-unrelated-histories git_learing/master`：允许没有交集的分支进行合并。

  ![image-20200601015536964](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200601015536964.png)

  最后就可以完成push操作了。

  【更多示例】

  ![image-20200601015728580](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200601015728580.png)

  ![image-20200601015642050](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200601015642050.png)

![image-20200601015738916](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200601015738916.png)



### 不同人修改了不同的文件

如果想为新的仓库起别的名字而不是采用全局的名字可以使用这个命令。（local > global）

* `git config --add --local user.name “新姓名”`：
* `git config --add --local user.email “新邮箱”`：

* `git config --global/--local -l`：查看config配置文件中的相关信息，当然也可以直接去文件中修改。

在.git/config配置文件中，也可以直接修改相关的用户信息：

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200601152136747.png" alt="image-20200601152136747" style="zoom: 80%;" />









# -------------------- version 1.0 ---------------------









# Git初始化

## 设置用户名和邮箱

* `git config --global user.name "xxxxx"`
* `git config --global user.email "xxxxx"`

以上全局性的配置可以在.git/config文件下进行修改。

删除全局性的用户名和邮箱：

* `git config --unset --global user.name`
* `git config --unset --global user.email`



## 初始化仓库

* `git init`：在项目目录下初始化仓库。
* `git init xxxxx`：直接初始化一个带有版本控制的仓库。



## 追踪文件（将文件添加至暂存区）

* `git add xxxxx`
* `git add -u`：将发生更新的文件（已追踪过的）添加至暂存区。

文件忽略，不让其追踪[文件忽略](#文件忽略（.gitignore）)



## 提交至仓库

* `git commit -m "xxxxx"`：会将暂存区的所有文件提交至仓库中。
* `git commit --allow-empty -m "xxxxx"`：允许将没有修改的文件进行提交到仓库。



## 备份

* `git clone xxxxx xxx`：将xxxxx仓库备份至xxx



## 打标签

标签严格意义上说是为哈希值起别名，要记住那么一长串哈希值是十分痛苦的，所以就有了标签这一功能。

* `git tag xxxx hashxxxxx`：打标签
* `git tag -d xxxxx`：删除标签





# Git暂存区

* `git log`：查看commit提交历史
* `git status`：查看文件状态（哪些文件还未add、哪些文件还未commit）
* `gitk / gitk --all`：查看分支 / 查看所有分支



## 后悔药（checkout，reset）

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200611113157290.png" alt="image-20200611113157290" style="zoom: 67%;" />

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200611113320835.png" alt="image-20200611113320835" style="zoom: 67%;" />

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200611113654115.png" alt="20200611113654115" style="zoom: 67%;" />

* `git reset HEAD / git reset`：暂存区的内容会被版本库的内容覆盖重写（工作区不会受影响），相当于`git add xxxx`的相反操作。
  * `git reset HEAD xxxxx / git reset -- xxxxx`：相当于`git add xxxxx`的反向操作，将xxxxx文件撤出暂存区。
* `git checkout -- xxxxx`：工作区的内容会被暂存区的内容覆盖重写
* `git checkout HEAD xxxxx`：工作区和暂存区的内容会被版本库的内容覆盖重写

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200611114253698.png" alt="image-20200611114253698" style="zoom: 67%;" />

重置命令（`git reset`）是Git最常用的命令之一，也是最危险的

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200611220142952.png" alt="image-20200611220142952" style="zoom: 67%;" />

使用参数`--hard`会执行上图中的1、2、3全部的三个动作。即：

i. 替换引用的指向。引用指向新的提交ID。
ii. 替换暂存区。替换后，暂存区的内容和引用指向的目录树一致。
iii. 替换工作区。替换后，工作区的内容变得和暂存区一致，也和HEAD所指向的目录树内容相同。

* `git reset --soft HEAD~1`：工作区和暂存区不改变，但是引用向前回退一次。**当对最新提交的提交说明或者提交**
  **的更改不满意时，撤销最新的提交以便重新提交。**对**最新提交的说明进行修改也可以使用这条命令**：`git commit --amend`（`git  commit --amend -m "xxxxxx"`）

> 命令：`git reset HEAD~1 / git reset --mixed HEAD~1`
> 工作区不改变，但是暂存区会回退到上一次提交之前，引用也会回退一次。
> 命令：:command:`git reset --hard HEAD~1`
> 彻底撤销最近的提交。引用回退到前一次，而且工作区和暂存区都会回退到上一次提交的状态。自上一次以来的提交全部丢失。



## diff

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200611114540269.png" alt="image-20200611114540269" style="zoom:67%;" />

* `git diff`：工作区和暂存区比较
* `git diff --cached`：暂存区和版本库（HEAD所指）比较
* `git diff HEAD`：工作区和版本库（HEAD所指）比较



## stash

* `git stash`：将工作区尚未add的内容和暂存区尚未commit的内容保存

  * `git stash save "xxxxx"`：将保存的内容添加备注

* `git stash pop`：将保存的内容弹出

* `git stash apply`：和git stash pop命令一样，只不过不会将其删除

* `git stash clear`：删除所有保存内容

* `git stash list`：查看保存内容的列表

  



# Git对象

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200529234706033.png" alt="image-20200529234706033" style="zoom:50%;" />

* `git cat-file -t hashxxxx`：查看类型（commit,tree,blob）
* `git cat-file -p hashxxxx`：查看内容

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200611210504101.png" alt="image-20200611210504101" style="zoom: 80%;margin-left:40px"/>

* `git branch`：显示commit当前分支

  <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200611212716976.png" alt="image-20200611212716976" style="margin-left: 1px;" />





# Git改变历史

## 修改某次历史

* `git rebase -i hashxxxx`

  * r

    <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200612110722098.png" alt="image-20200612110722098" style="zoom:80%;" />

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200612111851027.png" alt="image-20200612111851027" style="zoom:80%;" />

![image-20200612111024540](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200612111024540.png)

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200612111109296.png" alt="image-20200612111109296" style="zoom:80%;" />



## 对连续commit进行合并

* `git rebase -i hashxxxx`

  * s

    <img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200612112415300.png" alt="image-20200612112415300" style="zoom: 80%;" />![image-20200612113151421](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200612113151421.png)

    ![image-20200612113127252](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200612113127252.png)

    ![image-20200612113406664](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200612113406664.png)

  

## 对间隔commit进行合并

* `git rebase -i hashxxx`

  * s

  ![image-20200612114327426](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200612114327426.png)![image-20200612114416199](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200612114416199.png)

  ![image-20200612120437481](https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200612120437481.png)









# -------------------- version 2.0 ---------------------









# Git首次配置

## 用户身份

设置用户名和电子邮箱地址。

* `git config --global user.name "xxxxx"`
* `git config --global user.email "xxxxx"`

> Git的每一次提交需要用到这些信息，而且还会被写入到所创建的提交中。



## 检查个人设置

查看所有的设置

* `git config --list`

> 这些设置还可以在.git/config文件下进行查看。同时也可以通过键值对的形式进行查看：`git config user.name`



## 获取帮助

* `git help <verb>`：通过网页形式获取帮助
* `git <verb> -h`：在终端获取帮助





# Git基础

## 获取Git仓库

> 建立Git项目的方式主要有两种：
>
> 1. 在现有的项目中导入Git
> 2. 从服务器上克隆现有的Git仓库

在现有的仓库中导入Git

* `git init`



利用Git将来仓库

* `git init xxxx`



克隆现有仓库

* `git clone https://xxxxxxxx` 

> 如果想将克隆的项目重命名：`git clone https://xxxxxxx <重命名>`
>
> Git可以使用不同的协议传输协议，除了https://、还有git://、SSH协议



## 查看当前文件状态

* `git status`



## 追踪新文件

* `git add xxxx`



## 忽略文件

> 不想让某些文件被显示在未跟踪的文件列表下面，可以创建名为.gitignore的文件。
>
> Github维护了一份相当全面的.gitignore参考示例：https://github.com/github/gitignore

示例：

```
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```



## 查看已暂存和未暂存的变更

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200611114540269.png" alt="image-20200611114540269" style="zoom:67%;" />

* `git diff`：工作区和暂存区比较
* `git diff --cached`：暂存区和版本库（HEAD所指）比较
* `git diff HEAD`：工作区和版本库（HEAD所指）比较



## 提交变更

* `git commit -m "xxxx"`：提交变更，并说明提交理由
* `git commit -a -m "xxxxx"`：跳过暂存区直接提交

> 一般来说是**不允许直接跳过暂存区提交**的。
>
> Git的提交流程：`git add xxx -> git commit`



## 移除文件

* `rm xxxx`：移除工作区文件（Linux系统命令可用）

* `git rm --cached xxxx`：移除暂存区文件

> 如果忘了向.gitignore文件中添加相应的规则，不小心把一个很大的日志文件或者一些编译生成的文件添加进来，使用这种方法就可以将暂存区中的文件删除。

* git rm xxxx：工作区的文件删除了，但是暂存区的该文件还没删除就可以使用这个命令

> 如果工作区的文件还未删除使用了这条命令就会报错：error: the following file has changes staged in the index:1.txt 
>
> (use --cached to keep the file, or -f to force removal)。
>
> 可以使用`git rm -f xxx`将工作区和暂存区的该文件删除



## 重命名文件

* `git mv <原文件名> <新文件名>`

> 当使用`git mv xxx xxxx`重命名文件时，Git会敏锐的发现文件发生了重命名，所以会将工作区的重命名后的文件名运用到暂存区该文件上。这条命令相当于执行了三条命令：`mv xxx xxxx -> git rm xxx -> git add xxxx`



## 查看提交历史

* `git log`



## 撤销操作

对提交到版本库的备注不满意：

* `git commit --amend -m "xxxxx"`：修改最新提交的备注



对提交到暂存区的内容不满意：

* `git reset HEAD / git reset`：暂存区的内容会被版本库的内容覆盖重写（工作区不会受影响），相当于`git add xxxx`的相反操作。
  * `git reset HEAD xxxxx / git reset -- xxxxx`：相当于`git add xxxxx`的反向操作，将xxxxx文件撤出暂存区。



对工作区内容的修改不满意：

* `git checkout -- xxxxx`：工作区的内容会被暂存区的内容覆盖重写



对工作区和暂存区修改的内容不满意：

* `git checkout HEAD xxxxx`：工作区和暂存区的内容会被版本库的内容覆盖重写

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200611114253698.png" alt="image-20200611114253698" style="zoom: 67%;" />



重置命令（`git reset`）是Git最常用的命令之一，也是最危险的

<img src="https://gitee.com/siberiacurrent/pic_repository/raw/master/img/image-20200611220142952.png" alt="image-20200611220142952" style="zoom: 67%;" />

使用参数`--hard`会执行上图中的1、2、3全部的三个动作。即：

i. 替换引用的指向。引用指向新的提交ID。
ii. 替换暂存区。替换后，暂存区的内容和引用指向的目录树一致。
iii. 替换工作区。替换后，工作区的内容变得和暂存区一致，也和HEAD所指向的目录树内容相同。

* `git reset --soft HEAD~1`：工作区和暂存区不改变，但是引用向前回退一次。**当对最新提交的提交说明或者提交**
  **的更改不满意时，撤销最新的提交以便重新提交。**对**最新提交的说明进行修改也可以使用这条命令**：`git commit --amend`（`git  commit --amend -m "xxxxxx"`）

> 命令：`git reset HEAD~1 / git reset --mixed HEAD~1`
> 工作区不改变，但是暂存区会回退到上一次提交之前，引用也会回退一次。
> 命令：:command:`git reset --hard HEAD~1`
> 彻底撤销最近的提交。引用回退到前一次，而且工作区和暂存区都会回退到上一次提交的状态。自上一次以来的提交全部丢失。



## 远程仓库的使用

### 显示远程仓库

* `git branch -av`：显示本地和远程的所有仓库
* `git branch -vv`：显示本地已追踪的远程仓库

### 添加远程仓库

* `git remote add <为远程仓库在本地起的别名> <远程仓库地址>`

> 这个为远程仓库起的别名就可以指代远程仓库的地址了，以后就不用记住那么冗长的仓库地址。

### 从远程仓库拉取数据到本地

* `git fetch <远程仓库>[某分支]`
* `git pull <远程仓库>[某分支]`

> 1. 当克隆仓库时，克隆命令会自动添加远程仓库的地址并取名为 "origin" 。
>
> 2. `git fetch` 命令只会把数据拉取到本地仓库，然而并不会自动将这些数据合并到本地的工作成果中，也不会修改当前工作目录下的任何数据。
> 3. `git pull` 会从远程仓库上获取更新的数据，然后自动尝试将其合并入当前工作目录下的本地数据中。

### 删除和重命名远程仓库

* `git remote rename <原远程仓库名> <新远程仓库名>`
* `git remote rm <远程仓库名>`