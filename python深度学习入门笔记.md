# python深度学习入门学习笔记

## 1.python基础

### 1.1 基本语法

```python
type()
#通过函数查看数据类型
a[1:]
a[:-2]#获取第一个元素到倒数前两个元素之间的元素
{'height': 180, 'weight': 70}
#字典
```

### 1.2 **numpy模块**

#### 1.2.1 基本的概念和运算

```python
>>> x = np.array([1.0, 2.0, 3.0])
>>> y = np.array([2.0, 4.0, 6.0])
>>> x + y # 对应元素的加法
array([ 3., 6., 9.])
>>> x - y
array([ -1., -2., -3.])
>>> x * y # element-wise product
array([ 2., 8., 18.])
>>> x / y
array([ 0.5, 0.5, 0.5])

>>> x = np.array([1.0, 2.0, 3.0])
>>> x / 2.0
array([ 0.5, 1. , 1.5])

>>> A = np.array([[1, 2], [3, 4]])
>>> print(A)
[[1 2]
[3 4]]
>>> A.shape
(2, 2)
>>> A.dtype
dtype('int64')

>>> B = np.array([[3, 0],[0, 6]])
>>> A + B
array([[ 4, 2],
[ 3, 10]])
>>> A * B#对应的元素相乘
array([[ 3, 0],
[ 0, 24]])
```

**`一般化后的向量或者矩阵统称为张量（tensor）`**

#### 1.2.1 广播

在不同维度的张量进行运算的时候，将低维度的张量扩展为高维度的张量进行运算

![image-20210309152857085](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210309152857085.png)

#### 1.2.2 访问元素

```python
>>> X = np.array([[51, 55], [14, 19], [0, 4]])
>>> print(X)
[[51 55]
[14 19]
[ 0 4]]
>>> X[0] # 第0行
array([51, 55])
>>> X[0][1] # (0,1)的元素
55

>>> for row in X:#for循环访问元素
... print(row)
...
[51 55]
[14 19]
[0 4]

X = X.flatten() # 将X转换为一维数组

>>> X[np.array([0, 2, 4])] # 获取索引为0、2、4的元素
array([51, 14, 0])

>>> X > 15
array([ True, True, False, True, False, False], dtype=bool)
>>> X[X>15]
array([51, 55, 19])
```

### 1.3 Matplotlib模块

#### 1.3.1 绘制简单的图形

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(0,10,0.1)
y = np.sin(x)

plt.plot(x,y)
plt.show()
```

![image-20210309162447179](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210309162447179.png)

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(0,10,0.1)

y1 = np.sin(x)
y2 = np.cos(x)

plt.plot(x,y1,label="sin")
plt.plot(x,y2,linestyle="--",label="cos")

plt.xlabel("x")
plt.ylabel("y")
plt.title('sin & cos')

plt.legend()
plt.show()
```

![image-20210309162853354](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210309162853354.png)

#### 1.3.2 读入显示图像

```python
from matplotlib.image import imread
import matplotlib.pyplot as plt

img = imread("myplot.png")
plt.imshow(img)

plt.show()
```

## 2. 感知机

简单实现

```python
def AND(x1, x2):
w1, w2, theta = 0.5, 0.5, 0.7
tmp = x1*w1 + x2*w2
if tmp <= theta:
return 0
elif tmp > theta:
return 1
```

## 3. 神经网络

神经网络的激活函数

```python
sigmoid 函数

y = y.astype(np.int)
#进行格式转换
```

$$
h(x) = 1/(1+exp(-x))
$$

**MNIST数据集**

```python
load_mnist(normalize=True, flatten=True, one_hot_label=False)
#normalize设置是否将输入图像正规化为0.0～1.0 的值，如果将该参数设置为False，则输入图像的像素会保持原来的0～255。
#flatten设置是否展开输入图像（变成一维数组）。如果将该参数设置为False，则输入图像为1 × 28 × 28 的三维数组
#one_hot_label设置是否将标签保存为onehot表示（one-hot representation）。one-hot 表示是仅正确解标签为1，其余皆为0 的数组，就像[0,0,1,0,0,0,0,0,0,0]这样
```



```python
numbers.argmax(axis=0)
#当axis=0，是在列中比较，选出最大的 行 索引

#当axis=1，是在行中比较，选出最大的 列 索引
numbers.argmax(axis=1)
```



**需要针对不同的问题人工考虑合适的特征量**

**避免过拟合也是机器学习的一个重要课题（只对某个数据集过度拟合的状态称为过拟合）**

**损失函数是表示神经网络性能的“恶劣程度”的指标，即当前的神经网络对监督数据在多大程度上不拟合，在多大程度上不一致。**



### 3.1 常用的损失函数

**均方误差**

![image-20210311154629395](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210311154629395.png)

yk 是表示神经网络的输出，tk 表示监督数据，k 表示数据的维数。

**交叉熵误差**

![image-20210311154722448](https://raw.githubusercontent.com/wenchenwan/CloudPic/master/img/image-20210311154722448.png)

### 3.2 mini-batch学习

神经网络的学习也是从训练数据中选出一批数据（称为mini-batch, 小批量），然后对每个mini-batch 进行学习。



> **在进行神经网络的学习时，不能将识别精度作为指标。因为如果以识别精度为指标，则参数的导数在绝大多数地方都会变为0。**



```python
grad = np.zeros_like(x) # 生成和x形状相同的数组
```



**梯度指示的方向是各点处的函数值减小最多的方向**

## 4. 误差反向传播法

```python
OrderedDict() 
#生成有序字典
```

