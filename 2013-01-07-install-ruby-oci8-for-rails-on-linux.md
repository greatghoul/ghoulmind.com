---
layout: post
title: 在rails中安装ruby-oci8
slug: install-ruby-oci8-for-rails-on-linux
date: 2013-01-07 11:27
tags: [ruby, oracle, rails]
---

在 rails 中连接 oracle10g 数据库会使用下面两个包

    # Gems for oracle database
    gem 'ruby-oci8', '~> 2.1.4'
    gem 'activerecord-oracle_enhanced-adapter', '~> 1.4.1'

但是直接 ``bundle install`` 会报出以下错误

> An error occurred while installing ruby-oci8

所以需要先安装一些依赖 -- instant-client

## 安装 Instant Client 

访问 Instant Client 下载页面，下载两个包

 * 32bit: <http://www.oracle.com/technetwork/topics/linuxsoft-082809.html>
    - instantclient-basic-linux-11.2.0.3.0.zip 
    - instantclient-sdk-linux-11.2.0.3.0.zip
 * 64bit: <http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html>
    - instantclient-basic-linux.x64-11.2.0.3.0.zip 
    - instantclient-sdk-linux.x64-11.2.0.3.0.zip 

下载完成后，解压文件并移动至合适的目录(以下仅以32位的为例)

    unzip instantclient-basic-linux-11.2.0.3.0.zip
    unzip instantclient-sdk-linux-11.2.0.3.0.zip
    mv instantclient_11_2 /usr/local/instantclient_11_2

下载的步骤并不是所有系统中都需要，但是加上也无妨，它在某些系统中能更准确的定位库文件

    cd /usr/local/instantclient_11_2
    ln -s libclntsh.so.11.1 libclntsh.so
    ln -s libocci.so.11.1 libocci.so

设置环境变量，将 ``LD_LIBRARY_PATH``
    
    export LD_LIBRARY_PATH='/usr/local/instantclient_11_2'

安装 libaio

    sudo apt-get install libaio1 # Debian
    yum -y install libaio libaio-devel # CentOS

这一切做好后，再次安装 Gems

    bundle install

至于 oracle 的连接方法，可以参考下面的资料

 * <http://2muchtea.wordpress.com/2007/12/23/installing-ruby-oci8-on-ubuntu/>
 * <http://database.51cto.com/art/201107/276491.htm>
 * <http://rubydoc.info/gems/activerecord-oracle_enhanced-adapter/frames>
 * <http://ruby-oci8.rubyforge.org/en/>
