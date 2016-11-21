---
layout: post
comments: true
title:  "Jekyll+Github Pages搭建博客"
date:   2016-11-21 12:13:30
tags: jekyll githubpages
---

初衷：

想通过博客记录自己工作中遇到的问题,省去本地查看文档的麻烦,这样可以随时上网查阅,同时分享给需要的人.
至于为啥不去现成的博客网站去写,例如新浪博客,博客园等.完全是机缘巧合下看见同事在Github上有一个分享
链接,点击去发现里面都是他的一些学习笔记和生活记录等文章,觉得很炫酷,才有了冲动想创建自己的一个博客.
后来在百度上搜索Github+博客,才知道是通过Jekyll和Github Pages的方式搭建的.

最初,对于如何搭建博客完全懵B.都是上网找各种教程按部就班的照着别人说的做.由于每个人的搭建方法和文章的
表述不一样.导致配置的过程中遇到各种各样的问题.出现问题后就各种百度,解决一个又出现另一个.做到最后脑子
里还有没有一个完整的框架.再之后就试图通过Github的推荐的文档来配置.慢慢的才对搭建博客网站有了一个大概
的思路.此时才意识到一个正确的指导文档是多么的重要！凡是都得走官方啊！毕竟只有官方才能对他们的产品有
最好的诠释.

好了,废话不多说了.开始我们的博客搭建之旅吧.简单来说搭建博客分为两部分.一是:配置jekyll.二是:将jekyll
和Github Pages关联起来.三是:如何写一个博客.

配置jekyll是最基础的部分.只要jekyll配置好了,你就成功了一小步.关于如何配置jekyll,其实简单说起来也就只有
三步.分别执行以下三个命令即可:

{% highlight ruby %}

Install Chocolatey

choco install ruby -y

gem install jekyll

{% endhighlight %}

下面我们来说说这三个命令是用来干吗的.

首先先跟大家介绍下,由于jekyll默认是不支持Windows的,但是没办法,用不起苹果电脑的我们只能想办法在Windows上搭建它.当然不需要我们自己再去研究琢磨了,已经有各路大神前辈们为我们铺好了道路,把他们的经验无私地分享给了我们.所以我们直接套用他们的方法即可.在此要感谢他们做出的辛苦努力了！致敬！

jekyll是默认支持Linux平台的,所以在Linux平台上安装jekyll将是异常简单顺利的.仅仅只需要敲几个命令即可,其他什么都不用管,无需担心有任何问题,系统将为你自己安装好.但是Windows平台的你可能就要付出一倍的努力来成功配置它.

在Windows上安装jekyll,我们可以模拟其在Linux上的安装过程,使用命令行的方式.首先我们需要安装一个工具:Chocolatey.打开一个终端,执行 Install Chocolatey.系统会自动帮我们完成安装.至于这个工具是用来干什么的,你可以简单的把它当做一个Linux系统中安装软件的工具.例如：apt,yum之类的.使用它来帮我安装软件,可以帮我们解决安装过程中的依赖关系.(用它来安装Ruby之后附带有Gems,无需自己再单独下载)

接着我们来安装Ruby和Ruby development kitPermalink

为啥要安装Ruby呢？因为安装 Jekyll 的最好方式就是使用 RubyGems. 而RubyGems是依附于Ruby. 你只需要打开终端输入以下命令就可以安装了：$ gem install jekyll 所有的 Jekyll 的 gem 依赖包都会被自动安装,所以你完全不用去担心。

打开一个终端执行以下命令：

{% highlight ruby %}

choco install ruby -version 2.2.4

{% endhighlight %}

安装成功示例：

{% highlight ruby %}

$ ruby -v
ruby 2.2.4p230 (2015-12-16 revision 53155) [i386-mingw32]

{% endhighlight %}

{% highlight ruby %}
choco install ruby2.devkit - 编译json gem时需要使用

{% endhighlight %}

设置Ruby development kit

Ruby开发工具包并没有设置Ruby环境变量，所以我们需要手动设置：

在C:\tools\DevKit2目录下打开命令行界面执行

{% highlight ruby %}

ruby dk.rb init
//创建配置文件config.yml 

ruby dk.rb review 
//来查看是否初始化成功

{% endhighlight %}

编辑文件config.yml在其中包含你自己的Ruby安装路径：- C:/tools/ruby22.

最后通过执行命令：

{% highlight ruby %}

ruby dk.rb install 
//将DevKit绑定到Ruby来完成你的DevKit安装

{% endhighlight %}

Ruby包含一种安装方法即所谓的gems(Gem是一个管理Ruby库和程序的标准包,它通过Ruby Gem(如 http://rubygems.org/ )源来查找、安装、升级和卸载软件包,非常的便捷).
jekyll就是通过gem安装的.

常用的命令有

{% highlight ruby %}

// 从Gem源安装gem包
$ gem install [gemname]
/ 删除指定的gem包，注意此命令将删除所有已安装的版本
$ gem uninstall [gemname]
// 查看本机已安装的所有gem包
$ gem list [--local]

{% endhighlight %}

更多命令请参考：
[http://www.jianshu.com/p/728184da1699](http://www.jianshu.com/p/728184da1699)

下面我们就可以开始安装jekyll了,打开一个终端执行：

{% highlight ruby %}

gem install jekyll

{% endhighlight %}

执行的过程中,可能会遇到SSL_connect error,例如：

{% highlight ruby %}

ERROR: Could not find a valid gem 'jekyll' (>= 0), here is why: 
Unable to download data from https://rubygems.org/ 
- SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://rubygems.org/latest_specs.4.8.gz)

{% endhighlight %}

百度之后找到一个解决办法,就是在执行的命令后面加上 “--source http://rubygems.org/ ” 但是如果你参考的是官方文档,会有一个更一劳永逸的方法.去Github上下载GlobalSignRootCA.pem,(https://raw.githubusercontent.com/rubygems/rubygems/master/lib/rubygems/ssl_certs/index.rubygems.org/GlobalSignRootCA.pem) 把它放在Ruby安装目录下的一个文件夹里,例如%RUBY_DIR%\ssl_cert\ 然后打开一个命令行测试一下:

{% highlight ruby %}

SET SSL_CERT_FILE=%RUBY_DIR%\ssl_cert\GlobalSignRootCA.pem

{% endhighlight %}

将%RUBY_DIR%替换为你实际的路径 然后再重新执行gem install命令,如果可以正常下载了,则增加一个新的环境变量SSL_CERT_FILE,值为实际的路径值.

至此jekyll安装完毕.如果你想查看下安装的jekyll的版本,可以执行下面命令之一：

{% highlight ruby %}

$ jekyll --version
$ gem list jekyll

{% endhighlight %}

使用Jekyll创建博客站点.(此时的博客未集成到Github Pages,只是一个纯粹的jekyll 博客).

{% highlight ruby %}

jekyll new myblog
//创建一个新的站点

cd myblog
//一定要进入创建的对应blog目录，否则服务无法开启

jekyll serve
//启动一个地址为http://localhost:4000/的服务器

{% endhighlight %}

Jekyll服务器默认端口是4000，所以打开浏览器输入：http://localhost:4000 就可以看到生成的博客页面.

## 安装Nokogiri-gem ##

github-pages运行时需要Nokogiri这个软件包，但是要运行在64位Windows系统上还需要执行以下命令：

{% highlight ruby %}

choco install libxml2 -Source "https://www.nuget.org/api/v2/"

choco install libxslt -Source "https://www.nuget.org/api/v2/"

choco install libiconv -Source "https://www.nuget.org/api/v2/"

gem install nokogiri --^
   --with-xml2-include=C:\Chocolatey\lib\libxml2.2.7.8.7\build\native\include^
   --with-xml2-lib=C:\Chocolatey\lib\libxml2.redist.2.7.8.7\build\native\bin\v110\x64\Release\dynamic\cdecl^
   --with-iconv-include=C:\Chocolatey\lib\libiconv.1.14.0.11\build\native\include^
   --with-iconv-lib=C:\Chocolatey\lib\libiconv.redist.1.14.0.11\build\native\bin\v110\x64\Release\dynamic\cdecl^
   --with-xslt-include=C:\Chocolatey\lib\libxslt.1.1.28.0\build\native\include^
   --with-xslt-lib=C:\Chocolatey\lib\libxslt.redist.1.1.28.0\build\native\bin\v110\x64\Release\dynamic

{% endhighlight %}

注意:替换C:\Chocolatey\lib为你的实际安装路径


## 安装Github-gem ##

打开命令行界面安装 Bundler: gem install bundler
在你的博客根目录(即通过jekyll new myblog创建的博客)中创建名为Gemfile不带任何后缀名的文件
拷贝复制下面两行到文件中：

{% highlight ruby %}

source 'http://rubygems.org'
gem 'github-pages'

{% endhighlight %}

注意: 由于在使用的Ruby版本中使用SSL链接报错,所以这里我们使用不加密的链接

打开命令行界面,切换到你本地博客库的根目录,安装github-pages:

{% highlight ruby %}

bundle install 
//此命令就是往jekyll site中安装github pages的gem包

{% endhighlight %}

如果执行失败,提示:当前你使用的addressable 2.5.0已激活,但是根据的你Gemfile你需要使用2.4.0版本.可能是由于你安装了两个不同版本的jekyll导致的,你需要卸载最新的jekyll版本.使用gem list来查看当前安装的所有package.然后找到jekyll,再使用gem uninstall jekyll -v [version.number].例如:我已经安装的jekyll版本是jekyll (3.3.1,2.4.0).则我就需要使用命令
gem uninstall jekyll -v 3.3.1卸载jekyll 3.3.1,保留2.4.0的版本,使其版本号与Ruby保持一致.

这样就完成了本地在jekyll site上安装了github-gem,此时你可以通过 jekyll serve 命令来在本地启动你的博客.


最后一部分: 用Github Pages生成个人博客

Github Pages生成网站的两种方式的基本原理

方式一

一个Github账号对应一个用户或者一个组织,Github会给这个用户分配一个域名:username.github.io 在Github上有一个账号名为 username (任意),一个项目名为 username.github.io (固定格式,username与账号名一致,如果username与账号名不一致,就相当于以project的方式创建的site,最后生成的site link就不会是http://username.github.io 了,我之前一直在纠结为什么我创建的site link这么长,后来才发现这是根本原因所在.),项目分支名为 master (默认固定)个分支有类似下面的目录结构：

author

这样，当你访问 http://username.github.io/ 时,Github会使用Jekyll解析用户username 名下的 username.github.io 项目中,分支为 master 的源代码，为你构建一个静态网站，并将生成的 index.html 展示给你。

方式二

Github还为每个项目提供了域名,例如你的项目名为 blog ,Github会为这个项目提供一个 username.github.io/blog 的域名在Github上有一个账号名为 username (任意),有一个项目名为 project (任意),项目分支名为 gh-pages (固定),这个分支也有上面那种结构.

那么当你访问 http://username.github.io/project 时,Github会去解析username用户下project 项目的 gh-pages 分支下的源代码,为你构建网站。

所以要搭建自己的博客你有两种选择：

建立名为 username.github.io 的项目,在 master 分支下存放网站源代码

建立名为 blog 的项目,在 gh-pages 分支下存放网站源代码

接下来我们用第二种方式搭建博客

> 创建个人仓库

因为我们使用第二种方式创建，所以仓库的名称可以随意取.在这以Test01为名字。

在Repossitory name处填入Test01，选择Public类型，然后点击下面的Create respository按钮：

author

> 为仓库创建Github Pages

点击Settings设置：

author

翻到下面，选择Launch automatic page generator：

author

再接着编译一下内容，选择页面上的元素，再点Continue to layouts：

author

选好模板，点击Publish page，就生成了Github Pages：

author

并且，分支自动设置为了gh-pages：

author

让我们打开这个网站看看效果吧！在浏览器输入： http://zyc8904.github.io/Test01 (zyc8904换成你的github名称，Test01换成项目名)：

author


## 将本地Jekyll代码部署到Gihub仓库 ##

我们的目的就是在本地利用Jekyll搭建好博客，并且测试开发，写文章，然后推送到Github仓库里，发布到线上。前面已经教大家搭建好了本地Jekyll，接下来就教大家把它推送到Github仓库中。

把仓库克隆到本地

这里我以windows下的Git客户端：Github for windows 为例来为大家讲解（如果想用Git Bash命令行克隆，请移步这里）

首先打开我们的Github for windows客户端，点击左侧的 + 号按钮：

author

选择Clone，并且找到刚才新建的Test01仓库，然后点击最下方的Clone按钮：

author

然后选择存放位置，这里我放到D盘，然后点击确定：

author

这样我们的Test01就被克隆到了本地。打开D盘可以看见这个文件夹。这时我们启动Jekyll服务，别忘了进入Test01目录下：

cd d:\Test01
jekyll serve --watch
现在我们打开[http://localhost:4000]，就可以看见我们在Github上创建的Pages页面了。

> 拷贝本地Jekyll目录到版本库中

删除Test01下的示例文件和文件夹：_site 、stylesheets 、index.html 、params.json。

再把本地Jekyll创建的blog里的所有文件和文件夹全都拷贝到Test01里面。

如果你前面和我用的是一样的命令的话

jekyll serve --watch
这时你可能会发现cmd命令行中，自动处理了一些操作并且显示：

author

这就是我加–watch 的原因，不用再自己去生成，Jekyll服务器检测到你有变化会自动再生成页面，这时你在刷新浏览器，那么就会看到Github页面就会被Jekyll生成的页面代替了：

author

> 部署本地Jekyll站点到Github Pages上

打开Github for windows客户端，会看到 changes 上面有个点，说明他已经检测到我们的仓库的变化，并且自动把变化都列举在了下面：

author

这时我们在下方填入这次改变的名称和描述，点击Commit to gh-pages。然后再点击右上角的Sync把这次改变push到Github远程仓库里：

author

稍等几分钟，我们再次访问 http://zyc8904.github.io/Test01 就可以看见我们用Jekyll做的站点已经被部署到github上了：

author

以后你只要在本地Test01文件夹下的_posts文件夹里写文章，然后推送到远程仓库就可以了。