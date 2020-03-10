---
layout: post
category: Unity3D-Daily
title: 【Unity3D】设置Image中的Color的值
tagline: by 恬静的小魔龙
tag: Unity3D
---

首先给Button的Image组件的Color设置一个RGBA的值，值是129 69 69 255。

然后发现达不到效果

最后研究发现要除255

也就是

```csharp
Color s = new Color((129 / 255)f, (69 / 255)f, (69 / 255)f, (255 / 255)f);
```

搞定了~
