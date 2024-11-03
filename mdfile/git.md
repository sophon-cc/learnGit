# Git

## 什么是Git
Git是一个分布式版本控制工具，主要用于管理开发过程中的源代码文件。

区别于SVN，常见的集中式版本控制工具。

版本控制的用途：代码回溯、版本切换、多人协作和远程备份。

## 关联远程仓库
>注意：示例中已经在github用户名为sophon-cc的账户下，创建了一个名为learn-git的远程仓库。对应的仓库ssh为git@github.com:sophon-cc/learn-git.git，应根据实际仓库替换对应信息。
### 通过本地仓库初始化远程仓库
若本地不存在仓库，即从一个文件夹开始创建一个仓库。在本地命令行执行如下代码：
```bash
echo "# learn-git" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:sophon-cc/learn-git.git
git push -u origin main
```
若本地存在一个仓库（但未关联远程仓库），即项目目录下存在.git隐藏文件。则执行这段代码关联远程仓库：
```bash
git remote add origin git@github.com:sophon-cc/learn-git.git
git branch -M main
git push -u origin main
```

### clone远程仓库
直接clone远程的仓库，使用git提供的命令`git clone`。这样也会在本地生成一个本地仓库。
```bash
git clone git@github.com:sophon-cc/learn-git.git
```
## Git基本概念
**版本库**项目目录下.git隐藏文件夹就是版本库，版本库下存储了许多配置信息、日志信息和文件版本信息等。

**工作区**包含.git文件夹的目录就是工作区，也称工作目录，主要用于存放开发的代码。我们直接操作的文件和目录就是工作区中的。

**暂存区**.git文件夹中有很多文件，其中有一个index文件就算暂存区，也叫stage。暂存区是一个临时保存修改文件的地方。


## Git基本概念

## Git-VScode集成操作
