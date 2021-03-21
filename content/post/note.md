---
title: "摸鱼笔记"
date: 2021-03-21T17:09:23+08:00
draft: false 
---

## git

gitee：

```
 git add .
 git commit -m "备注内容"
 git push origin 
```



**删除远程仓库**

```
git rm -r --cached .DS_Store
git commit -m "删除.DS_Store" 
git push
```

用-r参数删除目录, git rm --cached .DS_Store 删除的是本地仓库中的文件，且本地工作区的文件会保留且不再与远程仓库发生跟踪关系，如果本地仓库中的文件也要删除则用git rm .DS_Store

![](https://img-blog.csdnimg.cn/20190604193324344.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JlbmJlbl8yMDE1,size_16,color_FFFFFF,t_70)



### git pull -rebase

<img src="https://img-blog.csdn.net/20160926112331953" alt="git pul" style="zoom:90%;" />

这个命令做了以下内容： 
a.把你 commit 到本地仓库的内容，取出来放到暂存区(stash)（这时你的工作区是干净的） 
b.然后从远端拉取代码到本地，由于工作区是干净的，所以不会有冲突 
c.从暂存区把你之前提交的内容取出来，跟拉下来的代码合并

所以 rebase 在拉代码前要确保你本地工作区是干净的，如果你本地修改的内容没完全 commit 或者 stash，就会 rebase 失败。

**删除文件后需要 git add -A, 光 git add. 不行**

git add -A 保存所有的修改 
git add . 保存新的添加和修改，但是不包括删除 
git add -u 保存修改和删除，但是不包括新建文件。

### git stash

```
git stash 

git stash apply
```

使用`git stash`就可以将你当前未提交到本地（和服务器）的代码推入到Git的栈中，这时候你的工作区间和上一次提交的内容是完全一样的，所以你可以放心的修Bug，等到修完Bug，提交到服务器上后，再使用`git stash apply`将以前一半的工作应用回来。

　git stash命令主要用于以下情形：

- 发现有一个类是多余的，想删掉它又担心以后需要查看它的代码，想保存它但又不想增加一个脏的提交。这时就可以考虑`git stash`。
- 使用git的时候，我们往往使用分支（branch）解决任务切换问题，例如，我们往往会建一个自己的分支去修改和调试代码, 如果别人或者自己发现原有的分支上有个不得不修改的bug，我们往往会把完成一半的代码`commit`提交到本地仓库，然后切换分支去修改bug，改好之后再切换回来。这样的话往往log上会有大量不必要的记录。其实如果我们不想提交完成一半或者不完善的代码，但是却不得不去修改一个紧急Bug，那么使用`git stash`就可以将你当前未提交到本地（和服务器）的代码推入到Git的栈中，这时候你的工作区间和上一次提交的内容是完全一样的，所以你可以放心的修Bug，等到修完Bug，提交到服务器上后，再使用`git stash apply`将以前一半的工作应用回来。
- 经常有这样的事情发生，当你正在进行项目中某一部分的工作，里面的东西处于一个比较杂乱的状态，而你想转到其他分支上进行一些工作。问题是，你不想提交进行了一半的工作，否则以后你无法回到这个工作点。解决这个问题的办法就是`git stash`命令。储藏(stash)可以获取你工作目录的中间状态——也就是你修改过的被追踪的文件和暂存的变更——并将它保存到一个未完结变更的堆栈中，随时可以重新应用。

### Go 交叉编译（cross-compilation）

macos输出linux/windows二进制文件：

```
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build main.go
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build main.go
```

Linux下编译Mac, Windows平台的64位可执行程序

```
CGO_ENABLED=0 GOOS=darwin GOARCH=amd64 go build main.go
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build mian.go
```

Windows下编译Mac, Linux平台的64位可执行程序：

```
SET CGO_ENABLED=0
SET GOOS=darwin
SET GOARCH=amd64
go build main.go

SET CGO_ENABLED=0
SET GOOS=linux
SET GOARCH=amd64
go build main.go
```

GOOS：目标可执行程序运行操作系统，支持 darwin，freebsd，linux，windows
GOARCH：目标可执行程序操作系统构架，包括 386，amd64，arm

Golang version 1.5以前版本在首次交叉编译时还需要配置交叉编译环境：

```
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 ./make.bash
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 ./make.bash
```





### 寻找Mac中的隐藏其他灰色文件（缓存or大视频）

1,打开终端，

```
du -sh *
```

2,快速定位到最大占比的文件：Library

````
du -d 1 -h
````

3,查看占比依次查看

```
open xxx
rm -rf xxx
```

4,该删的删

/Users/d47/Library/ScreenRecordings 里面经常有奇怪的自动摄影视频



**在图形界面显示隐藏文件**：

```
defaults write com.apple.finder AppleShowAllFiles -bool true

killall Finder
```

**恢复文件的隐藏状态**

```
defaults write com.apple.finder AppleShowAllFiles -bool false

killall Finder
```



## Linux

没有权限

```
sudo chmod -R 777 某一目录 
//-R 是指级联应用到目录里的所有子目录和文件 777 是所有用户都拥有最高权限
```

top命令：

PID：进程的ID　　

USER：进程所有

PR：进程的优先级别，越小越优先被执

VIRT：进程占用的虚拟内

RES：进程占用的物理内

SHR：进程使用的共享内

S：进程的状态,S表示休眠，R表示正在运行，Z表示僵死状态，N表示该进程优先值为负

%CPU：进程占用CPU的使用

%MEM：进程使用的物理内存和总内存的百分

TIME+：该进程启动后占用的总的CPU时间，即占用CPU使用时间的累加值

COMMAND：进程启动命令名称

### 配置ssh快捷链接

```
cd .ssh
ssh-copy-id -i id_rsa.pub root@192.168.0.123 -p 23333
ssh-add -K id_rsa
```



### macOS上解决rar问题：The Unarchiver

### mac交叉编译出错:

https://blog.csdn.net/woaizijiheni/article/details/50614062

![image-20210305200711273](/Users/d47/Library/Application Support/typora-user-images/image-20210305200711273.png)



## Go

```
bolt "go.etcd.io/bbolt" 飘红
```

引起以上的原因主要是etcd中使用的bbolt和grpc版本冲突引起

解决办法：

删除原来已生成得go.mod和go.sum

```
go mod init
 
go mod edit -replace github.com/coreos/bbolt@v1.3.4=go.etcd.io/bbolt@v1.3.4
 
go mod edit -replace google.golang.org/grpc@v1.29.1=google.golang.org/grpc@v1.26.0
 
go mod tidy

```

### hugo

```
brew install hugo

//设置好 GOPATH 环境变量，获取源码并编译：
export GOPATH=$HOME/go
go get -v github.com/spf13/hugo

//更新所有Hugo的依赖库，增加 -u 参数
go get -u -v github.com/spf13/hugo

//使用Hugo快速生成站点，比如希望生成到/Users/d47/hugo 路径：
sudo hugo new site /Users/d47/hugo
```

![image-20210319214830710](/Users/d47/Library/Application Support/typora-user-images/image-20210319214830710.png)



进入站点目录：

![image-20210319214943690](/Users/d47/Library/Application Support/typora-user-images/image-20210319214943690.png)

创建about页面

```
sudo hugo new about.md
```

![image-20210319215201047](/Users/d47/Library/Application Support/typora-user-images/image-20210319215201047.png)

![image-20210319215140921](/Users/d47/Library/Application Support/typora-user-images/image-20210319215140921.png)

内容是 `Markdown` 格式的，`+++` 之间的内容是 [TOML](https://github.com/toml-lang/toml) 格式的，根据你的喜好，你可以换成 [YAML](http://www.yaml.org/) 格式（使用 `---` 标记）或者 [JSON](http://www.json.org/) 格式。

创建第一篇文章，放到 `post` 目录，方便之后生成聚合页面。

```
sudo hugo new post/first.md

//打开编辑 post/first.md

---
date: "2015-10-25T08:36:54-07:00"
title: "first"
 
---

### Hello Hugo

 1. aaa
 1. bbb
 1. ccc
```



选皮肤（略）https://www.gohugo.org/theme/

```
git clone https://github.com/jchatkinson/HugoMDL.git
```

运行Hugo

```
hugo server -D
```

![image-20210319232116154](/Users/d47/Library/Application Support/typora-user-images/image-20210319232116154.png)



|- archetypes ：存放default.md，头文件格式 
|- content ：content目录存放博客文章（markdown文件） 
|- data ：存放自定义模版，导入的toml文件（或json，yaml） 
|- layouts ：layouts目录存放的是网站的模板文件 
|- static ：static目录存放图片，css等静态资源 
|- config.toml ：config.toml是网站的配置文件







参考：

https://www.gohugo.org
