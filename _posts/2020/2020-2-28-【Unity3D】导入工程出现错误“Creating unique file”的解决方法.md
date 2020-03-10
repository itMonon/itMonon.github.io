---
layout: post
category: Unity3D-Daily
title: 【Unity3D】导入工程出现错误“Creating unique file”的解决方法
tagline: by 恬静的小魔龙
tag: Unity3D
---

<p>Unity3d导入工程出现错误“<span style="color:#ff0000;">Creating unique file：creating file Temp/tempFile failed.Please ensure there is enough disk space and you have permissions setup correctly</span>”。</p>

<p>解决方法：<span style="color:#ff0000;">路径中有中文字符，把中文字符改成英文就可以了。</span></p>

<p>因路径路径有中文而出错的现象很多，如果出现导入错误，不妨看看路径是否有问题。</p>

<p> </p>

<p><img alt="" class="has" src="https://img-blog.csdn.net/20170925233315890?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcTc2NDQyNDU2Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" /></p>

<p> </p>

<p>有时候导入插件出现错误：“<span style="color:#ff0000;">Error while importing package: Couldn't decompress package</span>”（无法解压压缩包），也是因为路径里有中文造成的，要注意下。</p>

<p> </p>

<p><span style="color:#ff0000;">“Error while importing package: Couldn't decompress package”同样也可能是中文路径，或者导入的包与现有文件有相同的命名冲突，比如文件夹名。</span></p>
