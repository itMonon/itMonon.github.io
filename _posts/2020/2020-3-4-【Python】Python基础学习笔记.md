---
layout: post
category: web
title: 【Python】Python基础学习笔记
tagline: by 恬静的小魔龙
tag: Other
---

## 一、前言
最近对Python比较感兴趣，就去学习了一下，发现跟C#包括C都有很多相似的地方，但是也有一些不一样的地方，Python语言真的是非常的简洁，下面就将我这几天学习的内容总结一下。随时扩充

## 二、基本介绍
> Python是一种计算机程序设计语言。是一种动态的、面向对象的脚本语言，最初被设计用于编写自动化脚本(shell)，随着版本的不断更新和语言新功能的添加，越来越多被用于独立的、大型项目的开发。
> --------百度百科
不仅如此，它还是编程语言界里的新晋网红。
它火到什么程度？——现在，你可以在学校的各路考试中见到Python的身影，企业正在高薪招聘Python人才，甚至，越来越多非技术岗的人也开始学习Python了。

Python的应用方向也非常的广泛，包括数据分析方向的数据建模、数据挖掘、数据可视化、商业分析，数据采集方面的网络爬虫，人工智能方向的机器学习、计算机视觉、无人驾驶，编程开发方向的WEB开发、GUI编程、游戏开发、运维开发，还有工具脚本方向的自动化控制和批量处理。
> 无论如何，想要自由地选择接下来的应用方向，前提就是要先把Python的基础给打扎实。

顺便说一下我学习的途径，主要是通过风变编程
https://www.pypypy.cn，非常棒，非常有想法的一种学习编程的方式
然后介绍就不过多说了，省的觉得我是发广告的，大家自己去体验吧。

## 三、正文
### 1、单向编程
#### 1.1 print()函数
示例：
```python
print('Hello,World')
print("Hello,World")
```
#### 1.2 转义字符
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190328164355856.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
#### 1.3 变量和赋值

```python
name="张三"
```
### 2、沟通语言
#### 2.1 数据类型
字符串
```python
name="张三"
```
整数
```python
a = 10
```
浮点数

```python
a = 3.14159
```
#### 2.2 数据拼接
```python
name1 = '张三'
name2= '李四'
print(name1+name2)
```
type()函数
判断数据类型

```python
name1='张三'
print(type(name1))
```

#### 2.3 数据转换
负责数据转换的函数 转成字符串格式str() 转成整数格式int() 转成浮点数格式float()
```python
name1 = '张三'
age = 18
print(name1+str(age))
```
### 3、沟通逻辑
#### 3.1 条件判断
---
单向判断 if
```python
age = 19
if age>18:
	print('成年了')
```
---
双向判断
if... else...
如何if不成立，就执行else的指令
```python
age = 19
if age>18:
	print('成年了')
else :
	print('未成年')
```
这里有个坑，就是else后面不能跟判断语句，否则会报错
if 跟 else 还需要对齐，不然还是会报错

---
多向判断
if...elif...else
```python
age = 19
if 60>age>18:
	print('成年了')
elif age>=60:
	print('花甲了')
else
	print('未成年')
```
这个有一点需要注意，就是如果一直要加判断你的话可以用if...elif...elif可以不用加else 
#### 3.2 if嵌套
```python
age = 19
if age>18:
	if age > 30:
		print('中年人')
	else:
		print('成年了')
elif age<=18:
	if age < 6:
		print('小bb')
	else:
		print('花甲了')
else
	print('老大爷')
```
这里注意一点，同一组的if...else语句需要对齐
### 4、互动沟通

#### 4.1 Input()函数
input()函数的使用
```python
input('请输入你的名字：')
```
主要作用是接收控制台输入的数值

---
input()函数的赋值
```python
name = input('请输入你的名字：')
```

--- 
input()函数的数据类型
input()接收到的数据类型为字符串类型

---
input()函数的强制转换
```python
age=10;
name = int(input('请输入你的年龄：'))
if age>18:
	print('成年了')
else:
	print('未成年')
```

