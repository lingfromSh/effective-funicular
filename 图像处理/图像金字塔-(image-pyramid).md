---
title: "图像金字塔 (Image Pyramid)"
tags: ""
---

# 图像金字塔 (Image Pyramid)

图像金字塔是图像中多尺度表达的一种，最主要用于图像的分割，是一种以多分辨率来解释图像的有效但概念简单的结构。图像金字塔最初用于机器视觉和图像压缩，一幅图像的金字塔是一系列以金字塔形状排列的分辨率逐步降低，且来源于同一张原始图的图像集合。其通过梯次向下采样获得，直到达到某个终止条件才停止采样。金字塔的底部是待处理图像的高分辨率表示，而顶部是低分辨率的近似。我们将一层一层的图像比喻成金字塔，层级越高，则图像越小，分辨率越低。

## 高斯金字塔

高斯金字塔式在Sift算子中提出来的概念，首先高斯金字塔并不是一个金字塔，而是有很多组（Octave）金字塔构成，并且每组金字塔都包含若干层（Interval）。
用来向下/降采样，主要的图像金字塔。

```python
import cv2 as cv
import matplotlib.pyplot as plt

EXIT_KEY = 27


def gaussian_pyramid(image_path, depth=5):
    # Read image data
    image = cv.imread(image_path)

    # Make gaussian image pyramid
    pyramid = []
    pyramid.append(image)  # Add original image as pyramid 0
    # Add other pyramid layers
    for i in range(1, depth):
        pyramid.append(cv.pyrDown(pyramid[-1]))

    # Draw results
    for idx, layer in enumerate(pyramid):
        plt.subplot(1, depth, idx + 1)
        layer = cv.cvtColor(layer, cv.COLOR_BGR2RGB)
        plt.imshow(layer)
        plt.title(f"Pyramid Layer {idx}")

    # Show results
    plt.show()

```

![高斯金字塔](/home/ling/BoostNote/images/Gaussian-Pyramid.png)

## 拉普拉斯金字塔

用来从金字塔低层图像重建上层未采样图像，在数字图像处理中也即是预测残差，可以对图像进行最大程度的还原，配合高斯金字塔一起使用。  

```python
import cv2 as cv
import matplotlib.pyplot as plt

EXIT_KEY = 27


def laplacian_pyramid(image_path, depth=8):
    # Read image data
    image = cv.imread(image_path)

    # Make gaussian image pyramid
    pyramid = []
    pyramid.append(image)  # Add original image as pyramid 0
    # Add other pyramid layers
    for i in range(1, depth):
        pyramid.append(cv.pyrUp(pyramid[-1]))

    # Draw results
    for idx, layer in enumerate(pyramid):
        plt.subplot(1, depth, idx + 1)
        layer = cv.cvtColor(layer, cv.COLOR_BGR2RGB)
        plt.imshow(layer)
        plt.title(f"Pyramid Layer {idx}")

    # Show results
    plt.show()

```

![拉普拉斯金字塔](/home/ling/BoostNote/images/Laplacian-Pyramid.png)

## 两者的区别

高斯金字塔是下采样，分辨率逐层降低，在金字塔中表示为从底层向上；拉普拉斯金字塔是预测重建高分辨率图像，分辨率逐层增加，在金字塔中表示为从顶层向下。
PyrDown的实质是将i层中的偶数行，偶数列删除形成i+1层，所以i+1层尺寸会是i层的1/4.
PyrUp的实质是向i层插入行和列，偶数行和偶数列填充0，之后采用指定滤波器进行卷积去估计“丢失”像素的近似值。

## 差分金字塔(DOG)

就是把同一张图像在不同的参数下做高斯模糊之后的结果相减，得到的输出图像。称为高斯不同(DOG)。高斯不同是图像的内在特征，在灰度图像增强、角点检测中经常用到。

差分金字塔，DOG（Difference of Gaussian）金字塔是在高斯金字塔的基础上构建起来的，其实生成高斯金字塔的目的就是为了构建DOG金字塔。DOG金字塔的第1组第1层是由高斯金字塔的第1组第2层减第1组第1层得到的。以此类推，逐组逐层生成每一个差分图像，所有差分图像构成差分金字塔。概括为DOG金字塔的第0组第l层图像是由高斯金字塔的第0组第i+1层减第0组第i层得到的。

![DOG金字塔](/home/ling/BoostNote/images/Difference-of-gaussian.png)

实现代码

```python
import cv2 as cv
import matplotlib.pyplot as plt


def DOG(image_path, depth=5):
    # Read image data
    image = cv.imread(image_path)

    # Make Gaussian Pyramid
    pyramid = [[image]]
    # Group
    for o in range(depth):
        layer_original = pyramid[o][0]
        # Layer
        for i in range(depth-1):
            pyramid[o].append(cv.GaussianBlur(layer_original,
                                              ksize=(5, 5),
                                              sigmaX=(2**i)*5))
        if o < depth - 1:
            pyramid.append([cv.pyrDown(layer_original)])

    # Make DOG
    dog = []
    for group_idx, group in enumerate(pyramid):
        dog.append([])
        for i in range(1, depth):
            dog[group_idx].append(cv.subtract(group[i], group[i-1]))

    for group_idx, group in enumerate(dog):
        for layer_idx, layer in enumerate(group):
            plt.subplot(depth, depth, group_idx * depth + layer_idx + 1)
            plt.title(f"DOG Group-{group_idx} Layer-{layer_idx}")
            plt.imshow(layer)

    plt.show()


```

![差分金字塔](/home/ling/BoostNote/images/DOG.png)

## 尺度空间

在高斯金字塔中一共生成O组L层不同尺度的图像，这两个量合起来（O，L）就构成了高斯金字塔的尺度空间，也就是说以高斯金字塔的组O作为二维坐标系的一个坐标，不同层L作为另一个坐标，则给定的一组坐标（O,L）就可以唯一确定高斯金字塔中的一幅图像。
