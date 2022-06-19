---
title: 脚本分辨率
menu:
  docs:
    parent: miscellaneous
weight: 7400
---

ASS字幕文件，在某种程度上可以独立于视频文件使用。
这就需要使用虚拟视频分辨率来控制字体大小和坐标，这个分辨率这就是"脚本分辨率"。
不幸的是，由于VSFilter的一些BUG，渲染和视频分辨率不同的字幕脚本时这是不可靠的。

由于脚本分辨率和视频分辨率匹配不佳会导致意料之外的问题，所以我们建议做复杂样式字幕时，脚本分辨率和视频分辨率保持统一。
如果你要发布一个字幕文件，同时对应多个分辨率的片源，
[重设分辨率]({{< relref "Resolution_Resampler" >}})
可以被用来转化脚本分辨率以适配每一个视频。但是，如果你制作的字幕文件样式并不复杂，你便不用担心这个问题。

______________________________________________________________________

下面几类设置会被脚本分辨率影响:

绝对坐标 (边距, `\pos`, `\move`, `\clip`, 矢量绘图代码):
所有的绝对坐标都按照脚本分辨率像素计算。

字号：ASS中的字体大小按照脚本分辨率的 *行高度* 像素值进行计算。
注意这个定义和一般的字号定义是不同的，并且和宽度没有半毛钱关系。
结果就是，面对一个变形视频时，脚本分辨率无法被用来调整字幕的横纵比。

边框大小，阴影距离，模糊强度：这些都会受到脚本分辨率和视频分辨率像素数值的影响。
这几项在文件头部(脚本配置)中 比例缩放边框和阴影
区域可以控制。如果打√，则会使用脚本分辨率，反之则使用视频分辨率。
建议勾选。

### 更改脚本分辨率

脚本分辨率可以简单地在 [脚本配置]({{< relref "Properties" >}})
对话框中修改，或者使用 [重设分辨率]({{< relref "Resolution_Resampler" >}})
工具。 用哪个取决于你缘何修改脚本分辨率。
如果你目前有一个未设置样式的字幕脚本，这个脚本因为某些原因分辨率设置错误，这种情况下使用
[脚本配置]({{< relref "Properties" >}}) 进行修改。
如果脚本已经具有样式，并且你现在想做的就是把这个脚本匹配到另外一个分辨率的视频上，这时候就应该使用
[重设分辨率]({{< relref "Resolution_Resampler" >}})。

### YCbCr Matrix

你可以把这部分整体忽略，除非你需要把脚本中的颜色精确地和视频中的颜色进行匹配
(例如：你使用矢量图遮挡原字幕，还想保证绘图颜色和背景色一致，以免产生违和感)。

在 ASS
中，颜色的16进制代码是按照BGR值，但是视频所采用的色彩空间一般都是YCbCr，这两种色彩空间之间有几种转换方法。
在某些环境下，字幕渲染器需要知道Aegisub使用了哪一种色彩空间，这样才能和在Aegisub中编辑字幕时显示颜色保持一致。
最近版本的Aegisub都会自动正确设置，但是如果你手头的脚本是用3.1以前的版本创建的，你需要手动调整。

### 自动改变分辨率

当打开一个和当前脚本分辨率不同的视频时，默认情况下Aegisub会询问你想进行什么操作。

如果你的视频和脚本的横纵比一样，你会看到下面的对话框：

![resolution-mismatch](/img/3.2/resolution-mismatch.png#center)

"重设脚本分辨率" 按照 [重设分辨率]({{< relref "Resolution_Resampler" >}})
一样的方式重设脚本分辨率，如果你刷新一下字幕，你会发现看起来一切都是那么正常。

"直接设为视频分辨率" 会简单地直接把脚本分辨率设成视频分辨率。
如果这个脚本事实上原来匹配这个分辨率的视频，但被某人改坏了，这样做是正确的。

取消或者按ESC不会更改脚本分辨率，还是原来的值。
如果你在打开一个低分辨率的打轴用视频，而脚本中的样式已经和最终将要发布的视频匹配，就按这个方法做。

______________________________________________________________________

如果视频的横纵比和脚本的不符，见下面的对话框(脚本分辨率)：

![resolution-mismatch-ar](/img/3.2/resolution-mismatch-ar.png#center)

有几种常见的情况，这些情况下横纵比会被改变：

- 新视频是变形分辨率，而字幕脚本是在非变形分辨率的视频下制作的(或者反过来)。
  这种情况典型地发生在DVD影像中，一个实际分辨率720x480的影像在播放时被拉伸到640x480
  或853x480。
  这时就需要把字幕也拉伸到一个新的横纵比，因为在变形视频上渲染字幕时会产生变形。

- 原视频只含有内容部分而新视频有黑边。\
  可选地，原视频是扫描(非完整)(pan-and-scan)版本，而新视频含有完整图像。这种情况下，选择"适应视频"(增加黑边)

- 无论是原视频有黑边还是新视频没有黑边，抑或新视频是pan-and-scan版本。都选择"铺满视频"(remove
  borders)。