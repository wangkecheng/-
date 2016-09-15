# CocoaPods

## CocoaPods简介

CocoaPods是一个用来帮助我们管理第三方依赖库的工具。它可以解决库与库之间的依赖关系，下载库的源代码，同时通过创建一个Xcode的workspace来将这些第三方库和我们的工程连接起来，供我们开发使用。

使用CocoaPods的目的是让我们能自动化的、集中的、直观的管理第三方开源库。

## 安装Ruby环境

### 检查Mac是否安装Ruby和gem

在终端中输入命令：`ruby --version` 和`gem --version`

输出如下类似提示符，则表示Ruby环境已安装

``` shell
$ ruby --version
ruby 2.0.0p643 (2015-02-25 revision 49749) [x86_64-darwin14.3.0]
$ gem --version
2.4.8
```

PS：Ruby是一门开发语言，gem为Ruby第三方库管理工具，CocoaPods是用Ruby写的一个第三方工具。

### 若提示`command not found` 则需要安装Ruby环境

- 安装Ruby环境需要安装Xcode及Command Line Tools。

- 安装Command Line Tools：`xcode-select --install`

- 安装RVM，Ruby的多版本管理工具。

  ``` shell
  $ curl -L https://get.rvm.io | bash -s stable
  $ source ~/.rvm/scripts/rvm
  $ rvm install 2.0.0
  $ rvm use 2.0.0
  $ /bin/bash --login
  ```

## 安装CocoaPods

使用淘宝的镜像安装Ruby的第三方库，修改gem的镜像：

``` shell
$ gem sources --remove https://rubygems.org/
$ gem sources -a https://ruby.taobao.org/
```

为了验证你的Ruby镜像是**并且仅是**淘宝，可以用以下命令查看：

``` shell
$ gem sources -l
# 只有在终端中出现下面文字才表明你上面的命令是成功的：
* CURRENT SOURCES *
https://ruby.taobao.org/
```

如果出现多个需要将其余的源删除。

终端中执行安装CocoaPods

``` shell
$ sudo gem install cocoapods
```

执行完成后，需要初始化CocoaPods的环境

``` shell
$ pod setup （可进入cd .~/cocopods 用du -sh * 查看进度）
 若一直是Setting up CocoaPods master repo 或者 Updating local specs repositories
 则用 pod install --verbose --no-repo-update 命令
```
##更新CocoaPods
 sudo gem install cocoapods --pre

## 使用CocoaPods

1. 创建Xcode工程并切换到该工程路径

2. 使用命令`pod init`在当前文件夹下生成一个**Podfile**文件

3. 编辑该文件，在该文件中输入如下信息： 用vim podfile或者open -e Podfile 打开编辑，按 i 开始编辑 不能够直接找到podfile后打开，否则生成后会有Your Podfile has had smart quotes sanitised这样的提示以及工程中打开podfile，target 'MyApp' do 这个引号会变成英文名 
  - 解决方案：若将target，此时将中引号改为英文引号后执行 pod update (但没有成功)
  - 保存文件：esc + shift + :  然后wq保存 (若不做任何修改，则不是wq 而是q!)

   ``` shell
   $ vim Podfile
 platform :ios, '8.0'
use_frameworks!
target 'MyApp' do
  pod 'AFNetworking', '~> 2.6'
  pod 'ORStackView', '~> 3.0'
  pod 'SwiftyJSON', '~> 2.3'
end
   ```

   该文件中的命令格式为：`pod '第三库名称', '版本号'`

   第三库名称，名称一定要正确，不然有可能安装失败。

   版本号标识区别

   > \>= 1.0 至少版本为1.0
   >
   > ~> 1.0 兼容1.0版本的最新版
   >
   > == 1.0或1.0 都表示指定版本
4. 安装工程依赖的第三方库

   ``` shell
   $ pod install （或者pod install --verbose --no-repo-update）
   Updating local specs repositories
   Analyzing dependencies
   Downloading dependencies
   Installing AFNetworking (2.5.4)
   Installing KVNProgress (2.2.2)
   Installing SDWebImage (3.7.3)
   Generating Pods project
   Integrating client project
   [!] Please close any current Xcode sessions and use `CocoaPodsDemo.xcworkspace` for this project from now on.
   Sending stats
   Pod installation complete! There are 3 dependencies from the Podfile and 3 total
   pods installed.
   ```

   若出现`pods installed`字样表示安装成功。

5. 关闭Xcode工程，打开.xcworkspace文件。

6. 在工程中导入第三库文件，只需要`#import <AFNetworking.h>`类似的即可，开启CocoaPods之旅。
7. 在工程中Supporting Files(含有main.m文件那个文件夹)新建prifix.pch文件：CTR+N -> other ->选中.pch文件，->在Build Setting 中的搜索框中输入 prefix header，找到prefix prefix header项将 所创建的.pch文件路径托进去， 如：/Users/IOS1601/Desktop/test/test/PrefixHeader.pch 设置相对路径  $(SRCROOT)/test/PrefixHeader.pch, 最后在.pch文件中import要用的第三方包

8.查找第三方库版本

```
  pod search AFNetworking
```
9.查看ruby中的类库
```
 rvm list
```
10.
```
工程中还要添加第三方类库，打开podfile 添加类库并保存后
直接pod update 即可添加类库
还可用下面方法跳过Analyzing dependencies
pod update --verbose --no-repo-update
```

更多用法参考本文提供的参考链接。

 
 
## 参考链接

1. <http://code4app.com/article/cocoapods-install-usage>
2. <http://blog.csdn.net/wzzvictory/article/details/18737437>
3. <http://blog.csdn.net/wzzvictory/article/details/19178709>
4. http://www.jianshu.com/p/7884ec8da77e
5. http://blog.csdn.net/p1129530686/article/details/52209615

# Carthage

## Carthage简介

Carthage的目标是用最简单的方式来管理Cocoa第三方框架。

Carthage编译你的依赖，并提供框架的二进制文件，但你仍然保留对项目的结构和设置的完整控制。Carthage不会自动的修改你的项目文件或编译设置。

**Carthage只正式支持动态框架，动态框架能够在任何版本的OS X上使用，但只能在iOS 8及以上版本使用。**

## 安装Homebrew

OS X 不可或缺的套件管理器，用于安装命令工具。

终端中执行如下命令：

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 安装Carthage

终端执行命令安装Carthage

```
brew update
brew install carthage
```

## 使用Carthage

1. 创建Xcode工程并切换到该工程路径

2. 创建一个`Cartfile`，将你想要使用的框架列在里面

   ```
   github "AFNetworking/AFNetworking" ~> 3.0
   github "rs/SDWebImage"
   ```

3. 运行`carthage update`，将获取依赖文件到一个`Carthage.checkout`文件夹，然后编译每个依赖

4. 在你的应用程序target的`General`设置标签中的`Embedded Binaries`区域，将框架从`Carthage.build`文件夹拖拽进去。

## 参考链接

1. <http://www.cocoachina.com/ios/20141204/10528.html>
2. [官方文档](https://github.com/Carthage/Carthage)

# Carthage与CocoaPods的不同

1. Carthage只支持iOS 8及以上版本使用。

2. 首先，CocoaPods默认会自动创建并更新你的应用程序和所有依赖的Xcode workspace。Carthage使用xcodebuild来编译框架的二进制文件，但如何集成它们将交由用户自己判断。CocoaPods的方法更易于使用，但Carthage更灵活并且是非侵入性的。

3. CocoaPods的目标在它的README文件描述如下：

   > …为提高第三方开源库的可见性和参与度，创建一个更中心化的生态系统。

   与之对照，Carthage创建的是去中心化的依赖管理器。它没有总项目的列表，这能够减少维护工作并且避免任何中心化带来的问题（如中央服务器宕机）。不过，这样也有一些缺点，就是项目的发现将更困难，用户将依赖于Github的趋势页面或者类似的代码库来寻找项目。

4. CocoaPods项目同时还必须包含一个podspec文件，里面是项目的一些元数据，以及确定项目的编译方式。Carthage使用xcodebuild来编译依赖，而不是将他们集成进一个workspace，因此无需类似的设定文件。不过依赖需要包含自己的Xcode工程文件来描述如何编译。

5. 最后，我们创建Carthage的原因是想要一种尽可能简单的工具——一个只关心本职工作的依赖管理器，而不是取代部分Xcode的功能，或者需要让框架作者做一些额外的工作。CocoaPods提供的一些特性很棒，但由于附加的复杂性，它们将不会被包含在Carthage当中。