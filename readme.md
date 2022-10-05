# Git 的相关指令和 Github 使用方法

## 0 目录

- [Git 的相关指令和 Github 使用方法](#git-的相关指令和-github-使用方法)
  * [0 目录](#0-目录)
  * [1 Git 安装与本地的 Git 仓库管理](#1-git-安装与本地的-git-仓库管理)
    * [1.1 安装（Windows）](#11-安装windows)
    * [1.2 本地仓库管理](#12-本地仓库管理)
      * [1.2.1 新建仓库](#121-新建仓库)
      * [1.2.2 删除仓库](#122-删除仓库)
  * [2 Github 的仓库管理](#2-github-的仓库管理)
  * [3 （新建）远程 Github 仓库与本地仓库关联](#3-新建远程-github-仓库与本地仓库关联)
    + [3.1 创建 github 仓库](#31-创建-github-仓库)
    + [3.2 创建本地仓库](#32-创建本地仓库)
    + [3.3 配置 SSH](#33-配置-ssh)
    + [3.4 同步远程到本地](#34-同步远程到本地)
    + [3.5 本地库的修改，提交与远程同步](#35-本地库的修改提交与远程同步)
  * [4 将 Github 仓库快速部署到本地](#4-将-github-仓库快速部署到本地)

## 1 Git 安装与本地的 Git 仓库管理

### 1.1 安装（Windows）

首先，在官网https://git-scm.com/download/win下载 Git 的Windows版安装包并安装：

![az1](img/az1.png)

安装过程省略，按默认的来就可以了，完成后菜单里会多出几个 Git 的相关程序，找到 `Git Bash`并打开，进入命令行。由于 Git 最初是在 Linux 上用来做版本控制的工具，并不支持 MacOS，Windows 系统，所以我们使用的`Git Bash`是 Linux 风格移植过来的控制台，与 Windows 的 `Powershell` `CMD` 有不少区别，需要使用 Linux 的相关指令进行操作，**可能需要一点 Linux 基础**。

首次使用我们需要为 Git 设置用户名和账号，这样才能让你的机器在分布式的仓库系统中有“身份证”，使用以下命令指定用户名和邮箱账号：

```
git config --global user.name "your-name"
git config --global user.email "your-email@example.com"
```

需要注意 `--global` 参数表示此机器上所有的仓库都会使用此身份配置。

[返回目录](#0-目录)

### 1.2 本地仓库管理

**我们对于 Git 仓库的管理，比如新建、删除、同步、版本控制，都需要在 `Git Bash` 里面使用指令进行**。在我们的硬盘里， Git 仓库和普通文件夹最本质的区别是前者有`.git`这个隐藏目录，`.git`里面存放的是关于仓库的各种信息，比如说各个版本对应的文件hooks、用户配置、暂存缓存、logs这些保证仓库正常运行的材料，一般情况下不要去修改`.git`的文件，以防止仓库运行出现错误:pig:。

[返回目录](#0-目录)

#### 1.2.1 新建仓库

新建仓库的步骤是先使用`mkdir`新建目录，然后进入目录中使用`git init`将目录初始化为一个空的仓库，即增加了`.git`组件并设置为`master`分支，这样一个仓库就新建好了：

```
mkdir my_repo
cd my_repo && git init
```

![az3](img/az3.png)

[返回目录](#0-目录)

#### 1.2.2 删除仓库

因为`.git`是仓库管理组件，只要将`.git`删除就可以将它从 Git 仓库变回普通目录；如果想要连同内容一起删除，那直接删除整个仓库目录即可。在 `Git Bash` 中使用 `rm -rf` 命令移除目录：

```
# 删除 .git，可以看见分支提示(master)也消失了，变回普通文件夹目录
rm -rf .git/

# 删除整个my_repo仓库（包括内容），然后退回刷新内存，此时my_repo目录已经不见
rm -rf ../my_repo
cd ..
```

![az4](img/az4.png)

![az5](img/az5.png)

[返回目录](#0-目录)

## 2 Github 的仓库管理

...... （这个比较简单）

## 3 （新建）远程 Github 仓库与本地仓库关联

### 3.1 创建 github 仓库

首先要在 Github 中创建一个仓库，比如说`Aldenhovel/git-demo`，采用默认的设置就好了。

![1](img/1.png)

[返回目录](#0-目录)

### 3.2 创建本地仓库

然后我们在本地也创建一个仓库，在`git bash`中新建一个`git-demo`目录，然后使用：

```
git init
```

来初始化，这一步相当于新增了`.git`组件，把这个目录变成 git 仓库，默认的分支参数是`master`。

![2](img/2.png)

[返回目录](#0-目录)

### 3.3 配置 SSH

Github 与本地的通信需要使用 SSH 来验证身份，所以要生成 SSH 密钥，在`git bash`中输入（记得换成你自己的邮箱）：

```
ssh-keygen -t rsa -C "your-email@example.com"
```

然后回车即可得到生成的 SSH 密钥，存放在用户主目录下的`.ssh`下，其中生成的两个文件：

- `id_rsa` 是你的私钥，注意小心保管不要泄露。
- `id_rsa.pub`是公钥，我们在 Github 上提交的 SSH 密钥就是这个。

![3](img/3.png)

我们切换到对应目录下，用文本方式打开`id_rsa.pub`文件，将里面的内容复制下来。然后打开 Github 主页，在个人设置中找到`SSH and GPG keys`选项，点击`New SSH key`按钮：

![4](img/4.png)

将在`id_rsa.pub`中的公钥复制过来，可以改个名字方便管理，然后点击添加：

![5](img/5.png)

完成后就可以在列表中看到你的密钥：

![6](img/6.png)

[返回目录](#0-目录)

### 3.4 同步远程到本地

现在我们已经有了三大法宝：

- 远程的 Github 仓库。
- 本地的 Git 仓库。
- 配置好的用于远程和本地通信的 SSH 密钥。

可以进行本地和远程的双向同步了。首先是我比较喜欢的做法：在远程创建仓库，再同步到本地，然后在本地编辑完成后推送到远程，这么做比较简单。

首先我们在 Github 上找到 `Aldenhovel/git-demo` 仓库的 SSH 地址（用 HTTPS 也可以），把地址复制下来：

![7](img/7.png)

前面使用`git init`会产生默认为`master`的分支，因为这是我们自己的库，先不需要考虑分支冲突的问题，所以我们直接切换到`main`分支：

```
git branch -M main
```

![12](img/12.png)

然后在`git bash`里面使用：

```
git remote add git-demo-origin git@github.com:Aldenhovel/git-demo.git
```

将他关联过来，这里的`git-demo-origin`是仓库名，一般远程库约定成俗叫`origin`，也可以改别的名字比如`xx-origin`（我的习惯）：

![8](img/8.png)

现在本地库和远程库就产生了关联。

[返回目录](#0-目录)

### 3.5 本地库的修改，提交与远程同步

假设我们现在每日的工作是在本地库做修改，完成后再统一推送到远程，这个过程需要使用三个步骤对应三个命令 `add` `commit` `push`：

1. `add` 将新的、修改过的文件提交到本地的缓冲区。
2. `commit` 将缓冲区的文件提交到本地库并完成本地更改。
3. `push` 将本地库推送到远程库完成同步。

为什么需要 `add` 和 `commit` 分开，这是为了数据修改安全所设计的步骤逻辑，即所有修改要么一次全部改完要么完全不动，以防止修改过程中出现的突发情况导致文件部分修改而混乱（数据库的内容）。

我们尝试编辑一个`readme.md`文档并把它推送到 Github，随便写点东西：

![10](img/10.png)

使用`add`命令可以将文件添加到缓冲区，可以逐个文件添加，也可以在库的主目录下使用`.`将整个库添加：

```
git add .
```

然后用`commit`命令提交到本地库，注意`-m`是评论参数，你需要对你的提交加一点说明：

```
git commit -m "version 0.1"
```

最后使用`push`命令进行远程同步：

```
git push -u git-demo-origin main
```

![9](img/9.png)

没有出什么差错就好:pig:。最后我们检查 Github 发现信息已经同步过去啦：

![11](img/11.png)

[返回目录](#0-目录)

## 4 克隆远程仓库到本地

前面的是从0开始先建立 Github 仓库，再关联本地，最后在本地编辑项目并同步到本地。多数时候我们不需要从0开始建立仓库，而是在新的设备上将 Github 上的库拉下来开始上手干活。这里我们假设在本地建立一个`git-demo-2`仓库，并同步远程`Aldenhovel/git-demo`仓库。

使用`git clone`指令将远程仓库克隆到本地，因为我们现在已经有了一个`git-demo`本地库了，需要重新指定本地库名以防止冲突：

```
git clone git@github.com:Aldenhovel/git-demo.git git-demo-2
```

![13](img/13.png)

然后我们将这个新的库与`Aldenhovel/git-demo`远程库做关联：

```
git remote add git-demo-2 git@github.com:Aldenhovel/git-demo.git
```

![14](img/14.png)

后面就与前面的步骤一样啦，首先我们修改下`readme.md`：

![15](img/15.png)

然后，使用`add` `commit` `push` 来推送同步：

```
git add .
```

```
git commit -m "readme.md changed"
```

```
git push -u git-demo-2 main
```

![16](img/16.png)

完成，没有报错:pig:，去 Github 上检查下，发现已经同步成功：

![17](img/17.png)

[返回目录](#0-目录)



## 5 拉取最新的远程仓库

先说明下在 Git 指令里面 `clone` `fetch` `pull` 的区别：

- `clone` 指令可以直接将远程仓库整个克隆过来，完全从无到有，不需要做任何`init`的操作。
- `fetch` 指令将现有的仓库与远程的仓库做对比，将远程新的版本变化拉到本地。
- `pull` 指令是 `fetch` 与 `merge` 的结合，将远程仓库的拉取到本地并合并。

`fetch`和`pull`指令的主要区别是将远程的信息拉过来后是否需要直接合并，比较严谨的情况下，先将远程仓库`fetch`过来，经过检查后再`merge`是比较好的，但是方便起见我们直接使用`pull`比较多。