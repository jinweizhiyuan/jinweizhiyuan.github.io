---
layout: post
title: javascript 查找源文件中的中文字符
date: 2018-09-13 10:33:55 +UTC800
category: [front-end]
---

最近有人问我怎么提取项目中所有源文件的汉字，给出一个参考[网址](https://blog.csdn.net/barrydiu/article/details/2414717)，看了一下，借助的是wscript宿主脚本，可行然后调试修改了一下，代码如下：
{% highlight javascript linenos %}
/**
* @Author : Barry Diu  2008-05-08
* 找出目录下的php,js,htm文件中的中文字符的位置
*  usage : wscript find_chinese.js  outputpath sourcefolder1 sourcefolder2 sourcefolder3 ...
*  如果没有参数默认 搜索当前目录并把结果输出到当前目录的find_chinese.txt文件中
*  eg.   wscript find_chinese.js
*          wscript find_chinese.js   find.txt  C:ProjectAutobildwork runkmod  
*/

var ForReading = 1,
    ForWriting = 2; // FSO的常量，不要改动
var fso, f1, fldr, foldpath, outputfolderpath, outputfile;

var searchFileTypeArr = new Array('php', 'js', 'htm', 'html', 'txt'); // 要查找的文件类型的扩展名, 跟据你的需要修改

/*  显示参数
if(WScript.Arguments.length>0){
    for(i=0; i<WScript.Arguments.length; i++){
        WScript.Echo( i + " : "  +  WScript.Arguments(i) );
    }
}
*/

fso = new ActiveXObject("Scripting.FileSystemObject");

foldpath = new Array('.');
outputfolderpath = '';

if (WScript.Arguments.length > 0) {
    outputfolderpath = WScript.Arguments(0);
}

if (WScript.Arguments.length > 1) {
    foldpath = new Array();
    for (i = 1; i < WScript.Arguments.length; i++) {
        foldpath[i - 1] = WScript.Arguments(i);
    }
}

for (i = 0; i < foldpath.length; i++) {
    if (!fso.FolderExists(foldpath[i])) {
        WScript.Echo("folder is not exist!!!");
    }
}

fldr = fso.GetFolder(foldpath);

//WScript.Echo(fldr);

if (outputfolderpath != '') {
    outputfile = outputfolderpath;
} else {
    outputfile = "find_chinese.txt";
}

f1 = fso.createtextfile(outputfile, true);

var starttime = new Date();

f1.WriteLine(starttime.toString() + " : Starting to search chinese characters in \" " + fldr + "\" .....");
f1.WriteBlankLines(2);

for (i = 0; i < foldpath.length; i++) {
    iterate(foldpath[i]);
}

f1.WriteBlankLines(2);
var endtime = new Date();
f1.WriteLine(endtime.toString() + " : Search chinese characters finish");


// 递归循环列出目录下的文件和子目录下的文件
function iterate(path) {
    var folder, folders, files, file, fileExtName, fileTypeIsCorrect;
    folder = fso.GetFolder(path);

    // check files
    files = new Enumerator(folder.files);
    for (; !files.atEnd(); files.moveNext()) {
        // 过滤文件类型
        fileTypeIsCorrect = false;
        fileExtName = getFileExtendName(files.item().Name);
        for (i = 0; i < searchFileTypeArr.length; i++) {
            if (fileExtName == searchFileTypeArr[i]) {
                fileTypeIsCorrect = true;
            }
        }

        if (!fileTypeIsCorrect) {
            continue;
        }

        // 如果是自己则跳过
        if (files.item() != WScript.ScriptFullName && files.item() != fso.getFile(outputfile)) {
            // 查找中文字符
            checkChineseChar(files.item());
        }

    }

    // 递归查找子目录 check subfolders
    folders = new Enumerator(folder.SubFolders);
    for (; !folders.atEnd(); folders.moveNext()) {
        iterate(folders.item());
    }
}
// 查找中文字符
function checkChineseChar(targetFile) {
    var pattern;
    pattern = /([\u4E00-\u9FA5]|[\uFE30-\uFFA0])+/gim; // 中文 ;  判断使用    pattern.test(content)
    //pattern = /[/x00-/xff]/gi;                                   // 英文以外的文字, 判断使用         !pattern.test(content)
    var content;
    var output = '';
    var find = false;
    var line = 1;
    var rfile = fso.OpenTextFile(targetFile, ForReading);
    var matchResult;
    
    var objStream = new ActiveXObject("adodb.stream");
    objStream.Charset = "utf-8";
    objStream.Type = 2; // 默认
    objStream.Open();
    objStream.loadFromFile(targetFile);
    
    while(!objStream.EOS) {
        var lineText = objStream.ReadText(-2);
        var matchText = lineText.match(pattern);
        if (matchText) matchText = matchText.join(" <> ");
        if (matchText) { // 注意 ! 的 有/无 !
        // WScript.Echo(content);
            find = true;
            output += "     #line " + line + " ; " + matchText + "\r\n";
        }
    }

    /* while (!rfile.AtEndOfStream) {
        content = rfile.ReadLine();
        if (matchResult = content.match(pattern)) { // 注意 ! 的 有/无 !
            find = true;
            output += "     #line " + line + " ; " + matchResult.join(" ") + "\r\n";
        }
        line++;
    } */

    if (find) {
        f1.WriteLine(targetFile);
        f1.WriteLine(output);
        f1.WriteBlankLines(1);
    }
}

// 获取文件扩展名
function getFileExtendName(filename) {
    var length = filename.length;
    var charindex = filename.lastIndexOf(".");
    var extname = '';
    if (charindex > 0) {
        extname = filename.substring(charindex + 1, length);
    }
    return extname.toLowerCase();
}
{% endhighlight %}