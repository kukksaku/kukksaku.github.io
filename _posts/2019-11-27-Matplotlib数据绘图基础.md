---
layout:     post
title:      Matplotlib基础
subtitle:   Matplotlib数据绘图基础
date:       2019-11-27
author:     kkkkkuboy
header-img: img/post-bg-ios9-web.jpg
catalog: 	 true
tags:
    - Matplotlib
	- 数据分析
    
---

# Matplotlib数据绘图基础



Matplotlib是支持python语言的开源绘图库。

**知识点**

- 二维图形绘制
- 子图及组合图形
- 兼容 MATLAB 风格 API

在使用 Notebook 环境绘图时，需要先运行 Jupyter Notebook 的魔术命令 `%matplotlib inline`。这条命令的作用是将 Matplotlib 绘制的图形嵌入在当前页面中。而在桌面环境中绘图时，不需要添加此命令，而是在全部绘图代码之后追加 `plt.show()`。

## 简单图形绘制

使用 Matplotlib 提供的面向对象 API，需要导入 `pyplot` 模块，并约定简称为 `plt`。

```python
from matplotlib import pyplot as plt
```

下面进行简单的图形绘制

```python
# 通过一行代码绘制一张简单的折线图
>>>plt.plot([1, 2, 3, 2, 1, 2, 3, 4, 5, 6, 5, 4, 3, 2, 1])
>>>plt.show()
```

以上代码运行后如下

![mat1](C:\Users\sakura\Documents\prepare\kaggle\image\mat1.JPG)

<img src="C:\Users\sakura\Documents\prepare\kaggle\image\mat2.JPG" alt="mat2" style="zoom:67%;" />

前面，我们从 Matplotlib 中导入了 `pyplot` 绘图模块，并将其简称为 `plt`。`pyplot` 模块是 Matplotlib 最核心的模块，几乎所有样式的 2D 图形都是经过该模块绘制出来的。这里简称其为 `plt` 是约定俗成的，以便拥有更好的可读性。

`plt.plot()` 是 `pyplot` 模块下面的直线绘制（折线图）方法类。示例中包含了一个 `[1, 2, 3, 2, 1, 2, 3, 4, 5, 6, 5, 4, 3, 2, 1]` 列表，Matplotlib 会默认将该列表作为 y 值，而 x 值会从 0 开始依次递增。

当然，如果你需要自定义横坐标值，只需要传入两个列表即可。如下方代码，我们自定义横坐标刻度从 2 开始。如下：

<img src="C:\Users\sakura\Documents\prepare\kaggle\image\mat3.JPG" alt="mat3" style="zoom: 67%;" />



`pyplot` 模块中 `pyplot.plot` 方法是用来绘制折线图的。你应该会很容易联想到，更改后面的方法类名就可以更改图形的样式。的确，在 Matplotlib 中，大部分图形样式的绘制方法都存在于 `pyplot` 模块中。例如：

| 方法                               | 含义           |
| ---------------------------------- | -------------- |
| `matplotlib.pyplot.angle_spectrum` | 绘制电子波谱图 |
| `matplotlib.pyplot.bar`            | 绘制柱状图     |
| `matplotlib.pyplot.barh`           | 绘制直方图     |
| `matplotlib.pyplot.broken_barh`    | 绘制水平直方图 |
| `matplotlib.pyplot.contour`        | 绘制等高线图   |
| `matplotlib.pyplot.errorbar`       | 绘制误差线     |
| `matplotlib.pyplot.hexbin`         | 绘制六边形图案 |
| `matplotlib.pyplot.hist`           | 绘制柱形图     |
| `matplotlib.pyplot.hist2d`         | 绘制水平柱状图 |
| `matplotlib.pyplot.pie`            | 绘制饼状图     |
| `matplotlib.pyplot.quiver`         | 绘制量场图     |
| `matplotlib.pyplot.scatter`        | 散点图         |
| `matplotlib.pyplot.specgram`       | 绘制光谱图     |

`matplotlib.pyplot.plot(*args, **kwargs)` 方法严格来讲可以绘制线形图或者样本标记。其中，`*args` 允许输入单个 y 值或 x, y 值。

如，我们绘制一张自定义的正弦曲线图

```python
import numpy as np  # 载入数值计算模块

# 在 -2Π 和 2Π 之间等间距生成 1000 个值，也就是 X 坐标
X = np.linspace(-2*np.pi, 2*np.pi, 1000)
# 计算 y 坐标
y = np.sin(X)

# 向方法中 `*args` 输入 X，y 坐标
plt.plot(X, y)
plt.show()
```

结果如下:

![mat4](C:\Users\sakura\Documents\prepare\kaggle\image\mat4.JPG)

![mat5](C:\Users\sakura\Documents\prepare\kaggle\image\mat5.JPG)

<img src="C:\Users\sakura\Documents\prepare\kaggle\image\mat6.JPG" alt="mat6" style="zoom: 67%;" />

正弦曲线就绘制出来了。但值得注意的是，`pyplot.plot` 在这里绘制的正弦曲线，实际上不是严格意义上的曲线图，而在两点之间依旧是直线。这里看起来像曲线是因为样本点相互挨得很近。

柱形图 `matplotlib.pyplot.bar(*args, **kwargs)` 大家应该都非常了解了。这里，我们直接用上面的代码，仅把 `plt.plot(X, y)` 改成 `plt.bar(X, y)` 试一下。

```python
plt.bar([1,2,3],[1,2,3])
```

结果如下：

<img src="C:\Users\sakura\Documents\prepare\kaggle\image\mat7.JPG" alt="mat7" style="zoom:80%;" />



散点图 `matplotlib.pyplot.scatter(*args, **kwargs)` 就是呈现在二维平面的一些点，这种图像的需求也是非常常见的。比如，我们通过 GPS 采集的数据点，它会包含经度以及纬度两个值，这样的情况就可以绘制成散点图。

```python
# X,y 的坐标均有 numpy 在 0 到 1 中随机生成 1000 个值
X = np.random.ranf(1000)
y = np.random.ranf(1000)
# 向方法中 `*args` 输入 X，y 坐标
plt.scatter(X, y)
```

<img src="C:\Users\sakura\AppData\Roaming\Typora\typora-user-images\image-20191127164511701.png" alt="image-20191127164511701" style="zoom: 67%;" />



饼状图 `matplotlib.pyplot.pie(*args, **kwargs)` 在有限列表以百分比呈现时特别有用，你可以很清晰地看出来各类别之间的大小关系，以及各类别占总体的比例。

量场图 `matplotlib.pyplot.quiver(*args, **kwargs)` 就是由向量组成的图像，在气象学等方面被广泛应用。从图像的角度来看，量场图就是带方向的箭头符号。

如：

```python
X, y = np.mgrid[0:10, 0:10] # mgrid功能是返回多维结构，常见如2D图形，3D图形。np.mgrid[ 第1维，第2维 ，第3维 ， …] 
plt.quiver(X, y)
```

<img src="C:\Users\sakura\Documents\prepare\kaggle\image\mat8.JPG" alt="mat8" style="zoom:80%;" />



中学学习地理的时候，我们就知道等高线了。等高线图 `matplotlib.pyplot.contourf(*args, **kwargs)` 是工程领域经常接触的一类图，它的绘制过程稍微复杂一些。

```
# 生成网格矩阵
x = np.linspace(-5, 5, 500)
y = np.linspace(-5, 5, 500)
X, Y = np.meshgrid(x, y)
# 等高线计算公式
Z = (1 - X / 2 + X ** 3 + Y ** 4) * np.exp(-X ** 2 - Y ** 2)

plt.contourf(X, Y, Z)
```

<img src="C:\Users\sakura\Documents\prepare\kaggle\image\mat9.JPG" alt="mat9" style="zoom: 80%;" />



## 定义图形样式

上面，我们绘制了简单的基础图形，但这些图形都不美观。你可以通过更多的参数来让图形变得更漂亮。我们已经知道了，线形图通过 `matplotlib.pyplot.plot(*args, **kwargs)` 方法绘出。其中，`args` 代表数据输入，而 `kwargs` 的部分就是用于设置样式参数了。

**二维线形图** [<i class="fa fa-external-link-square" aria-hidden="true"> 包含的参数</i>](https://matplotlib.org/3.1.0/api/_as_gen/matplotlib.pyplot.plot.html) 超过 40 余项，其中常用的也有 10 余项，选取一些比较有代表性的参数列举如下：

| 参数         | 含义                            |
| ------------ | ------------------------------- |
| `alpha=`     | 设置线型的透明度，从 0.0 到 1.0 |
| `color=`     | 设置线型的颜色                  |
| `fillstyle=` | 设置线型的填充样式              |
| `linestyle=` | 设置线型的样式                  |
| `linewidth=` | 设置线型的宽度                  |
| `marker=`    | 设置标记点的样式                |
| ……           | ……                              |

如三角函数图形：

```python
# 在 -2PI 和 2PI 之间等间距生成 1000 个值，也就是 X 坐标
X = np.linspace(-2 * np.pi, 2 * np.pi, 1000)
# 计算 sin() 对应的纵坐标
y1 = np.sin(X)
# 计算 cos() 对应的纵坐标
y2 = np.cos(X)

# 向方法中 `*args` 输入 X，y 坐标
plt.plot(X, y1, color='r', linestyle='--', linewidth=2, alpha=0.8)
plt.plot(X, y2, color='b', linestyle='-', linewidth=2)
```

<img src="C:\Users\sakura\Documents\prepare\kaggle\image\mat10.JPG" alt="mat10" style="zoom:67%;" />

**散点图**也是相似的，它们的很多样式参数都是大同小异，需要大家阅读 [<i class="fa fa-external-link-square" aria-hidden="true"> 官方文档</i>](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.scatter.html) 详细了解。

| 参数          | 含义                 |
| ------------- | -------------------- |
| `s=`          | 散点大小             |
| `c=`          | 散点颜色             |
| `marker=`     | 散点样式             |
| `cmap=`       | 定义多类别散点的颜色 |
| `alpha=`      | 点的透明度           |
| `edgecolors=` | 散点边缘颜色         |

**饼状图**通过 `matplotlib.pyplot.pie()` 绘出。我们也可以进一步设置它的颜色、标签、阴影等各类样式。下面就绘出一个示例。

```python
label = 'Cat', 'Dog', 'Cattle', 'Sheep', 'Horse'  # 各类别标签
color = 'r', 'g', 'r', 'g', 'y'  # 各类别颜色
size = [1, 2, 3, 4, 5]  # 各类别占比
explode = (0, 0, 0, 0, 0.2)  # 各类别的偏移半径
# 绘制饼状图
plt.pie(size, colors=color, explode=explode,
        labels=label, shadow=True, autopct='%1.1f%%')
# 饼状图呈正圆
plt.axis('equal')
```



![mat11](C:\Users\sakura\Documents\prepare\kaggle\image\mat11.JPG)



## 组合图形样式

上面演示了单个简单图像的绘制。实际上，我们往往会遇到将几种类型的一样的图放在一张图内显示，也就是组合图的绘制。其实很简单，你只需要将所需图形的代码放置在一起就可以了，比如绘制一张包含柱形图和折线图的组合图。

```python
from matplotlib import pyplot as plt
x = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19]
y_bar = [3, 4, 6, 8, 9, 10, 9, 11, 7, 8]
y_line = [2, 3, 5, 7, 8, 9, 8, 10, 6, 7]

plt.bar(x, y_bar)
plt.plot(x, y_line, '-o', color='y')
plt.show()
```

<img src="C:\Users\sakura\Documents\prepare\kaggle\image\mat12.JPG" alt="mat12" style="zoom:80%;" />



当然，并不是任何的代码放在一起都是组合图。上面，两张图的横坐标必须共享，才能够被 Matplotlib 自动判断为组合图效果。



## 定义图形位置

在图形的绘制过程中，你可能需要调整图形的位置，或者把几张单独的图形拼接在一起。此时，我们就需要引入 `plt.figure` 图形对象了。

```python
import numpy as np
x = np.linspace(0, 10, 20)  # 生成数据
y = x * x + 2

fig = plt.figure()  # 新建图形对象
axes = fig.add_axes([0.5, 0.5, 0.8, 0.8])  # 控制画布的左, 下, 宽度, 高度, 需小于1
axes.plot(x, y, 'r')
```

定义一个plt.figure对象后必须要add_axes,即控制画布，不然只会生成一个对象但是没有画布而报错，报错‘Figure’ object has no attribute 'plot'.

上面的绘图代码中，你可能会对 `figure` 和 `axes` 产生疑问。Matplotlib 的 API 设计的非常符合常理，在这里，`figure` 相当于绘画用的画板，而 `axes` 则相当于铺在画板上的画布。我们将图像绘制在画布上，于是就有了 `plot`，`set_xlabel` 等操作。

借助于图形对象，我们可以实现大图套小图的效果。

```python
fig = plt.figure()  # 新建画板
axes1 = fig.add_axes([0.1, 0.1, 0.8, 0.8])  # 大画布
axes2 = fig.add_axes([0.2, 0.5, 0.4, 0.3])  # 小画布

axes1.plot(x, y, 'r')  # 大画布
axes2.plot(y, x, 'g')  # 小画布
```

<img src="C:\Users\sakura\Documents\prepare\kaggle\image\mat13.JPG" alt="mat13" style="zoom:67%;" />

上面的绘图代码中，你已经学会了使用 `add_axes()` 方法向我们设置的画板 `figure` 中添加画布 `axes`。在 Matplotlib 中，还有一种添加画布的方式，那就是 `plt.subplots()`，它和 `axes` 都等同于画布。

```python
fig, axes = plt.subplots()
axes.plot(x, y, 'r')
```

借助于 `plt.subplots()`，我们就可以实现子图的绘制，也就是将多张图按一定顺序拼接在一起。

```python
fig, axes = plt.subplots(nrows=1, ncols=2)  # 子图为 1 行，2 列
for ax in axes:
    ax.plot(x, y, 'r')
```



<img src="C:\Users\sakura\Documents\prepare\kaggle\image\mat15.JPG" alt="mat15" style="zoom:80%;" />

通过设置 `plt.subplots` 的参数，可以实现调节画布尺寸和显示精度。

```python
fig, axes = plt.subplots(
    figsize=(16, 9), dpi=50)  # 通过 figsize 调节尺寸, dpi 调节显示精度
axes.plot(x, y, 'r')
```





## 规范绘图方法

首先，任何图形的绘制，都建议通过 `plt.figure()` 或者 `plt.subplots()` 管理一个完整的图形对象。而不是简单使用一条语句，例如 `plt.plot(...)` 来绘图。

管理一个完整的图形对象，有很多好处。在图形图形的基础上，给后期添加图例，图形样式，标注等预留了很大的空间。除此之外。代码看起来也更加规范，可读性更强。



### 添加图标题、图例

绘制包含图标题、坐标轴标题以及图例的图形，举例如下：

```python
fig, axes = plt.subplots()

axes.set_xlabel('x label')  # 横轴名称
axes.set_ylabel('y label')
axes.set_title('title')  # 图形名称

axes.plot(x, x**2)
axes.plot(x, x**3)
axes.legend(["y = x**2", "y = x**3"], loc=0)  # 图例
```

![image-20191127195930550](C:\Users\sakura\AppData\Roaming\Typora\typora-user-images\image-20191127195930550.png)

图例中的 `loc` 参数标记图例位置，`1，2，3，4` 依次代表：右上角、左上角、左下角，右下角；`0` 代表自适应



### 线型、颜色、透明度

```python
fig, axes = plt.subplots()

axes.plot(x, x+1, color="red", alpha=0.5)
axes.plot(x, x+2, color="#1155dd")
axes.plot(x, x+3, color="#15cc55")

# 而对于线型而言，除了实线、虚线之外，还有很多丰富的线型可供选择
fig, ax = plt.subplots(figsize=(12, 6))

# 线宽
ax.plot(x, x+1, color="blue", linewidth=0.25)
ax.plot(x, x+2, color="blue", linewidth=0.50)
ax.plot(x, x+3, color="blue", linewidth=1.00)
ax.plot(x, x+4, color="blue", linewidth=2.00)

# 虚线类型
ax.plot(x, x+5, color="red", lw=2, linestyle='-')
ax.plot(x, x+6, color="red", lw=2, ls='-.')
ax.plot(x, x+7, color="red", lw=2, ls=':')

# 虚线交错宽度
line, = ax.plot(x, x+8, color="black", lw=1.50)
line.set_dashes([5, 10, 15, 10])

# 符号
ax.plot(x, x + 9, color="green", lw=2, ls='--', marker='+')
ax.plot(x, x+10, color="green", lw=2, ls='--', marker='o')
ax.plot(x, x+11, color="green", lw=2, ls='--', marker='s')
ax.plot(x, x+12, color="green", lw=2, ls='--', marker='1')

# 符号大小和颜色
ax.plot(x, x+13, color="purple", lw=1, ls='-', marker='o', markersize=2)
ax.plot(x, x+14, color="purple", lw=1, ls='-', marker='o', markersize=4)
ax.plot(x, x+15, color="purple", lw=1, ls='-',
        marker='o', markersize=8, markerfacecolor="red")
ax.plot(x, x+16, color="purple", lw=1, ls='-', marker='s', markersize=8,
        markerfacecolor="yellow", markeredgewidth=2, markeredgecolor="blue")
```



### 画布风格、坐标轴范围

有些时候，我们可能需要显示画布网格或调整坐标轴范围。设置画布网格和坐标轴范围。这里，我们通过指定 `axes[0]` 序号，来实现子图的自定义顺序排列。

```python
fig, axes = plt.subplots(1, 2, figsize=(10, 5))

# 显示网格
axes[0].plot(x, x**2, x, x**3, lw=2)
axes[0].grid(True)

# 设置坐标轴范围
axes[1].plot(x, x**2, x, x**3)
axes[1].set_ylim([0, 60])
axes[1].set_xlim([2, 5])
```

除了折线图，Matplotlib 还支持绘制散点图、柱状图等其他常见图形。下面，我们绘制由散点图、梯步图、条形图、面积图构成的子图。

```python
n = np.array([0, 1, 2, 3, 4, 5])

fig, axes = plt.subplots(1, 4, figsize=(16, 5))

axes[0].scatter(x, x + 0.25*np.random.randn(len(x)))
axes[0].set_title("scatter")

axes[1].step(n, n**2, lw=2)
axes[1].set_title("step")

axes[2].bar(n, n**2, align="center", width=0.5, alpha=0.5)
axes[2].set_title("bar")

axes[3].fill_between(x, x**2, x**3, color="green", alpha=0.5)
axes[3].set_title("fill_between")
```

![mat17](C:\Users\sakura\Documents\prepare\kaggle\image\mat17.JPG)





## 图形标注方法

当我们绘制一些较为复杂的图像时，阅读对象往往很难全面理解图像的含义。而此时，图像标注往往会起到画龙点睛的效果。图像标注，就是在画面上添加文字注释、指示箭头、图框等各类标注元素。

```python
# 对柱形图进行文字标注
fig, axes = plt.subplots()

x_bar = [10, 20, 30, 40, 50]  # 柱形图横坐标
y_bar = [0.5, 0.6, 0.3, 0.4, 0.8]  # 柱形图纵坐标
bars = axes.bar(x_bar, y_bar, color='blue', label=x_bar, width=2)  # 绘制柱形图
for i, rect in enumerate(bars):
    x_text = rect.get_x()  # 获取柱形图横坐标
    y_text = rect.get_height() + 0.01  # 获取柱子的高度并增加 0.01
    plt.text(x_text, y_text, '%.1f' % y_bar[i])  # 标注文字
```

![image-20191127213027112](C:\Users\sakura\AppData\Roaming\Typora\typora-user-images\image-20191127213027112.png)

除了文字标注之外，还可以通过 `matplotlib.pyplot.annotate()` 方法向图像中添加箭头等样式标注。接下来，我们向上面的例子中增添一行增加箭头标记的代码。

```python
fig, axes = plt.subplots()

bars = axes.bar(x_bar, y_bar, color='blue', label=x_bar, width=2)  # 绘制柱形图
for i, rect in enumerate(bars):
    x_text = rect.get_x()  # 获取柱形图横坐标
    y_text = rect.get_height() + 0.01  # 获取柱子的高度并增加 0.01
    plt.text(x_text, y_text, '%.1f' % y_bar[i])  # 标注文字

    # 增加箭头标注
    plt.annotate('Min', xy=(32, 0.3), xytext=(36, 0.3),
                 arrowprops=dict(facecolor='black', width=1, headwidth=7))
```

上面的示例中，`xy=()` 表示标注终点坐标，`xytext=()` 表示标注起点坐标。在箭头绘制的过程中，`arrowprops=()` 用于设置箭头样式，`facecolor=` 设置颜色，`width=` 设置箭尾宽度，`headwidth=` 设置箭头宽度，可以通过 `arrowstyle=` 改变箭头的样式。

## 

