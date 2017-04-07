---
layout: single
title: sublime text开发工具
description: 工作中总结
tags: IDE
---
# 说明
1. sublime text所有的配置，都是通过配置文件来实现的。
2. package control是所有包/模块的管理工具。install pacakage是安装包的。npm本身也作为一个package来管理的。
3. install package正确的打开方式是：敲入这个命令，会打开新的交互窗口，在这个窗口里面，就可以输入你想输入的插件名称。
4. 出现“there is no available package"这种错误，最好是重新安装一下sublime text

# sublime text配置
> sublime text的配置文件包含哪些：通过下面的二级目录进行拆分。

## 代码运行时环境配置
1. javaScript的配置环境
{
    "cmd": ["node", "$file"],
    "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
    "working_dir": "${file_path}",
    "selector": "source.js",
    "shell": true,
    "encoding": "utf-8",
    "windows": {
        "cmd": ["node", "$file"]
    },
    "linux": {
        "cmd": ["killall node; node", "$file"]
    }
}
2.  
