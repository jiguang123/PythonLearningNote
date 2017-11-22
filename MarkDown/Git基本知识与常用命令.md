# Git基本知识与常用命令
## 一、版本控制系统
Linus一直痛恨的CVS及SVN都是集中式的版本控制系统，而Git是分布式版本控制系统，集中式和分布式版本控制系统有什么区别呢？

先说集中式版本控制系统，版本库是集中存放在中央服务器的，而大家工作的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始工作，工作完成，再把自己的修订推送给中央服务器。这类系统，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。

![image](https://dn-anything-about-doc.qbox.me/userid1labid485time1423114955957)

那分布式版本控制系统与集中式版本控制系统有何不同呢？首先，分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样，你工作的时候，就不需要联网了，因为版本库就在你自己的电脑上。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件A，你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。

和集中式版本控制系统相比，分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库，某一个人的电脑坏掉了不要紧，随便从其他人那里复制一个就可以了。而集中式版本控制系统的中央服务器要是出了问题，所有人都没法干活了。

在实际使用分布式版本控制系统的时候，其实很少在两人之间的电脑上推送版本库的修改，因为可能你们俩不在一个局域网内，两台电脑互相访问不了，也可能今天你的同事病了，他的电脑压根没有开机。因此，分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。

![image](https://dn-anything-about-doc.qbox.me/userid1labid485time1423115040073)

许多这类系统都可以指定和若干不同的远端代码仓库进行交互。籍此，你就可以在同一个项目中，分别和不同工作小组的人相互协作。你可以根据需要设定不同的协作流程，比如层次模型式的工作流，而这在以前的集中式系统中是无法实现的。

## 二 Git常用命令

### 2.1 Git 配置
使用Git的第一件事就是设置你的名字和email,这些就是你在提交commit时的签名，每次提交记录里都会包含这些信息。使用git config命令进行配置：

```
$ git config --global user.name "Scott Chacon"
$ git config --global user.email "schacon@gmail.com"
```

执行了上面的命令后,会在家目录(/home/shiyanlou)下建立一个叫.gitconfig 的文件（该文件为隐藏文件，需要使用ls -al查看到）. 内容一般像下面这样，可以使用vim或cat查看文件内容:

```
$ cat ~/.gitconfig
[user]
        email = schacon@gmail.com
        name = Scott Chacon
```

上面的配置文件就是Git全局配置的文件，一般配置方法是git config --global <配置名称> <配置的值>。

如果你想使项目里的某个值与前面的全局设置有区别(例如把私人邮箱地址改为工作邮箱)，你可以在项目中使用git config 命令不带 --global 选项来设置. 这会在你当前的项目目录下创建 .git/config，从而使用针对当前项目的配置。

### 2.2 Clone一个仓库
为了得到一个项目的拷贝(copy),我们需要知道这个项目仓库的地址(Git URL). Git能在许多协议下使用，所以Git URL可能以ssh://, http(s)://, git://. 有些仓库可以通过不只一种协议来访问。

我们在github.com上提供了一个名字为gitproject的供大家测试的公有仓库，这个仓库可以使用下面方式进行clone：

```
$ git clone https://github.com/shiyanlou/gitproject
```

### 2.3 初始化一个新的仓库
可以对一个已存在的文件夹用下面的命令让它置于Git的版本控制管理之下。

创建代码目录project：

```
$ cd /home/shiyanlou/
$ mkdir project
```
进入到代码目录，创建并初始化Git仓库：

```
$ cd project
$ git init
```
Git会输出:

```
Initialized empty Git repository in /home/shiyanlou/project/.git/
```
通过ls -la命令会发现project目录下会有一个名叫.git 的目录被创建，这意味着一个仓库被初始化了。可以进入到.git目录查看下有哪些内容。



### 2.4 正常的工作流程

git的基本流程如下：

1. 创建或修改文件
1. 使用git add命令添加新创建或修改的文件到本地的缓存区（Index）
1. 使用git commit命令提交到本地代码库
1. （可选，有的时候并没有可以同步的远端代码库）使用git push命令将本地代码库同步到远端代码库

进入我们刚才建立的project目录，分别创建文件file1，file2，file3：

```
$ cd project
$ touch file1 file2 file3
```

修改文件，可以使用vim编辑内容，也可以直接echo添加测试内容。

```
$ echo "test" >> file1
$ echo "test" >> file2
$ echo "test" >> file3
```

此时可以使用git status命令查看当前git仓库的状态：


```
$ git status
On branch master

Initial commit

Untracked files:
   (use "git add <file>...") to include in what will be committed)

       file1
       file2
       file3
nothing added to commit but untracked files present (use "git add" to track)
```

可以发现，有三个文件处于untracked状态，下一步我们就需要用git add命令将他们加入到缓存区（Index）。

使用git add命令将新建的文件添加到：

```
$ git add file1 file2 file3
```
将修改内容添加到本地缓存区，通配符可以把当前目录下所有修改的新增的文件都自动添加：

```
$ git add *
```




然后再次执行git status就会发现新的变化：

```
$ git status
On branch master

Initial commit

Changes to be committed:
    (use "git rm --cached <file>..." to unstage)

       new file: file1
       new file: file2
       new file: file3
```

你现在为commit做好了准备，你可以使用 git diff 命令再加上 --cached 参数，看看缓存区中哪些文件被修改了。进入到git diff --cached界面后需要输入q才可以退出：

```
$ git diff --cached
```
如果没有--cached参数，git diff 会显示当前你所有已做的但没有加入到索引里的修改。

如果你要做进一步的修改, 那就继续做, 做完后就把新修改的文件加入到缓存区中。

当所有新建，修改的文件都被添加到了缓存区，我们就要使用git commit提交到本地仓库：

```
$ git commit -m "add 3 files"
```

需要使用-m添加本次修改的注释，完成后就会记录一个新的项目版本。除了用git add 命令，我们还可以用下面的命令将所有没有加到缓存区的修改也一起提交，但-a命令不会添加新建的文件。

```
$ git commit -a -m "add 3 files"
```

再次输入git status查看状态，会发现当前的代码库已经没有待提交的文件了，缓存区已经被清空。

至此，我们完成了第一次代码提交，这次提交的代码中我们创建了三个新文件。需要注意的是如果是修改文件，也需要使用git add命令添加到缓存区才可以提交。如果是删除文件，则直接使用git rm命令删除后会自动将已删除文件的信息添加到缓存区，git commit提交后就会将本地仓库中的对应文件删除。

这个时候如果本地的仓库连接到了远程Git服务器，可以使用下面的命令将本地仓库同步到远端服务器：


```
$ git push origin master
```
这时候可能需要你输入在Git服务器上的用户名和密码。


git log命令可以显示所有的提交(commit)：

```
$ git log
```
如果提交的历史纪录很长，回车会逐步显示，输入q可以退出。


### 2.5 小结
本节讲解了几个基本命令：

git config：配置相关信息

git clone：复制仓库

git init：初始化仓库

git add：添加更新内容到索引中

git diff：比较内容

git status：获取当前项目状况

git commit：提交

git branch：分支相关

git checkout：切换分支

git merge：合并分支

git reset：恢复版本

git log：查看日志


## 三 参考链接
1. [Git 实战教程](https://www.shiyanlou.com/courses/4)
1. [Git 使用指南](http://www.runoob.com/git/git-tutorial.html)

## 四 连接远程仓库Github
![image](http://www.runoob.com/wp-content/uploads/2015/03/1BCB4379-1A25-4C77-BB82-92B3E7185435.jpg)

以上信息告诉我们可以从这个仓库克隆出新的仓库，也可以把本地仓库的内容推送到GitHub仓库。
现在，我们根据 GitHub 的提示，在本地的仓库下运行命令：


```
$ mkdir runoob-git-test                     # 创建测试目录
$ cd runoob-git-test/                       # 进入测试目录
$ echo "# 菜鸟教程 Git 测试" >> README.md     # 创建 README.md 文件并写入内容
$ ls                                        # 查看目录下的文件
README
$ git init                                  # 初始化
$ git add README.md                         # 添加文件
$ git commit -m "添加 README.md 文件"        # 提交并备注信息
[master (root-commit) 0205aab] 添加 README.md 文件
 1 file changed, 1 insertion(+)
 create mode 100644 README.md

# 提交到 Github
$ git remote add origin git@github.com:tianqixin/runoob-git-test.git
$ git push -u origin master
```
















