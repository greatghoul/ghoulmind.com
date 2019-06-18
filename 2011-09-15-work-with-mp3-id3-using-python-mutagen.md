---
layout: post
title: python-mutagen操作MP3的ID3信息
slug: work-with-mp3-id3-using-python-mutagen
date: 2011-09-15 01:11
tags: [mutagen, python]
---

python 的 mutagen 库提供了便捷的 mp3 处理功能，操作 ID3 就像操作 dict 一样方便。

值得一提的是，mutagen 的信息都是采用的 unicode，这样在处理不同语言的 ID3信息就很方便了。同事每个字段都是支持多值的。

官方对于两个特性的描述：

> **Unicode** <http://code.google.com/p/mutagen/wiki/Tutorial#Unicode>
> 
> Mutagen has full Unicode support for all formats. When you assign text strings, we strongly recommend using Python 
> unicode objects rather than str objects. If you use str objects, Mutagen will assume they are in UTF-8.
> 
> (This does not apply to strings that must be interpreted as bytes, for example filenames. Those should be passed as 
> str objectss, and will remain str objects within Mutagen.)
> 
> **Multiple Values**
> 
> Most tag formats support multiple values for each key, so when you access then (e.g. audio["title"]) you will get 
> a list of strings rather than a single one ([u"An example"] rather than u"An example"). Similarly, you can assign 
> a list of strings rather than a single one.

下面是一个读写 ID3 信息的例子：

    #!/usr/bin/env python
    #-*- coding:utf-8 -*-

    from mutagen.mp3 import MP3
    import mutagen.id3
    from mutagen.easyid3 import EasyID3

    def print_id3(id3):
        print
        print '%-13s\t%s' % ('Field', 'Value')
        print '-' * 70
        for k, v in id3info.items():
            print '%-13s\t%s' % (k, v and v[0])

    if __name__ == '__main__':
        id3info = MP3(u'/home/greatghoul/Musics/刘若英/听说/It Just Be - 刘若英.mp3', ID3=EasyID3)
        print_id3(id3info)

        # change the title.
        old_title = id3info['title']
        id3info['title'] = u'new title'
        id3info.save()
        print_id3(id3info)

        # change the title back.
        id3info['title'] = old_title
        id3info.save()
        print_id3(id3info)</pre>

输出结果：

    Field        	Value
    ----------------------------------------------------------------------
    date         	2004-10-13 00:00:00
    tracknumber  	5
    album        	听说
    genre        	华语流行音乐【Chinese Pop Music】,流行音乐【Pop Music】
    artist       	刘若英
    title        	It Must Be

    Field        	Value
    ----------------------------------------------------------------------
    date         	2004-10-13 00:00:00
    tracknumber  	5
    album        	听说
    genre        	华语流行音乐【Chinese Pop Music】,流行音乐【Pop Music】
    artist       	刘若英
    title        	new title

    Field        	Value
    ----------------------------------------------------------------------
    date         	2004-10-13 00:00:00
    tracknumber  	5
    album        	听说
    genre        	华语流行音乐【Chinese Pop Music】,流行音乐【Pop Music】
    artist       	刘若英
    title        	It Must Be

然而奇怪的是，mutagen 用这种方法并无法读取出 ID3 的 comment 字段，这首mp3应该还有一个 comment 字段
**WWW.TOP100.CN 巨鲸音乐网**

**muagen 项目主页：** <http://code.google.com/p/mutagen/>

