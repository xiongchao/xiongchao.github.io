---
layout: post
title: Phabricator使用cpplint和gtest
---

>“效率是做好工作的灵魂”
> ― 切斯特菲尔德

Phabricator是facebook开源的一款优秀的code review工具。Phabricator支持配置lint和unit test工具。我们的代码主要使用c++，代码规范遵循google开源的c++代码规范。Phabricator本身不支持cpplint和gtest，需要自己配置。

## 配置使用cpplint

tmux依赖libevent 2.0以上版本。
我这里安装的是[tmux 1.9a](http://downloads.sourceforge.net/tmux/tmux-1.9a.tar.gz)，[libevent 2.0.21](https://github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz)。

## 配置使用gtest

    $ tar -zxvf libevent-2.0.21-stable.tar.gz
    $ cd libevent-2.0.21-stable/
    $ ./configure
    $ make clean && make && sudo make install

