---
layout: 'post'
title: 'win7 删除以点结尾的文件夹名时提示找不到文件'
date: 2019-03-06 9:45:50 +UTC800
categories: [windows]
---

今天在解压文件夹时，不小心把文件夹名后边加了个点，删除时系统提示`找不到指定的路径`

百度了一下找到一个方法，步骤如下：

1. 新建一个文本文件
2. 在文本文件中输入以下内容：
    ```powershell
    DEL /F /A /Q \\?\%1

    RD /S /Q \\?\%1
    ```
    `\\?\%1` 这个参数不理解，于是搜索了一下

    `\\?\` UNC路径的一个特例。UNC路径就是符合 \\servername\sharename 格式，其中 servername 是服务器名，sharename 是共享资源的名称。?是统配符，表示匹配0个或1个任意字符。使用UNC路径不会捡测路径中的保留字设备名称等，因此可以用这种方法来删除特殊文件或目录

    `%1` 是拖动到bat上的文件路径，针对的是一个文件，如果是多个文件，可以用`%*`
3. 重命名文件名为 提示找不到该文件方法.bat
4. 将要删除的文件或文件夹拖动到 提示找不到该文件方法.bat上即可自动删除
