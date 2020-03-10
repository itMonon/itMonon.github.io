---
layout: post
category: Unity3D-Daily
title: 【Unity3D】游戏无法部署到windows phone8手机上的解决方法
tagline: by 恬静的小魔龙
tag: Unity3D
---

<p>今天搞了个unity3d游戏，准备部署到自己的lumia 920上，数据线连接正常，操作正常，但是“build”以后，始终无法部署到手机上，也没有在选择的目录下生产任何相关文件。（你的系统必须是win8）</p>

<p>但是提示有一个错误：</p>

<p><span style="color:#ff0000;"> Error building Player: Exception: Error: method `System.Byte[] System.IO.File::ReadAllBytes(System.String)` doesn't exist in target framework. It is referenced from Assembly-CSharp.dll at System.Byte[] NGUITools::Load(System.String).</span></p>

<p> </p>

<p>意思是NGUITools.cs里面的Load（）方法有问题，导致无法部署。</p>

<p>解决方案：找到NGUITools.cs，找到Load（）方法。代码如下：</p>

<p><a href="http://www.cnblogs.com/zhibolife/p/3715247.html#">?</a></p>

<table border="1" cellpadding="2" cellspacing="0"><tbody><tr><td style="border-color:#999999;">
			<p>1</p>

			<p>2</p>

			<p>3</p>

			<p>4</p>

			<p>5</p>

			<p>6</p>

			<p>7</p>

			<p>8</p>

			<p>9</p>

			<p>10</p>

			<p>11</p>

			<p>12</p>

			<p>13</p>

			<p>14</p>

			<p>15</p>

			<p>16</p>

			<p>17</p>

			<p>18</p>

			<p>19</p>

			<p>20</p>
			</td>
			<td style="border-color:#999999;">
			<p><code>    </code><code>/// &lt;summary&gt;</code></p>

			<p><code>    </code><code>/// Load all binary data from the specified file.</code></p>

			<p><code>    </code><code>/// &lt;/summary&gt;</code></p>

			<p> </p>

			<p><code>    </code><code>static</code><code>public</code><code>byte</code><code>[] Load (</code><code>string</code><code>fileName)</code></p>

			<p><code>    </code><code>{</code></p>

			<p><code>#if UNITY_WEBPLAYER || UNITY_FLASH</code></p>

			<p><code>        </code><code>return</code><code>null</code><code>;</code></p>

			<p><code>#else</code></p>

			<p><code>        </code><code>if</code><code>(!NGUITools.fileAccess) </code><code>return</code><code>null</code><code>;</code></p>

			<p> </p>

			<p><code>        </code><code>string</code><code>path = Application.persistentDataPath + </code><code>"/"</code><code>+ fileName;</code></p>

			<p> </p>

			<p><code>        </code><code>if</code><code>(File.Exists(path))</code></p>

			<p><code>        </code><code>{</code></p>

			<p><code>            </code><code>return</code><code>File.ReadAllBytes(path);</code></p>

			<p><code>        </code><code>}</code></p>

			<p><code>        </code><code>return</code><code>null</code><code>;</code></p>

			<p><code>#endif</code></p>

			<p><code>    </code><code>}</code></p>
			</td>
		</tr></tbody></table><p>　　只要在#if UNITY_WEBPLAYER || UNITY_FLASH后面加个“UNITY_WP8”就可以了。</p>

<p>完整代码：</p>

<p><a href="http://www.cnblogs.com/zhibolife/p/3715247.html#">?</a></p>

<table border="1" cellpadding="2" cellspacing="0"><tbody><tr><td style="border-color:#999999;">
			<p>1</p>

			<p>2</p>

			<p>3</p>

			<p>4</p>

			<p>5</p>

			<p>6</p>

			<p>7</p>

			<p>8</p>

			<p>9</p>

			<p>10</p>

			<p>11</p>

			<p>12</p>

			<p>13</p>

			<p>14</p>

			<p>15</p>

			<p>16</p>

			<p>17</p>

			<p>18</p>

			<p>19</p>

			<p>20</p>
			</td>
			<td style="border-color:#999999;">
			<p><code>    </code><code>/// &lt;summary&gt;</code></p>

			<p><code>    </code><code>/// Load all binary data from the specified file.</code></p>

			<p><code>    </code><code>/// &lt;/summary&gt;</code></p>

			<p> </p>

			<p><code>    </code><code>static</code><code>public</code><code>byte</code><code>[] Load (</code><code>string</code><code>fileName)</code></p>

			<p><code>    </code><code>{</code></p>

			<p><code>#if UNITY_WEBPLAYER || UNITY_FLASH||UNITY_WP8</code></p>

			<p><code>        </code><code>return</code><code>null</code><code>;</code></p>

			<p><code>#else</code></p>

			<p><code>        </code><code>if</code><code>(!NGUITools.fileAccess) </code><code>return</code><code>null</code><code>;</code></p>

			<p> </p>

			<p><code>        </code><code>string</code><code>path = Application.persistentDataPath + </code><code>"/"</code><code>+ fileName;</code></p>

			<p> </p>

			<p><code>        </code><code>if</code><code>(File.Exists(path))</code></p>

			<p><code>        </code><code>{</code></p>

			<p><code>            </code><code>return</code><code>File.ReadAllBytes(path);</code></p>

			<p><code>        </code><code>}</code></p>

			<p><code>        </code><code>return</code><code>null</code><code>;</code></p>

			<p><code>#endif</code></p>

			<p><code>    </code><code>}</code></p>
			</td>
		</tr></tbody></table><p>　　之后，按照部署步骤，Build以后就可以看到游戏安装到手机上了。</p>

<p> </p>

<p>参考资料：</p>

<p>http://www.tasharen.com/forum/index.php?topic=6625.0</p>

<p>unity3d部署到wp手机：</p>

<p>http://game.ceeger.com/Manual/wp8-deployment.html</p>

<p> </p>
