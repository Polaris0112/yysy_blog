---
title: "{{ replace .Name "-" " " | title }}"
description:
date: {{ .Date }}
image:
math:
license:
hidden: false
comments: true
draft: false
---

# Github Pages + Hugo + hugo-theme-stack主题搭建全记录

##  背景

无论是在工作还是闲时，笔者都会折腾一些软件工具、系统等等东西，有时候仅靠备忘录或者是其他备注/文档工具使用的局限性会比较大，近期翻了翻自己的GitHub，发现2018年的自己弄的博客还在，但是属于几年前自己工作刚开始几年随便记录的一些工作上的知识相关的文章，不过保存也还算不错。虽然自己在家里也有Mac Mini来做工作站，用来跑一些网赚脚本和当做私人网盘，不过在可视化和发布上面还是比不上直接在公网上更为直接。

这里我考虑的有几点需求：

- 成本尽量降到最低
- 发布需要自动化
- 文章整理方便，条理化

- 附加：需要账号体系或者评论系统，导流tg群可以大家一起讨论，互助互利

根据以上几点进行方案筛选，以前的框架使用过hexo，其实整体用起来还不错。不过秉着折腾的原则，这次就使用Hugo，网上教程文章数量也不少，但是质量参差不齐，有不少文章按照实际操作起来其实是跑不下来的，所以本文是我部署的全流程+踩坑过程和解决方案。



##  概念、前期准备

### GitHub Pages

[GitHub Pages](https://pages.github.com/) 是一组静态网页集合(Static Web Page)，这些静态网页由 [GitHub](https://github.com/) 托管(host)和发布，所以是 GitHub + Pages。

### Hugo

[Hugo](https://gohugo.io/) 是用Go语言写的静态网站生成器(Static Site Generator)。可以把Markdown文件转化成HTML文件。

### 准备工作

1. 准备好github账号
2. 准备好git的操作环境
3. 前往[Hugo Themes](https://themes.gohugo.io/)找到自己想要的主题并且记录仓库地址（比如我已经选好了[hugo-theme-stack](https://github.com/CaiJimmy/hugo-theme-stack)）
4. （建议）先了解命令行的操作，比如`ls`,`cd`,`git`,`cp`等等
5. （可选）准备好域名，永久免费域名我后续会出教程



## 部署过程

### 安装Hugo

[官方安装文档](https://gohugo.io/getting-started/installing/)，这里仅展示每个平台中其中一种安装方式。

#### Windows

- 设置你的文件夹

你将需要一个存储 Hugo 可执行文件、博客内容（你创建的的那些文件），以及生成文件（Hugo 为你创建的 HTML）的地方。

1. 打开 Windows Explorer。
2. 创建一个新的文件夹，`D:\Hugo`。
3. 创建一个新的文件夹，`D:\Hugo\bin`。
4. 创建一个新的文件夹，`D:\Hugo\Sites`。

- 下载预先编译好的 Windows 版本的 Hugo 可执行文件

使用 go 编译 Hugo 的一个优势就是仅有一个二进制文件。你不需要运行安装程序来使用它。相反，你需要把这个二进制文件复制到你的硬盘上。我假设你将把它放在 `D:\Hugo\bin` 文件夹内。如果你选择放在其它的位置，你需要在下面的命令中替换为那些路径。

1. 在浏览器中打开 https://github.com/spf13/hugo/releases
2. 当前的版本是 hugo_0.13_windows_amd64.zip。
3. 下载那个 ZIP 文件，并保存到 `D:\Hugo\bin` 文件夹中。
4. 在 Windows Explorer 中找到那个 ZIP 文件，并从中提取所有的文件。
5. 你应该可以看到一个 `hugo_0.13_windows_amd64.exe` 文件。
6. 把那个文件重命名为 `hugo.exe`。
7. 确保 `hugo.exe` 文件在 `D:\Hugo\bin` 文件夹。（有可能提取过程会把它放在一个子文件夹中。如果确实这样的话，使用 Windows Explorer 把它移动到 `D:\Hugo\bin`。）
8. 使用 `D:\Hugo\bin>set PATH=%PATH%;D:\Hugo\bin` 把 hugo.exe 可执行文件添加到你的 PATH路径中。

#### MacOS

```shell
brew install hugo
```

#### Linux

根据自身Linux版本选择对应的安装方式

```shell
## 安装包管理snap
sudo snap install hugo

## 安装包管理Homebrew
brew install hugo

## 存储库包Arch Linux
sudo pacman -S hugo

## 存储库包Debian
sudo apt install hugo

## 存储库包Fedora
sudo dnf install hugo

## 存储库包openSUSE
sudo zypper install hugo

## 存储库包Solus
sudo eopkg install hugo

## 容器Docker
docker pull klakegg/hugo

## 源码安装
## 1.先安装Git（这里不展开说明）
## 2.再安装Go环境（这里不展开说明）
## 3.最后安装Hugo
go install -tags extended github.com/gohugoio/hugo@latest
```

#### 检查Hugo

以MacOS系统为例

```shell
hugo version
hugo v0.96.0+extended darwin/amd64 BuildDate=unknown
```

能正常返回结果即安装成功。目前这里我还没有踩过坑，但是安装Hugo成功率是比较大的。

**注意**：**建议安装**带有extended字样的版本，主要是extended支持Sass/SCSS，有些主题可能需要会用到，所以避免重复部署，还是安装有extended字样的。

### 设置Github博客源代码仓库/本地启动

**注意**：这里其实是需要创建两个仓库，一个用来放博客源代码，一个用来放Hugo生成出来的静态文件。他们的关系是：A仓库放源代码（只是存放，不涉及Pages和访问），B仓库放静态文件（用于Pages和外网访问）

#### 创建博客源代码仓库

1. 仓库命名（随意）
2. 勾选**Public**，设置为公开仓库![FireShot Capture 010 - Create a ](/Users/all/Downloads/FireShot Capture 010 - Create a .jpg)

#### 部署Hugo源代码到本地

1. 使用`hugo`命令创建网页文件夹

```shell
hugo new site yysy
```

2. 配置上述`yysy`文件夹的`git`配置绑定博客源代码仓库

```shell
cd yysy
git init
git remote add https://github.com/Polaris0112/yysy_blog.git
```

此时，yysy文件夹下目录状态应该是这样的

![image-20230113161358579](/Users/all/Library/Application Support/typora-user-images/image-20230113161358579.png)

- **archetypes**：存放用hugo命令新建的md文件应用的front matter模版
- **config.toml**：网站配置文件
- **content**：存放内容页面，如Blog
- **data**：存放创建站点时Hugo使用的其他数据
- **layouts**：存放定义网站的样式，写在`layouts`文件下的样式会覆盖安装的主题中的 `layouts`文件同名的样式
- **static**：存放所有静态文件，如图片
- **themes**：存放主题文件

#### 部署hugo-theme-stack主题

1. 拉取主题hugo-theme-stack

```shell
git submodule add https://github.com/CaiJimmy/hugo-theme-stack themes/hugo-theme-stack
```

2. 把主题所需文件拷贝到主目录`yysy`下

```shell
cp -r themes/hugo-theme-stack/exampleSite/ ./
```

**注意**：这里复制之后，`yysy`目录下会多一个`config.yaml`，这个`yaml`文件才是适配这个主题的文件，后面是用这个来配置，所以`config.toml`**需要删除**。若同时保留两个`config`文件，之后Github Pages会读取`config.toml`而不是`config.yaml`。

#### 测试Hugo Server

可以启动本地服务，来测试我们的配置是否正确

```shell
## 在yysy目录下运行
hugo server -D
```

正常返回是![Snipaste_2023-01-13_16-28-08](/Users/all/Downloads/Snipaste_2023-01-13_16-28-08.jpg)

这时候就能打开浏览器访问 http://localhost:1313/

显示以下页面就是最最最原始的测试页面，元素都能正常展示。（到目前为止还没联动Github Pages，只是本地启动调试）

![image-20230113162938496](/Users/all/Library/Application Support/typora-user-images/image-20230113162938496.png)



### 设置Github Pages仓库

#### 创建Github Pages仓库

1. 命名GitHub Pages仓库，这个仓库名**必须是** `<username>.github.io `的格式，`<username>` 是自己的GitHub的用户名。比如我的Github用户名是Polaris0112，那我就要创建polaris0112.github.io作为Pages的仓库名。
2.  勾选**Public**，设置为公开仓库。![FireShot Capture 010 - Create a  (/Users/all/Downloads/FireShot Capture 010 - Create a  (1).jpg)](/Users/all/Downloads/FireShot Capture 010 - Create a  (1).jpg)

#### 推送静态文件

1. 创建workflows文件。进入上述`yysy`目录，创建`.github/workflows/gh-pages.yml`文件（目录不存在的话就创建目录），文件内容如下，复制后保存文件。

```yaml
## cat .github/workflows/gh-pages.yml
# 将 Hubo 博客构建后部署到 Github Pages
name: Deploy github-pages

# 在 master 主干分支的任何 push 事件都会触发本 DevOps 工作流水线
on:
  push:
    branches: [ master ]

# 以下是本串行执行工作流的所有组成部分
jobs:
  # 这里只定义了一个名为 "deploy" 的多步骤作业
  build-deploy-hugo-blog:
    # 将后续的所有工作步骤都运行在最新版的 ubuntu 操作系统上
    runs-on: ubuntu-latest

    # 本构建和部署作业的所有步骤定义如下
    steps:

    # Step 1 - Checks-out 你的代码库到 $GITHUB_WORKSPACE
    - name: Checkout blog code repo
      uses: actions/checkout@v2 # 这是 Github 官方提供的一个动作模块
      with:
          submodules: true  # 同步更新所使用的 Hugo 模板
          fetch-depth: 0    # 更新到该模板最新的版本

    # Step 2 - 配置最新版本的 Hugo 环境
    - name: Setup Hugo
      uses: peaceiris/actions-hugo@v2  # 这是 Github Actions 市场中的一个动作模块
      with:
          hugo-version: 'latest'

    # Step 3 - 清理代码库中 public 目录中的内容
    - name: Clean public directory
      run: rm -rf public  # 彻底删除这个目录

    # Step 4 - 用最新版本的 Hugo 构建个人博客站点
    - name: Build blog site
      run: hugo --minify

    # Step 5 - 创建用于私有域名所需要的 CNAME 文件
    - name: Create CNAME file
      run: echo 'martinliu.cn' > public/CNAME

    # Step 6 - 将构建好的博客站点推送发布到 gh-pages 分支
    - name: Deploy blog to Github-pages
      uses: peaceiris/actions-gh-pages@v3
      with:
          github_token: ${{ secrets.DEPLOY_KEY }}
          publish_dir: ./public
```

保存文件后先更新博客源代码仓库

```shell
## yysy目录下执行
git add . 
git commit -m "提交更新workflows"
git push -u origin main
```

2. 使用`hugo`命令生成对应静态文件

```shell
hugo
```

![image-20230113163849511](/Users/all/Library/Application Support/typora-user-images/image-20230113163849511.png)

![image-20230113163918193](/Users/all/Library/Application Support/typora-user-images/image-20230113163918193.png)

运行后，`yysy`目录下会生成`public`文件夹，里面就是根据其他文件生成出来，整个网站的静态文件。

3. 进入public目录后，配置git仓库并上传

```shell
cd public
git init
git remote add https://github.com/Polaris0112/Polaris0112.github.io.git
git add . 
git commit -m "提交更新博客静态文件"
git push -u origin main
```

成功推送之后应该是会触发Github Actions，具体在Github Pages仓库（就是仓库名有github.io那个）中Issue那一栏会有Action标签，在那里可以看到每次推送之后触发任务的全部流程细节。

![image-20230113164418830](/Users/all/Library/Application Support/typora-user-images/image-20230113164418830.png)

4. 在Pages页面可以看到部署状态，并且可以尝试从github提供的连接（比如我的就是https://polaris0112.github.io/）进行访问，访问结果应该要与本地跑的localhost地址显示的要一样。

![image-20230113164647942](/Users/all/Library/Application Support/typora-user-images/image-20230113164647942.png)

### 结束语

到这里，已经完成了博客的部署，接下来就是调整配置等等相关细节问题，后续会根据笔者使用情况出对应的教程，有兴趣的朋友可以继续阅读以下文章：

- 如有个人域名，将Github Pages设置个人域名并且支持HTTPS
- 让搜索引擎能搜索该网站
- Hugo-theme-stack的配置
- Hugo-theme-stack添加搜索栏、Archive栏、Links栏、Abount栏
- Hugo-theme-stack使用emoji表情，增加加载进度条，添加评论插件
- 免费域名（附带续费方式，相当于永久免费）