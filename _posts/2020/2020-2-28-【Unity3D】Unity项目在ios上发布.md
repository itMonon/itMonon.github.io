---
layout: post
category: Unity3D-Daily
title:【Unity3D】Unity项目在ios上发布
tagline: by 恬静的小魔龙
tag: Unity3D
---


<p><span style="font-family:Microsoft YaHei; font-size:18px">Unity3d项目要想在ios上发布，必须要有开发者账号，如果想在app store里发布，必须要花钱，这里我只是自己调试，做学习用。</span></p>
<p><span style="font-family:Microsoft YaHei; font-size:18px">要在真机上调试，首先你必须用iphone连接mac，我用的Unity版本是unity5.5，下面是我的发布过程，遇到了一些麻烦，所以写下来给大家做个参考。</span></p>
<p></p>
<div><span style="font-family:Microsoft YaHei; font-size:18px">1.用iphone连接上mac，发布必须要有发布ios的支持包，如果没有，要重新下载并安装，一开始我安装了之后打开的是复制到launchpad里面的app，所以还是没有ios发布这一项，只要打开原来的那个app就行了。然后点发布并运行，失败，直接点发布，发布为一个工程，用xcode打开。</span></div>
<div><span style="font-family:Microsoft YaHei; font-size:18px"><img src="http://img.blog.csdn.net/20170124145434352?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjYwMTA0OTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt=""><br>
</span></div>
<div><span style="font-family:Microsoft YaHei; font-size:18px"><img src="file:///C:/Users/ADMINI~1/AppData/Local/Temp/enhtmlclip/Image.png" alt="" style="height:auto"></span></div>
<div><span style="font-family:Microsoft YaHei; font-size:18px"><br>
</span></div>
<div><span style="font-family:Microsoft YaHei; font-size:18px">2.然后运行，报如下错误：</span></div>
<div><span style="font-family:Microsoft YaHei; font-size:18px"><span style="white-space:pre"></span>Couldn't find developer disk image</span></div>
<div><span style="font-family:Microsoft YaHei; font-size:18px">这是因为xcode版本的问题，下载安装最新版本的xcode和系统兼容，然后再运行，报如下错误：</span></div>
<div style="outline:none; padding:0px">
<div><span style="font-family:Microsoft YaHei; font-size:18px">code signing is required for product type 'xxxxx' in SDK 'iOS 10.2’</span></div>
<div><span style="font-family:Microsoft YaHei; font-size:18px"><span style="white-space:pre"></span>这是因为项目的签名和开发者账号不一致，点击项目-Genernal,把Bundle Identifier改为你自己的开发者账号即可。</span></div>
<div><span style="font-family:Microsoft YaHei; font-size:18px"><img src="http://img.blog.csdn.net/20170124145441510?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjYwMTA0OTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt=""></span></div>
<div><span style="font-family:Microsoft YaHei; font-size:18px">如果还有错误，那么参考这篇文章：<a target="_blank" target="_blank" href="http://www.cnblogs.com/losedMemory/p/6062968.html">点击打开链接</a></span></div>
<div><br>
</div>
</div>
<p><span style="font-family:Microsoft YaHei; font-size:18px">3.在手机上通用-设备，信任你自己的开发者账号即可。</span></p>
   
</div>







      



            
                                    
            
                                    








               



