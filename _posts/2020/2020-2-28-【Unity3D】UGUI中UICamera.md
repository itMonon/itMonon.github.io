---
layout: post
category: Unity3D-Daily
title: 【Unity3D】UGUI中UICamera
tagline: by 恬静的小魔龙
tag: Unity3D
---


<!DOCTYPE html>
<html>
<head>
</head>
<body>
<script id="toolbar-tpl-scriptId" prod="download" skin="black" src="http://c.csdnimg.cn/public/common/toolbar/js/content_toolbar.js?v=5.00.37" type="text/javascript" domain="http://blog.csdn.net"></script>

<div class="container clearfix">
    <main>
        <article>
            <h1 class="csdn_top">Unity中UICamera设置</h1>
            <div id="article_content" class="article_content csdn-tracking-statistics" data-mod="popu_519" data-dsm="post">
                <p>默认状态下创建的UGUI的原点和世界坐标的原点是重合的，这样其实非常不方便。</p><p style="text-align: center;"><img src="http://img.blog.csdn.net/20160810004514163" alt="" /><br /></p><p><br /></p><p>解决方法就是创建一个Camera专门用于绘制UI。具体步骤如下：</p><p>1.新建一个Camera，参数如下</p><p style="text-align: center;"><img src="http://img.blog.csdn.net/20160810011237128" alt="" /><br /></p><p><br /></p><p>尽量拉到一个比较远的位置</p><p><br /></p><p>Canvas需要设置一下Canvas Component</p><p style="text-align: center;"><img src="http://img.blog.csdn.net/20160810011543429" alt="" /><br /></p><p><br /></p><p>这下UI就不会和场景中的物体有重合了。</p><p><br /></p><link rel="stylesheet" href="http://static.blog.csdn.net/public/res-min/markdown_views.css?v=2.0" />
            </div>

</html>

