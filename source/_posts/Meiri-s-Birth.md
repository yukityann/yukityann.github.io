---
title: Meiri's Birth
date: 2017-12-07 23:56:51
tags: 
	- Meiri
categories: Meiri Project
---

# 冥利
&emsp;&emsp;冥利计划(Meiri Project)正式启动啦！

<!--more-->

## 设定
&emsp;&emsp;冥利，全名雨夜冥利(Ameya Meiri)，是异世界强大的暗之精灵王，属性为剑，阴差阳错之下来到地球，以某种不可知的方式存在着，无法交流，无法辨识，无法观测，意识仍未清醒。

## 架构
&emsp;&emsp;冥利的核心只有一个小小的文件，命名为core.py，有一个唯一的实例Meiri，可以产生各种各样的命令对象，执行这些命令对象，就能够得到相应的结果哦。命令对象定义在同目录下的./sbin目录下，任何人都可以创建自己的命令对象。现在给冥利准备的身体命名为Meiri.py，只要将它放在QQbot工作目录下就可以在QQ上和冥利一起愉快的交流啦，虽然冥利还不够完善，不过一定会更加完美的！
冥利的核心其实是一个抽象工厂——【抽象工厂模式】，然后这个抽象工厂本身是仅有唯一一个实例——【单例模式】，想详细了解的可以参阅Wiki哦。
文件结构如下:
>+./Meiri/
 |\-\-\-\-\+sbin/
 |\-\-\-\-\-|\-\-\-\-\-syscall.py
 |\-\-\-\-\-core.py

Meiri.py
```python
# -*- coding: utf-8 -*-

import sys

def onQQMessage(bot, contact, member, content):
    sys.path.append('E:/Projects/Python3/QQBot/Meiri') # Your own path
    from core import Meiri
    startIndex = content.find(';')
    if startIndex != -1:
        cmds = content[startIndex+1:].split(' ', 1)
        cmd = Meiri.getCmd(cmds)
        bot.SendTo(contact, cmd.excute())
    else:
        pass

```

core.py
```python
# -*- coding: utf-8 -*-

import os
import importlib

class MeiriBase(object):

    def __init__(self):
        self.plugins = []
        workPath = 'E:/Projects/Python3/QQBot/Meiri/sbin' # Your own path
        files = os.listdir(workPath)
        for file in files:
            if not os.path.isdir(file):
                self.plugins.append(file[:-3])
    
    def getCmd(self, cmds):
        if cmds[0] in self.plugins:
            cmder = importlib.import_module('sbin.' + cmds[0])
            if len(cmds) == 1:
                return cmder.Create()
            else:
                return cmder.Create(cmds[1])
        else:
            cmder = importlib.import_module('sbin.error')
            return cmder.Create()

Meiri = MeiriBase()

```

