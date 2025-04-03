# Git 常用指令
```bash

--- 撤销修改等

git checkout -- . 
git restore .  # 放弃工作区的所有修改（未暂存文件）

git checkout -- filename
git restore filename # 放弃指定文件的修改

git clean -fd # 强制 目录 永久删除未跟踪的文件

git reset HEAD filename # 撤销暂存的文件

```

# Git入门

**Git与其他版本控制系统的差异**

1. 直接记录快照，而非差异比较
    当你在git中提交更新或者保持项目状态时，它基本上就会对当时的全部文件创建一个快照并保持这个快照的索引。如果文件没有修改，git不再重新存储该文件，而是保留一个链接指向之前存储的文件。
![文件快照流](/GitProNotes/files/imgs/版本文件快照流.png)  

2. 几乎所有的操作都是在本地执行  

3. Git保证完整性  
    不可能在Git不知情时更改任何文件内容或目录内容，因为Git中的所有数据在存储前都计算校验和，然后以校验和来引用.

    Git用于计算校验和的机制叫做**SHA-1散列**.是一个有40个16禁止字符组成的字符串，时基于Git中文件的内容或目录结构计算出来的。看起来如下:  

    > 24b9da6552252987aa493b52f8696cd6d3b00373  
4. Git一般只添加数据

**文件的三种状态**
Git 有三种状态，你的文件可能处于其中之一： 已提交（committed）、已修改（modified） 和 已暂存（staged）
- 已修改表示修改了文件，但还没保存到数据库中。
- 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。
- 已提交表示数据已经安全地保存在本地数据库中。

即对应了Git项目中的三个阶段，工作区，暂存区，Git目录
![stages](/GitProNotes//files/imgs/3stages.png)

- 工作区是对项目的某个版本独立提取出来的内容。放在磁盘上供你使用或修改
- 暂存区是一个文件，保存了下次要提交的文件列表信息，一般在Git仓库目录中，叫暂存区或索引区
- Git仓库目录是Git用来保存项目的元数据和对象数据的地方。

*基本的Git工作流程如下*
> 1. 在工作区中修改文件
> 2. 将你想要下次提交的更改选择性地暂存，只会将更改的部分添加到暂存区
> 3. 提交更新，找到暂存区的文件，将文件快照永久性存储到Git目录

如果 Git 目录中保存着特定版本的文件，就属于**已提交**状态。 如果文件已修改并放入暂存区，就属于**已暂存**状态。 如果自上次检出后，作了修改但还没有放到暂存区域，就是**已修改**状态

**命令行**
只有在命令行模式下才可以执行Git所有的命令，大多数的GUI软件只是实现了其所有功能的一个子集来降低操作难度。所以，学习在命令行下操作Git是十分必要的


**Git的基本配置**

```
$ git config --list 查看所有的配置
```

设置用户信息，即用户名和邮件地址。在每次提交时都会记录这些信息，不可更改

```
$ git config --global user.name "John"
$ git config --global user.email "shuzipper@gmail.com"
```

设置默认的文本编辑器

```
$ git config --global core.editor ...
```

检查某一项特定的git配置

```
$ git config user.name 

最后修改配置值的配置文件
$ git config --show origin rerere.autoUpdate
```

使用help

``` 
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
$ git add -h 获取简明的帮助信息
```


# Git基础
---
- 涵盖各种基本命令。 
- 能够配置并初始化一个仓库（repository），开始或停止跟踪(track)文件，暂存(stage)或提交(commit)更改。
- 配置Git忽略指定的文件和文件模式，撤销错误操作，浏览历史版本及差异
- 远程仓库push和pull操作

## 获取Git仓库
---

- 在已存在目录中初始化仓库
`$ git init`  
该命令将创建一个名为.git的子目录，含有初始化的git仓库中的必须文件

- 克隆现有的仓库
`$ git clone + url`  
```
$ git clone https://github.com/libgit2/libgit2

也可以自定义仓库的名字
$ git clone https://github.com/libgit2/libgit2 mylibgit
```
Git支持多种数据传输协议。上面使用的例子是http,也可以使用git协或者SSH协议传输。

## 记录每次更新到仓库
---

### **文件状态的变化周期**

![文件状态的变化周期](/GitProNotes//files//imgs/文件状态周期.png)  
工作目录下的每一个文件只有两种状态：**已跟踪** 或 **未跟踪** 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录。

可以选择性地将修改后的文件放入暂存区，然后提交所有已暂存的修改，如此反复。

### **检查当前文件状态**
可以使用`git status`命令查看哪些文件处于什么状态

```git
$ git status
On branch master
Your branch is up-to-date with 'origin/master'
nothing to commit, working directory clean
```
在项目下新建一个READEME文件后再次使用`git status`，会看到一个新的未跟踪的文件
 ```
 $ echo 'My Project' > README
 $ git status
 On branch master
 Your branch is up-to-date with 'origin/master'.
 Untracked files:
  (use "git add <file>..." to include in what will be committed)
  README
 nothing added to commit but untracked files present (use "git add" to
 track)
 ```
### **跟踪新文件**

使用命令开始追踪一个新的文件。
`$ git add README`
此时再运行`git status`会看到README文件已经被跟踪并且处于暂存状态:  
```
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
  new file: README
```

`git add`命令使用文件或目录作为参数；如果参数是目录的路径，该命令将递归跟踪该目录下的所有文件
`git add *`命令可以将所有文件加入跟踪状态

### **暂存已修改的文件**  

如果修改一个已被跟踪的文件时，再使用`git status`命令后会看到该文件出现到 `changes not staged for commit`，说明文件的内容发生了变化，但是没有放到暂存区。如果需要暂存，需要运行`git add`  

**git add** 的功能时多样的：
1. 开始跟踪新文件
2. 将已跟踪的文件加入到暂存区
3. 合并时把有冲突的文件标记为已解决状态等

> 精确地将内容添加到下一次提交中

### **状态简览**

> 使用`git status -s / --short`命令可以得到更为紧凑的输出
```
$ git status -s
 M README          修改过的文件
 MM Rakefile       已修改，暂存后又作了修改    
 A  lib/git.lib    新增到暂存区的文件
 M  lib/simple.git.rb
 ?? LICENSE.txt    新添加的未跟踪的文件
```

### **忽略文件**  

一般来说我们总是有些文件无需纳入Git的管理，也不希望其总是出现在未跟踪列表。通常时自动生成的文件，比如日志文件或者时编译过程中创建的临时文件。  
这种情况下，可以创建一个`.gitignore`文件，列出需要忽略的文件格式

```
*.[oa] 忽略所有以.o或者.a结尾的文件，一般都时编译过程中产生的
*~     忽略所有名字以波浪符结尾的文件

此外，你可能还需要忽略log，temp，pid目录以及自动生成的文档等等
```
要对新仓库设置好`.gitignore`文件，避免误提交此类无用的文件

> 文件 .gitignore的格式范式如下
-  所有空行或者以#开头的行都会被Git忽略
- 可以使用标准的glob模式匹配，他会递归地应用与整个工作区
- 匹配模式可以以(/)开头防止递归
- 匹配模式可以使用(/)结尾指定目录
- 要忽略指定模式意外的文件或目录，可在模式前增加感叹号(!)取反

所谓的glob模式匹配是指shell所使用的简化了的正则表达式
- 星号`*`匹配零个或者多个任意字符，`[abc]`匹配任何一个列在方括号中的符号
- 问号`？`只匹配一个任意字符
- 如果在方括号中使用短划线分隔两个字符，表示在字符范围内的都可以匹配。如`[0-9]表示匹配所有0-9的数字`
- 使用两个星号`**`表示匹配任意的中间目录，`比如a/**/z可以匹配a/z,a/b/z,a/b/c/z等`

再看一个.gitignore文件的例子
![示例](/GitProNotes/files/imgs/gitignore.png)

Github中有对不同项目及语言的`.gitignor`文件列表，可以再[github gitignore](https://github.com/github/gitignore)

多个`.gitignore`文件可参考`man gitignore`  

### 查看已暂存和为暂存的修改

倘若你希望知道具体修改了哪些内容，什么地方。可以使用`git diff`命令，其通过文件补丁的格式更加具体地显示了哪些行为发生了改变

此命令是比较工作目录中当前的文件和暂存区快照之前的差异，也就是修改之后还没暂存起来的变化内容

若要查看已暂存的将要添加到下次提交里的内容，可以使用`git diff --staged/--cached`, 此命令将对比已暂存文件与最后一次提交文件的差异

> 注：git diff本身只显示尚未暂存的改动，而不是自上次提交以来所作的所有改动。如果暂存了所有的更新文件，那么运行git diff是什么都没有的


### 提交更新

目前暂存区以及准备就绪，可以提交了。需提前确认还有什么已修改的文件或者新建的文件没有`git add`过，否则提交的时候不会记录这些尚未暂存的变化，所以再提交之前，可以现检查一下所需文件是否都被暂存起来了。

```
$ git commit  会启动文本编辑器来输入提交说明

$ git commit -m "Story 182: Fix benchmarks for speed"
[master 463dc4f] Story 182: Fix benchmarks for speed
 2 files changed, 2 insertion(+)
 create mode 100644 README
```

可以看到实在哪个分支提交的，本次完整提交的SHA-1校验和是什么，以及在本次提交中，多少行代码添加和删改过

**每一次提交，都是对项目作一次快照，以后可以回到这个状态，或者进行比较**

### 跳过使用暂存区域 

使用暂存区域的方式可以精心准备要提交的细节，但是略显繁琐，因此Git提供了一个跳过使用暂存区域的方式。

`$ git commit -a -m "message"`

Git会自动把所有已跟踪的文件暂存起来并一并提交，跳过`git add`步骤

**小心可能会将不必要的文件添加到提交中**

### 移除文件

要从git中移除某个文件，需要从已跟踪文件清单中移除（即从暂存区），然后提交。可以使用`git rm`来完成，并连带从工作目录中删除指定的文件。

如果只是简单地在工作目录手工删除文件，在查看状态时就会出现`changes not staged for commit`部分

如果要删除之前修改过或以及放到暂存区的文件，则需要使用强制删除选项`-f`

另外一种情况时，希望把文件从仓库中删除（移出暂存区），但是希望文件仍然保留在当前目录下。即希望文件保留在磁盘，但是不想让Git继续跟踪。 当忘记添加`.gitignore`文件，不小心将文件添加到暂存区时。使用`--cached`选项可达到此目的

```git
git rm --cached README 将其移除暂存区但是保留在文件夹中

git rm log/\*.log 删除log/目录下扩展名为.log的所有文件

git rm \*~  删除所有以~结尾的文件
```


### 移动文件

Git并不显示地跟踪文件移动操作。如果在Git中重命名了某个文件，仓库中的元数据并不会体现出这是一次改名操作

要在Git中对文件改名，可以
```
$git mv file_from file_to

$ git mv README.md READEME

相当于三条命令
$ mv README.md README
$ git rm README.MD
$ git add README

```

> 如果在Git外对文件重命名时，计得在提交前`git rm`删除旧文件，再`git add`新文件

### 查看提交历史

再若干次更新后或者克隆某个项目之后，你可能想回顾一下提交历史。`git log`可以完成这个任务。

在默认情况下，会按时间先后顺序列出所有的提交，最新的更新排在上面。会列出每个提交的SHA-1校验和，作者的名字和电子邮件地址，提交时间以及说明。

常用的选项以帮助搜索所找的提交
```
 -p --patch 显示每次提交引入的差异，限制显示的日志条目数量

    $ git log -p -2 

 --stat 可以为其附带一系列总结选项,多少修改文件以及文件的哪些行被移除或者时添加了

    $ git log --stat

 --pretty 展示提交历史，不同于默认格式。
    比如online， short，full，fuller选项，展示信息的格式基本一致，但是详尽程度不同、

    $ git log --pretty=online 将每个提交显示在一行

    format选项可以定制记录的显示格式，这样可以对后期分析提取格外有用。

    $ git log --pretty=format:"%h - %an, %ar : %s"

    oneline或format与--graph结合使用时可以形象 地展示项目的分支，合并历史
``` 

Git log 的常用选项

![常用选项](/GitProNotes/files/imgs/git%20log选项.png)

**限制输出长度**
只输出一部分提交历史
```
 -<n> 仅显示最近的n条提交

 --since --until按照时间限制的选项

 $ git log --since=2.weeks  "2023-08-12" "2 years 1 day 3 minutes ago"

 --author 显示指定作者的提交 --grep选项搜索提交说明中的关键字

 可以指定多个--author和--grep搜索条件，--all-match 会输出匹配所有条件的提交

 -S 过滤器，只显示那些添加或删除了该字符串的提交
```

### 撤销操作

有些撤销操作时不可逆的。

提交完才发现漏掉了几个文件没有添加，或者提交信息写错了。可以在`git add`之后执行下面操作
`$ git commit --amend` 用一个新的提交替换旧的提交，尽可能保证仓库历史的清晰

**取消暂存文件**

场景：
> 修改了多个文件并且想要独立为多次修改提交，但是使用`git add *`暂存了他们，需要取消暂存

`$ git reset HEAD CONTRIBUTING.md`

**注**：git reset 是个危险的命令，尤其时加上--hard选项。文件尚未修改，相对安全。

**撤销对文件的修改**

场景：
> 不想保留对文件的修改，将其还原成上次提交的样子

`$ git checkout -- CONTRIBUTING.md`

**注** 
- 这是一个危险的命令，对该文件任何的修改都会消失，git会使用最近提交的版本覆盖掉它。
- 在git中任何已提交的东西几乎总是可以恢复的，设置那些被删除的分支中的提交或使用amend选项覆盖的提交也可以恢复。
- 如果你想保留对文件做出的修改，但是现在仍然需要撤销修改，git保存进度与分支时更好的做法

## 远程仓库的使用
---

### 查看远程仓库

`git remote`命令可以列出指定的每一个远程服务器的简写
如果你克隆了自己仓库，那至少可以看到**origin**--这是Git给你克隆的仓库服务器的默认的名字

```
$ git remote -v 显示需要读写远程仓库使用的Git保存的简写与对应的URL

origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)
```

### 添加远程仓库

`git remote add <shortname> <url>` 可添加一个新的仓库同时指定方便使用的简写

### 从远程仓库中抓取与拉取

```
$ git fetch <remote>

此命令会访问远程仓库，从中拉取所有你还没有的数据，成功后你将会拥有那个远程仓库所有的分支的引用，可以随时合并或查看
```

注意：git fetch只会将数据下载到你的本地仓库，但是不会自动合并或修改你当前的工作。你必须手动将其合并人你的工作

*分支设置了跟踪远程分支，可以使用git pull来自动抓取后合并该远程分支当当前分支*

### 推送到远程仓库

将你所作备份到远程仓库

`git push origin master`

注：只有当你有服务器的写入权限，且之前没有人推送过时，才生效。也就是如果你和其他人在同一时间克隆，他们先推送到上游，你的推送就会被拒绝，你必须先抓取他们的工作将其合并才能进行推送。

### 查看某个远程仓库

查看某一个远程仓库的更多信息
`git remote show <remote>`

### 远程仓库的重命名与移除

```
$ git remote rename pb paul 将pb重命名为paul

$ git remote
origin
paul

$ git remote remove paul

# git remote
origin
```

## 打标签
---

### 列出标签

```
$ git tag
v1.0
v2.0

加上`-l/--list`可以按照通配符列出标签
$ git tag -l "v1.8*"
```

### 创建标签
- 轻量标签 lightweight
像一个不会改变的分支---只是某个特定提交的引用
`$ git tag v1.4-lw`

- 附注标签 annnotated
存储在Git数据库中的一个完整对象，可以被校验，包含打标签者的名字，电子邮件，日期时间，此外还有一个标签信息，且可以输赢GNU Privacy Guard签名并验证。

创建附注标签
`$ git tag -a v1.4 -m "my version 1.4"`

`$ git show v1.4` 可以看到标签信息和提交信息

- 后期打标签
`$ GIT TAG -A v1.2 9FCEB02`


# Git 分支

Git分支的本质上其实是指向提交对象的指针。 默认的分支名是master, master分支会在每次自动提交时向前移动。

![分支](/GitProNotes/files/imgs/分支.png)

### 分支的创建

`$ git branch testing`

**Head**指针，指向当前所在的本地分支。 git branch 命令仅仅是创建一个指针，并不会自动切换到新的分支中去。

可以使用git log指令查看当前各个分支所指的对象

`$ git log --oneline --decorate`

### 分支切换

`$ git checkout testing`

注意：分支切换同时也会改变工作目录中的文件，如果是切换到一个旧的分支，工作目录会回复到该分支最后一次提交时的样子，当Git不能干净利落完成这个任务，它将禁止切换分支。

你可以在不同的分支间不断来回切换和工作，并在时机成熟时将他们合并起来。

可以使用git log命令查看分叉历史

`$ git log --oneline --decorate --graph --all`

通常我们会在创建一个新的分支后立马切换过去，可以使用git checkout -b newbranchname

### 分支的合并

`$ git merge issue53`

在合并之后不再需要的分支可以删除，使得git提交历史更加简洁

`$ git branch -d iss53`

**遇到冲突分支合并**

如果在不同的分支中对同一个文件的同一部分进行了不同的修改，在合并时就会产生冲突。

此时，git做了合并的操作，但是没有自动创建一个新的合并提交，git会暂停下来，等待冲突解决，可以在合并后的任意时刻使用`git status`命令来查看那些因包含合并冲突而处于未合并状态（emerged）状态的文件。

在确认之前有冲突的文件都已经暂存后，可以使用git commit来完成合并并提交。

### 分支管理 

常用的分支管理命令：

$ git branch  得到当前所有分支的列表 * 所指即当前所在的分支

$ git branch -v 查看每一个分支的最后一次提交

$ git branch --mergerd 过滤列表中已经合并到当前分支的分支

$ git branch --no-mergerd 过滤列表中未合并到当前分支的分支

$ git branch -d testing 删除分支

如果包含了还未合并的工作但是仍然想要删除分支并丢掉那些工作

$ git branch -D testing  强制删除

### 分支开发工作流

长期分支
![Alt text](/GitProNotes//files//imgs/长期分支.png)

主题分支
![Alt text](/GitProNotes//files//imgs/主题分支.png)

### 远程分支



