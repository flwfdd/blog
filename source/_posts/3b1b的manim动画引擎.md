---
title: 3b1b的manim动画引擎
date: 2020-02-03 01:09:14
categories: 技
tags:
- 技
- manim
- 视频制作
- 动画
---


官方文档不行...看[这个](https://talkingphysics.wordpress.com/2019/01/08/getting-started-animating-with-manim-and-python-3-7/)不错。

## 基本架构与渲染方式

每一个`.py`文件作为一个素材库，每个素材库中可以定义类`class`，普通模式继承`Scene`，每一个类作为一个素材，其中必须包含一个`construct`函数，渲染时会调用这个函数。其实并不需要了解这些...反正就在那里面写就完了。很多时候艺术创作的感觉大于编程。

渲染命令栗子：`python -m manim test.py Prt -pl`

其中`-`后面的是简写的渲染参数（完整参数是`--`两个杠），可以调用`python -m manim -h`查看更多参数说明，比较常用的有定义清晰度的`-l`、`-m`、`--high_quality`。

## 通用命令

* wait()
`self.wait(second)`
延时，以秒为单位。

* play()
`self.play(something(,something2,……))`
一切效果显示的核心，连续调用的话会等上一个执行完再执行下一条，如果希望多个效果同时执行，那么就需要同时传入同一个函数中（就像上面内层括号中所做的那样）

## 常量

常见的常量有颜色、单位坐标等等，定义在[constants](https://github.com/3b1b/manim/blob/master/manimlib/constants.py)文件中。

## Mobject与基础属性

`manim`中预设的图形叫做`Mobject`（**M**ath **Object**），可以在生成的时候传入参数，如`C=Circle(radius=2,color=RED)`

除了主颜色和宽高需要用`Obj.set_color`、`Obj.set_width`、`Obj.set_height`设置外，其他均可调用`Obj.set_style()`设置（可以同时传入多个参数，如果想要传入`list`可以`self.play(*list)`）。

### Mobject

大部分定义在[这里](https://github.com/3b1b/manim/blob/master/manimlib/mobject/geometry.py)
属性在[这里](https://github.com/3b1b/manim/blob/master/manimlib/mobject/mobject.py)和[这里](https://github.com/3b1b/manim/blob/master/manimlib/mobject/types/vectorized_mobject.py)

* Circle
`Obj=Circle()`
圆形，大小通过`radius`（半径）控制。

* Dot
`Obj=Dot()`
点

* SmallDot
`Obj=SamllDot()`
小点

* Line
`Obj=Lind(start=LEFT, end=RIGHT)`
直线，通过`stroke_width`控制粗细

* DashedLine
`Obj=DashedLind(start=LEFT, end=RIGHT)`
虚线

* Rectangle
`Obj=Rectangle()`
矩形

* Square
`Obj=Square(side_length=2)`
正方形

* RoundedRectangle
`Obj=RoundedRectangle(corner_radius=0.5)`
圆角矩形

* Polygon
`Obj=Polygon(P1,P2,P3,……)`
多边形

* RegularPolygon
`Obj=RegularPolygon(n=6)`
正多边形

* Triangle
`Obj=Triangle()`
三角形

* Ellipse
`Obj=Ellipse()`
椭圆

* Arc
`Obj=Arc(start_angle=0, angle=TAU / 4)`
弧

* ArcBetweenPoints
`Obj=ArcBetweenPoints(start,end,angle=TAU / 4)`
由两点定义的弧

* CurvedArrow
`Obj=CurvedArrow(start,end,angle=TAU / 4)`
由两点定义的弧箭头

* CurvedDoubleArrow
`Obj=CurvedDoubleArrow(start,end,angle=TAU / 4)`
由两点定义的双向弧箭头

* Arrow
`Obj=Arrow(start,end)`
由两点定义的箭头

* Vector
`Obj=Vector(direction=RIGHT)`
向量

* DoubleArrow
`Obj=DoubleArrow(start,end)`
由两点定义的双向箭头

* AnnularSector
`Obj=AnnularSector(inner_radius=1,outer_radius=2)`
部分圆环，由Arc定义而来

* Sector
`Obj=Sector(angle=TAU/4,start_angle=0)`
扇形

* Annulus
`Obj=Annulus(inner_radius=1,outer_radius=2)`
圆环

* TangentLine
`Obj1=TangentLine(Obj2,vmob,alpha)`
切线，`vmob`是一个常数，意义应该是`Obj2`上取点的比例位置。

* Elbow
`Obj=Elbow(width=1,angle=-TAU/6)`
一个直切角

* VGroup
`Obj=VGroup(Obj1,Obj2,……)`
打组

* SurroundingRectangle
`Obj1=SurroundingRectangle(Obj2)`
外框矩形

* SurroundingRectangle
`Obj1=SurroundingRectangle(Obj2)`
背景矩形

* Underline
`Obj1=Underline(Obj2)`
下划线

* VectorizedPoint
`Obj=VectorizedPoint(pos)`
啥都不会显示，可以占位用。

### 基础属性

* 主颜色
`color`：颜色


* 宽度高度
`width`：宽度
`height`：高度

* 填充
`fill_color`：填充颜色
`fill_opacity`：填充透明度

* 描边
`stroke_color`：描边颜色
`stroke_width`：描边宽度
`stroke_opacity`：描边透明度

* 渐变（由白色渐变到定义颜色）
`sheen_factor`：渐变比例
`sheen_direction`：渐变方向（白色的方向，如`UR`就是白色在左上）

### 属性变更
`add_background_rectangle()`：矩形背景
`fade(darkness=0.5)`：变暗
`fade_to(color,alpha)`：变成某透明度颜色
`save_state()`：保存当前状态
`restore()`：还原保存过的状态

### 属性获取

* get_corner()
`Obj.get_corner(pos)`
获取某方向上的边界坐标

## 坐标系统与移动

使用numpy三元数组定义，`np.array(0,0,0)`。

默认定义整个视频的高为8单位，长根据视频比例计算，例如`16:9`的长约为`14.2`。

宏定义了原点`ORIGIN`、`LEFT`、`UR`、`TOP`、`X_AXIS`等等一票基本坐标，详见常量。

因为本质是numpy三元组，所以都支持加减和数乘。

* move_to()
`Obj.move_to(pos)`
将物体移动到给定的绝对坐标

* shift()
`Obj.shift(pos)`
将物体相对于当前位置移动

* next_to()
`Obj1.next_to(Obj2,pos)`
移动到相对于另一个物体（也就是以`Obj2`为原点）的给定坐标

* to_edge()
`Obj.to_edge(pos)`
感觉和`to_corner()`没差啊...
移动到给定向量方向的边界处

* scale()
`Obj.scale(A)`
缩放为`A`倍。

* rotate()
`Obj.rotate(degree)`
逆时针旋转一定角度，角度是由`np.pi`定义的，常量中有`PI`（180°）、`TAUI`（360°）和`DEGREES`（1°）。

* surround()
`Obj1.surround(Obj2)`
自适应坐标和大小包住另一个物体。文档中给`Circle`用，实测`Rectangle`、`Ellipse`也还是能用的。

* stretch()
`Obj.stretch(a,dim)`
向一个方向压缩，`a`是比例，`dim`为1是压缩为横线，0是竖线。

* wag()
`Obj.wag(self, direction=RIGHT, axis=DOWN, wag_factor=1.0)`
拉伸

## 出现动画

* add()
`self.add(Obj)`
直接显示

* ShowCreationCreate()
`self.play(ShowCreation(Obj))`
平滑地画出，效果超爱

* FadeIn()
`self.play(FadeIn(Obj))`
平滑淡入

* GrowFromCenter()
`self.play(GrowFromCenter(Obj))`
从中央缩放出现

## 强调动画

[主要源码](https://github.com/3b1b/manim/blob/master/manimlib/animation/transform.py)

* Transform()
`self.play(Transform(Obj1,Obj2))`
将一个东西非常平滑地转换为另一个

* ApplyMethod()
`self.play(ApplyMethod(Obj.method,set))`
下面的所有都是用这个定义出来的，`Obj.method`是变换函数，`set`是相应的参数，单参数直接传入即可，如果多参数，可以使用字典传入，例如`self.play(ApplyMethod(Obj.set_color,PURPLE))`、`self.play(ApplyMethod(Obj.set_style,{"fill_opacity":0.5}))`

* FadeToColor()
`self.play(FadeToColor(Obj,color))`
渐变为`color`颜色

* ScaleInPlace()
`self.play(ScaleInPlace(Obj,A))`
缩放为原来的`A`倍，如果为`0`就直接向中央缩放消失了。

* Rotate()
`self.play(Roate(degree))`
旋转一定角度

* Rotating()
`self.play(Rotating(Obj))`
旋转


## 文字

由于文字比较纷繁复杂，单独揪出来了。当然，文字既然叫`TextMobject`，终究和其他的`Mobject`是一样的。

整个的转换过程大概是`LaTeX`到`dvi`到`svg`，然后对`svg`进行操作。

在3.7中已经可以非常方便地在常量文件中设置`TEX_USE_CTEX`来开启`CTEX`，就可以使用中文了。

更多地直接学习`LaTeX`即可，只是注意因为`Python`的字符串转义，反斜杠都要双写（或者在字符串前加一个`r`表示不转义如`r"Hello World"`）。

`Text=TextMobject("Hello World")`

可以使用`Text=TexMobject("Hello World")`直接键入公式而无需添加`$$`

可以用逗号隔开字符串，放入同一个函数中，会合并渲染，这可以方便我们染色。

* Write()
`self.play(Write(Text))`
帅到炸裂的出场方式

* set_color_by_gradient
`Text.set_color_by_gradient(RED,GREEN,BLUE)`
设置渐变色

* set_colors_by_radial_gradient
`Text.set_colors_by_radial_gradient(center=None, radius=1, inner_color=WHITE, outer_color=BLACK)`
设置中心渐变

* set_color_by_tex
`Text.set_color_by_tex("text",color)`
包含`text`的字串都会被染色

* set_color_by_tex_to_color_map
`Text.set_color_by_tex_to_color_map({"text":color,……})`
其实和上一个是一样的，批量而已。

* arrange_submobjects
`group.arrange_submobjects(direction,buff)`
自适应排列组内元素

# 坐标系

类需要继承`GraphScene`，配置这个类下的`CONFIG`可以更改坐标系参数。

```
CONFIG = {
    "x_min": -1,
    "x_max": 10,
    "x_axis_width": 9, #屏幕显示宽度
    "x_tick_frequency": 1, #每格宽度
    "x_leftmost_tick": None,  # Change if different from x_min
    "x_labeled_nums": None, #标出坐标的列表，可以用range()生成
    "x_axis_label": "$x$", #轴标志
    "y_min": -1,
    "y_max": 10,
    "y_axis_height": 6,
    "y_tick_frequency": 1,
    "y_bottom_tick": None,  # Change if different from y_min
    "y_labeled_nums": None,
    "y_axis_label": "$y$",
    "axes_color": GREY, #轴颜色
    "graph_origin": 2.5 * DOWN + 4 * LEFT, #原点在屏幕上位置
    "exclude_zero_label": True,
    "default_graph_colors": [BLUE, GREEN, YELLOW], #默认函数颜色
    "default_derivative_color": GREEN,
    "default_input_color": YELLOW,
    "default_riemann_start_color": BLUE,
    "default_riemann_end_color": GREEN,
    "area_opacity": 0.8,
    "num_rects": 50,
}
```

标出了一些自认为常用一些的参数

* setup_axes()
`self.setup_axes(animate=Ture)`
初始化画出坐标轴

* get_graph()
`self.get_graph(func,color=None,x_min=None,x_max=None)`
返回一个函数图像，func是一个函数，可以使用`def`定义或者用`lambda`匿名函数。

* get_vertical_line_to_graph()
`self.get_vertical_line_to_graph(x,graph)`
返回一个图像在`x`处的垂线。

* get_graph_label()
`self.get_graph_label(graph,label="f(x)",x_val=None,direction=RIGHT,color=None)`
返回函数标签

* input_to_graph_point()
`self.input_to_graph_point(x,graph)`
返回某个函数上的点在屏幕上的坐标
