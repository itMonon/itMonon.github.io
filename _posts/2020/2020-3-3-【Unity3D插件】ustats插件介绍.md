---
layout: post
category: Unity3D-Plugin
title: 【Unity3D插件】ustats插件介绍
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
ustats插件 - Unity项目统计
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905100527984.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
使用我们的编辑器扩展来度量您的项目统计数据：ustats

## 二、原理
ustats是一个简单的UnitedEditor扩展，它显示了游戏的开发统计信息：

- 编辑游戏花费的时间
- 测试游戏花费的时间
- 有多少行 C#代码
- 有多少行 JavaScripts代码
- 有多少行 BOO代码
- 脚本的总数量
- 纹理的总数量
- 音响的总数量
- 视频的总数量
- 文本的总数量


## 三、使用指南
选择Window->ustats从顶部菜单：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905102642693.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

补充资料：

“花费时间“统计数据依赖于项目，即使在更改计算机或将项目移动到不同的文件夹后，它们仍然保持不变。(这是通过将它们保存在Library/ustats.data中来实现的)

“花费时间“只有在统一工作时统计数据才会增加，而当窗口处于不活动状态或最小化时，统计数据就不会增加。”

