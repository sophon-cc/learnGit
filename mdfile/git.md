# Git

## 什么是Git
Git是一个分布式版本控制工具，主要用于管理开发过程中的源代码文件。

区别于SVN，常见的集中式版本控制工具。

版本控制的用途：代码回溯、版本切换、多人协作和远程备份。

## 关联远程仓库
>注意：示例中已经在github用户名为sophon的账户下，创建了一个名为learn-git的远程仓库。对应的仓库ssh为git@github.com:sophon/learn-git.git，应根据实际仓库替换对应信息。
### 通过本地仓库初始化远程仓库
若本地不存在仓库，即从一个空文件夹开始创建一个仓库。在本地命令行执行如下代码：
```bash
echo "# learn-git" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:sophon/learn-git.git
git push -u origin main
```
若本地存在一个仓库（但未关联远程仓库），即项目目录下存在.git隐藏文件夹。则执行这段代码关联远程仓库：
```bash
git remote add origin git@github.com:sophon/learn-git.git
git branch -M main
git push -u origin main
```

### clone远程仓库
直接clone远程的仓库，使用git提供的命令`git clone`。这样也会在本地生成一个本地仓库。
```bash
git clone git@github.com:sophon/learn-git.git
```
## Git基本概念
**工作区**包含.git文件夹的目录就是工作区，也称工作目录，主要用于存放开发的代码。我们直接操作的文件和目录就是工作区中的。

**暂存区**.git文件夹中有很多文件，其中有一个index文件就算暂存区，也叫stage。暂存区是一个临时保存修改文件的地方。

**版本库**项目目录下.git隐藏文件夹就是版本库，版本库下存储了许多配置信息、日志信息和文件版本信息等。

<div style="text-align: center;">
    <img src="../pictures/git01.png" alt="git basic concepts" width="300">
</div>


## Git基本命令
### 本地仓库基本命令
#### git status
```bash
git status
```
git status 是一个用于查看 Git 仓库当前状态的命令。它会显示：  
1.工作区和暂存区的状态：列出已经修改但未暂存的文件（工作区）和已经暂存但尚未提交的文件（暂存区）。这里的修改既指 modified 也指 deleted。  
2.未跟踪的文件：显示还没有被 Git 追踪的文件，即新创建的文件。  
3.分支信息：显示当前所在分支名称以及该分支相对于上游分支的提交差异（比如 ahead 或 behind）。
#### git add
git add 将用于将文件或更改的文件添加到暂存区，以便在下一次提交（git commit）中包含这些更改。

常见用法：  
1.添加单个文件
```bash
git add <file>
```
例如：git add 1.txt。将1.txt添加到暂存区。  
2.添加所有更改的文件：
```bash
git add .
```
将工作区所有对文件的修改暂存。  
3.其它用法
```bash
# 添加多个文件
git add <file1> <file2>

# 将指定目录下的所有文件添加到暂存区
git add <directory>
```
#### git restore
git restore 是一个用于恢复文件到特定状态的命令，常用于取消修改、还原文件或目录。

常见用法：  
1.恢复工作区文件
```bash
git restore <file>
```
执行该命令会丢弃工作区的修改，通常是为了还原文件状态。如果当前文件被暂存过该命令会将当前文件恢复到暂存状态，否则会将文件恢复到上一次提交。  
2.取消暂存
```bash
git restore --staged <file>
```
用于将文件从暂存区（staging area）移除，但不删除文件本身，也不会影响工作区中的文件内容。等价作用效果的命令`git reset <file>`，两条命令作用效果都是取消暂存。  
3.还原到指定的提交
```bash
git restore --source=<commit_hash> <file>
```
你可以指定一个提交哈希，将文件恢复到那个提交的状态。
> git restore命令可以跟多个文件或使用 $.$ 作用到所有文件上。
#### git reset
git reset 用于撤销提交、取消暂存等。它会将当前分支的 HEAD 指针指向指定的提交，从而改变提交历史或重置文件状态。

常见用法：  
1.取消暂存
```bash
git reset <file>
```
用于将文件从暂存区（staging area）移除，但不删除文件本身，也不会影响工作区中的文件内容。等价作用效果的命令`git restore --staged <file>`，两条命令作用效果都是取消暂存。  
2.版本回退  
git reset有三种工作模式分别是 soft , hard 和 mixed。三种工作模式作用效果对照表如下：
| 模式        | HEAD 移动 | Index 改动 | 工作目录改动 | 是否保留更改   |
| --------- | ------- | -------- | ------ | -------- |
| `--soft`  | ✅       | ❌        | ❌      | ✅ 完全保留   |
| `--mixed` | ✅       | ✅        | ❌      | ✅ 保留工作目录 |
| `--hard`  | ✅       | ✅        | ✅      | ❌ 全部丢弃   |

例如使用这段命令：
```bash
git reset --soft <commit>
```
作用效果是版本库回退到指定版本，并保留暂存区和工作区内容。这样你的指定版本之后的多次提交历史将被忽略，但你任拥有最新代码。
### 远程仓库基本命令
### 分支操作
#### git merge 和 git rebase
**git merge**
 merge 会把两个分支的历史合并在一起，保留所有历史记录，并通常会产生一个新的合并提交（merge commit）。
 示例代码：
```bash
git checkout main
git merge feature
```
效果：
```mathematica
A---B---C (main)
     \ 
      D---E (feature)

==>

A---B---C-------M (main)
     \         /
      D---E--- (feature)
```

**git rebase**
相对于当前分支来说 rebase 会把当前分支的提交挪到目标分支的后面，形成一条线性历史，重写历史。而相对于目标分支来说，没有任何改变发生。
 示例代码：
```bash
git checkout feature
git rebase main
```
这个操作的意思是：把 feature 分支（D 和 E）的更改，移动到 main 的最新提交 C 后面，就像是从 C 才开始写 D 和 E 一样。效果：
```mathematica
A---B---C (main)
     \ 
      D---E (feature)

==>

A---B---C (main)
             \
              D'---E' (feature)
```
### 标签操作

## Git-VScode集成操作
