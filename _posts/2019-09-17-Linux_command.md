---
layout: post
title: Linux权限详解
date: 2019-09-12 
tag: Linux
---

# 一、背景
排查问题的时候发现Linux命令用的不是很熟练，且有很多快捷键不熟悉，所以特此记录。

## 二、常用快捷键
- ctrl+左右键:在单词之间跳转
- ctrl+a:跳到本行的行首
- ctrl+e:跳到页尾
- Ctrl+u：删除当前光标前面的文字 （还有剪切功能）
- ctrl+k：删除当前光标后面的文字(还有剪切功能)
- Ctrl+L：进行清屏操作
- Ctrl+y:粘贴Ctrl+u或ctrl+k剪切的内容
- Ctrl+w:删除光标前面的单词的字符
- Alt – d ：由光标位置开始，往右删除单词。往行尾删

## 三、移动光标
- Ctrl – a ：移到行首
- Ctrl – e ：移到行尾
- Ctrl – b ：往回(左)移动一个字符
- Ctrl – f ：往后(右)移动一个字符
- Alt – b ：往回(左)移动一个单词
- Alt – f ：往后(右)移动一个单词
- Ctrl – xx ：在命令行尾和光标之间移动
- M-b ：往回(左)移动一个单词
- M-f ：往后(右)移动一个单词
- 
## 四、控制命令
- Ctrl – l ：清除屏幕，然后，在最上面重新显示目前光标所在的这一行的内容。
- Ctrl – o ：执行当前命令，并选择上一条命令。
- Ctrl – s ：阻止屏幕输出
- Ctrl – q ：允许屏幕输出
- Ctrl – c ：终止命令
- Ctrl – z ：挂起命令