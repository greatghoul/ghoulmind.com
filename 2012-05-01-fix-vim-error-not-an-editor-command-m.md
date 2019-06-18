---
layout: post
title: "Not an editor command: ^M"
slug: fix-vim-error-not-an-editor-command-m
date: 2012-05-01 22:48
tags: [vim]
---

五一假期在家里修改了 `.vimrc` 的配置,结果回到租住的地方同步配置后启动 vim 就报错了。

    E492: Not an editor command: ^M

原因是在家里用的是 xp 的系统，放在 linux 下换行符不一致导致不兼容了。

修复的方法吗,自然是用 `dos2unix` 了

如果系统中没有的话，可能需要先安装一下。

    $ sudo pacman -S dos2unix
    $ dos2unix .vimrc 
    dos2unix: converting file .vimrc to Unix format ...

大功告成。
