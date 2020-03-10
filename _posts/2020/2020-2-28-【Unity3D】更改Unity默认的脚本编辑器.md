---
layout: post
category: Unity3D-Daily
title: 【Unity3D】更改Unity默认的脚本编辑器
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
尽管Unity有一个像样的脚本编辑器(Mono)，但很多人喜欢使用另一个编辑器。这篇短文解释了如何更改脚本编辑器，并介绍了Mono的一些替代方案。

## 二、默认脚本编辑器：mono
如果您想知道脚本编辑器是什么：在双击脚本时会打开它。Unity附带的默认脚本编辑器是Mono:
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS9zY3JpcHQtZWRpdG9yLWNoYW5nZS9tb25vLnBuZw)
## 三、更改脚本编辑器
如果我们想让Unity使用不同的脚本编辑器，我们所要做的就是在顶部菜单Editor中，选择Preferences然后选择External Tools:
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS9zY3JpcHQtZWRpdG9yLWNoYW5nZS9leHRlcm5hbF90b29scy5wbmc)
在这里，我们可以选择一个自动检测到的编辑器。选择编辑器后，试着保存场景并重新启动Unity，直到它正常工作(有时仍然是错误的)。

注意：如果编辑器不在列表中，只需选择浏览.。并手动查找编辑器的.exe文件。

## 四、VisualStudio脚本编辑器
大多数使用Windows操作系统的程序员通常都安装了VisualStudio，这种情况应该由Unity自动检测。VisualStudio是Mono的一个不错的替代方案。它工作得很好，有不错的语法高亮显示和许多定制选项，比如自动完成。

**优点**
Unity VisualStudio支持的伟大之处在于代码帮助工具(有时称为智能提示)。所以如果你写的是“GUI“，在VisualStudio中，它将自动显示所有统一GUI函数和变量的小窗口。

**缺点**
VisualStudio的缺点是它不能正确突出Javascript。此外，在双击脚本时，Unity有时仍然很难正确地打开VisualStudio，但总有一天会修复的。

## 五、记事本+脚本编辑器
如果您喜欢简单，记事本+文本编辑器是一个很好的选择。基本上，它是一个轻量级的文本编辑器，语法突出显示，这并不糟糕。除此之外，它还提供了一些不错的功能，如单词计数或将制表符转换为空格等。


**优点**
Notepad+的伟大之处在于它可以与Unity目前支持的所有脚本语言一起工作。将自动检测到C#和Javascript，如果您正在使用Boo，请尝试转到语言菜单和选择Python，这将突出显示语法足够好。

另一个优点是，每次双击联合中的脚本时，它都能正常工作。它总是在任何时候打开记事本+，并正确地显示脚本。

**缺点**
记事本+的唯一缺点是这个小代码帮助窗口并不完美。它可以在Settings->Preferences->Backup/Auto-Completion->Enable自动完成时启用，但它不会向您显示VisualStudio或Mono在输入以下内容时显示的所有函数GUI。或者其他任何特定于Unity的功能。

