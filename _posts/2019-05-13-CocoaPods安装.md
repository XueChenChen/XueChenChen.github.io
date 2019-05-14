---
layout:     post
title:      "CocoaPods 安装"
date:       2019-05-13 21:00:00
author:     "Chen"
header-img: "img/swift/swift-zhinan.jpg"
tags:
    - iOS
    - CocoaPods
---


**好久没弄过iOS了最近游戏要接个SDK，要用CocoaPods 之前没记录下这次记录一下**

## CocoaPods 安装

### 1. 查看源 
    gem sources -l //查看ruby源

 **默认显示：**
    
```
hawk-ipv6:~ mac$ gem sources -l
*** CURRENT SOURCES ***

https://rubygems.org/
```

### 2. ruby源在墙内是访问不到的,需要置换为国内（之前一直用淘宝的但现在不更新了，用另一个）
    gem source -a https://gems.ruby-china.org

**显示：**
```
hawk-ipv6:~ mac$ gem source -a https://gems.ruby-china.org
Error fetching https://gems.ruby-china.org:
	bad response Not Found 404 (https://gems.ruby-china.org/specs.4.8.gz)
```

**我们在浏览器打开（[https://gems.ruby-china.org](https://gems.ruby-china.org)**

**官方显示**

```
服务域名更换公告
因域名备案问题，.org 域名无法继续提供 RubyGems 镜像服务，我们提供 .com 代替 .org 的域名，其他一切不变！！
详情访问
https://gems.ruby-china.com
如有其他疑问，请在 Ruby China 联系。
```

**so, 我们换成官方提示的源地址**

```
gem source -a https://gems.ruby-china.org
```

**OK 再次查看源**

```
gem sources -l
```

**显示：**

```
gem sources -l
*** CURRENT SOURCES ***

https://rubygems.org/
https://gems.ruby-china.com
```

#### 3. 升级Gem（ruby管理包的命令工具） ，防止安装出现因版本低的错误
```
sudo gem update --system　  //升级gem
gem -v 　　　　　　　　　　　　//查看版本
```

#### 4. 安装 CocoaPods 

    sudo gem install cocoapods

**显示报错:**

```
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /usr/bin directory.
```

**更改权限:**

```
sudo chmod 777 /usr/bin
chmod: Unable to change file mode on /usr/bin: Operation not permitted
```

#### 擦嘞不让改 百度用了另一条命令

```
sudo gem install cocoapods -n /usr/local/bin
```

**显示 安装成功**

```
Done installing documentation for xcodeproj, molinillo, cocoapods-try, netrc, cocoapods-trunk, cocoapods-stats, cocoapods-search, cocoapods-plugins, cocoapods-downloader, cocoapods-deintegrate, fuzzy_match, cocoapods-core, cocoapods after 8 seconds
13 gems installed
```

**然后 搞去吧**

### OVER