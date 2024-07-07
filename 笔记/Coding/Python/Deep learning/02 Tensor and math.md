## The gears of neural networks - Tensor operations

All transformations learned by deep Neural networks can be reduced to a handful of tensor operations (or tensor functions) applied to tensors of numeric data.

In the first example, use `dense` on the top.

```python
from tensorflow import keras 
keras.layers.Dense(512, activation="relu")
```

This layer can be interpreted as a function, which takes as input a matrix and returns another matrix

matrix → another matrix

and the ouput will be like:   
`output = relu(dot(input, W) + b)`

+ A dot product (dot) between the input tensor and a tensor named W
+ An addition (+) between the resulting matrix and a vector b
+ A relu operation: relu(x) is max(x, 0); “relu” stands for “rectified linear unit”
### Element-wise

是两个张量之间的操作，它在相应张量内的对应的元素进行操作。

An element-wise operation operates on corresponding elements between tensors. 如果两个元素在张量内占据相同位置，则称这两个元素是对应的。

The relu operation and addition are element-wise operations.
```python
import numpy as np

def naive_relu(x):
 assert len(x.shape) == 2 # a martix
 x = x.copy() 
 for i in range(x.shape[0]):
    for j in range(x.shape[1]):
        x[i, j] = max(x[i, j], 0)
 return x

a = np.array([[-3,4],[5,6]])

naive_relu(a)
```

The function takes in a 2D NumPy array x as input and returns a new array x_relu where all negative elements in x are replaced with zeros.
```python
def naive_add(x, y):
 assert len(x.shape) == 2
 assert x.shape == y.shape
 x = x.copy() 
 for i in range(x.shape[0]):
    for j in range(x.shape[1]):
        x[i, j] += y[i, j]
 return x

b = np.array([[-7,8],[9,10]])
c = a + b 
c = np.maximum(c, 0.)
c
```

Time the difference:
```python

import time
x = np.random.random((20, 100))
y = np.random.random((20, 100))
t0 = time.time() 
print(x)

for _ in range(1000):
 z = x + y
 z = np.maximum(z, 0.) 
print("Took: {0:.2f} s".format(time.time() - t0))

t0 = time.time() 
for _ in range(1000):
 z = naive_add(x, y)
 z = naive_relu(z) 
print("Took: {0:.2f} s".format(time.time() - t0))
```

### Broadcasting

Broadcasting refers to the execution of arithmetic operations between tensors of different shapes.

For example, two tensors being added with different shapes, the smaller tensor will be broadcast to match the shape of the larger tensor.

```python
import numpy as np
X = np.random.random((3, 5)) 
y = np.random.random((5,))
print(X)
print("\n")
print(y)
```

```output
[[0.87628777 0.19179271 0.10802148 0.87111038 0.75219124] [0.13592823 0.91225958 0.94121726 0.43965436 0.77155662] [0.91764492 0.64003406 0.16399792 0.76989473 0.12364939]] 

[0.64336139 0.66375359 0.62909959 0.45616497 0.35307919]
```

Extend to 2-D:
```python
y_extend = np.expand_dims(y, axis=0) 
y_extend  # with shape of (1,10)
```

```output
array([[0.64336139, 0.66375359, 0.62909959, 0.45616497, 0.35307919]])
```

Then repeat and add

```python
Y = np.concatenate([y_extend] * 3, axis=0)  # Repeat y 3 times along axis 0
print(Y)
print("\n")
print(X+Y)
```

One function code:

```python
def naive_add_matrix_and_vector(x, y):
 assert len(x.shape) == 2 
 assert len(y.shape) == 1 
 assert x.shape[1] == y.shape[0]
 x = x.copy() 
 for i in range(x.shape[0]):
   for j in range(x.shape[1]):
    x[i, j] += y[j]
 return x

naive_add_matrix_and_vector(X,y)
```

With broadcasting, you can generally perform element-wise operations that take two inputs tensors if one tensor has shape $$(a, b, … n, n + 1, … m)$$ and the other has shape $$(n, n + 1, … m)$$. 
### Tensor product (Dot product)

In NumPy, a tensor product is done using the np.dot function (because the mathematical notation for tensor product is usually a dot):

```python
v1 = np.random.random((5,))
v2 = np.random.random((5,))
v_dot = np.dot(v1, v2)
v_dot

---
1.6083268610285781
```

The basic function is:

```python
def naive_vector_dot(x, y):
 assert len(x.shape) == 1 
 assert len(y.shape) == 1 
 assert x.shape[0] == y.shape[0]
 z = 0.
 for i in range(x.shape[0]):
   z += x[i] * y[i]
 return z
```

You can also take the dot product between a matrix and a vector
```
def naive_matrix_vector_dot(x, y):
 assert len(x.shape) == 2 
 assert len(y.shape) == 1 
 assert x.shape[1] == y.shape[0] 
 z = np.zeros(x.shape[0]) 
 for i in range(x.shape[0]):
    for j in range(x.shape[1]):
        z[i] += x[i, j] * y[j]
 return z
```

```python
m1 = np.array([[2,3],[4,5]])
v3 = np.array([5,6])
dot_2 = np.dot(m1, v3)
dot_2
```

###  Tensor reshaping

A third type of tensor operation that’s essential to understand is tensor reshaping.

Preprocessed the digits data before feeding it into our model:
`train_images = train_images.reshape((60000, 28 * 28))`

```python
x = np.array([[0., 1.],
 [2., 3.],
 [4., 5.]])
x

---
array([[0., 1.], [2., 3.], [4., 5.]])
```


```python
x = x.reshape((6, 1))
x

---
array([[0.], 
	   [1.], 
	   [2.], 
	   [3.], 
	   [4.], 
	   [5.]])
```

Use transpose can let x[i, :] becomes x[:, i]:

```python
x = np.transpose(x)
x

---
array([[0., 1., 2., 3., 4., 5.]])
```

### Geometic explaination of tensors
See a vector:
$$A = [0.5, 1]$$

In a 2D space will be:
```python
import numpy as np
import matplotlib.pyplot as plt

fig,ax=plt.subplots(figsize=(2.5, 2.5), dpi=100)
a=ax.quiver(0, 0, 0.5, 1, angles='xy', scale_units='xy', scale=1)
ax.set_xlim([0, 1.1])
ax.set_ylim([0, 1.1])
plt.show()
```

```python
import numpy as np
import matplotlib.pyplot as plt

fig, ax= plt.subplots(figsize=(3, 3))
a1=ax.quiver(0, 0, 0.5, 1, angles='xy', scale_units='xy', scale=1)
a2=ax.quiver(0, 0, 1, 0.5, angles='xy', scale_units='xy', scale=1, color='b')
a3=ax.quiver(0.5, 1, 1, 0.5, angles='xy', scale_units='xy', scale=1, color='grey',alpha=0.3)
a4=ax.quiver(1, 0.5, 0.5, 1, angles='xy', scale_units='xy', scale=1, color='grey',alpha=0.3)
a5=ax.quiver(0, 0, 1.5, 1.5, angles='xy', scale_units='xy', scale=1, color='green')
ax.text(0.15, 0.5, 'a', fontsize=12)
ax.text(0.6, 0.15, 'b', fontsize=12, color='b')
ax.text(0.95, 1.1, 'a+b', fontsize=12, color='green')
ax.set_xlim([0, 1.6])
ax.set_ylim([0, 1.6])
plt.show()
```

Tensor, as a result, represents the action of translating an object (moving
the object without distorting it) by a certain amount in a certain direction:

![[../../../assets/Coding/translation_as_a_vector_addition.png]]

And rotation of a 2D vector by an angle via a dot product:

![[../../../assets/Coding/2D_rotation.png]]

In general, elementary geometric operations such as translation, rotation, scaling, skewing, and so on can be expressed as tensor operations. 

For example:

```python
np.dot(np.array([[1, 0],[0, -0.5]]),np.array([[2],[8]]))

---
array([[ 2.], 
	   [-4.]])
```

![Scaling](D:/Study/Obsidian/00%20Attachments/Coding/2D_scaling.png)

```python
np.dot(np.array([[0, 1],[1, 0]]),np.array([[2],[8]]))

---
array([[8], 
	   [2]])
```

Other operation include:   
`W • x + b`   
`relu(W • x + b)`   

Each layer in a deep network applies a transformation that disentangles the data a little, and a deep stack of layers makes tractable an extremely complicated disentanglement process. 

## The engine of neural networks: Gradient-based optimization

See the equation:
$$output = relu(dot(input, W) + b)$$
In this expression, **W** and **b** are tensors that are attributes of the layer. They’re called
the **weights (权重)** or **trainable parameters of the layer** (the kernel and bias attributes, respectively). Initially, these weight matrices are filled with small random values (a step called random initialization).

These weights contain the information learned by the model from exposure to
training data.

Training loop:
1. Draw a batch of training samples, x, and corresponding targets, y_true.
2. Run the model on x (a step called the forward pass) to obtain predictions, y_pred.
3. Compute the loss of the model on the batch, a measure of the mismatch between y_pred and y_true.
4. Update all weights of the model in a way that slightly reduces the loss on this batch. 

Step 1 sounds easy enough—just I/O code. Steps 2 and 3 are merely the application
of a handful of tensor operations, so you could implement these steps purely from what
you learned in the previous section.

The difficult part is step 4: updating the model’s weights. Given an individual weight coefficient in the model, **how can you compute whether the coefficient should be increased or decreased, and by how much.**
### Gradient descent (梯度下降)
The differential (微分) is equal to 0, and you find either a maximum or a minimum, a maximum or a minimum, depending on the solution you get from the second-order differential, whether the result is greater than 0 or less than 0.

$$ f(x) = x^2 - 10x + 2 $$
$$ {f}'(x) = 2x -10 $$
$$ {f}''(x) = 2 $$

So when x = 5, there is a minimum of -24.

But in practice it is impossible to find a unique solution like this, so it is necessary to find an approximate solution to approach the extreme value.

Gradient descent is an iterative process that adjusts the model's parameters to minimize the loss function. It is one of the optimization theory to find the local minimum of a function.
So firstly, what is gradient?

A premise is this function is an any differentiable one.

+ The gradient of a one-dimensional scalar x, usually expressed as ${f}'(x)$ .
+ The gradient of a multi-dimensional vector x, usually expressed as $∇f(x)$.

$$
x=\left[\begin{array}{c}
x_{1} \\
x_{2} \\
\vdots \\
x_{d}
\end{array}\right], \nabla f(x)=\left[\begin{array}{c}
\frac{\partial f(x)}{\partial x_{1}} \\
\frac{\partial f(x)}{\partial x_{2}} \\
\vdots \\
\frac{\partial f(x)}{\partial x_{d}}
\end{array}\right]
$$

[知乎回答](https://www.zhihu.com/question/305638940)

Types:
+ 批量梯度下降 (Batch gradient descent, BGD)，某些文献中也称为 (Full gradient, FG)
+ 小批量梯度下降 (Mini-batch gradient descent, MGD)
+ 随机梯度下降 (Stochastic gradient descent, SGD)

![](https://img-blog.csdnimg.cn/img_convert/4b735a8ca98a1232985160cdadc5901e.png)

Improved algorithm?
动量梯度下降 (Gradient Descent with Momentum), or 重球 (Heavy Ball, HB) 方法.

```python
past_velocity = 0.
momentum = 0.1 
while loss > 0.01: 
    w, loss, gradient = get_current_parameters()
    velocity = past_velocity * momentum - learning_rate * gradient
    w = w + momentum * velocity - learning_rate * gradient
    past_velocity = velocity
    update_parameter(w)
```
### Back Propagation (BP)
> 反向传播法是梯度下降法在深度网络上的具体实现方式

[Reference here](https://zhuanlan.zhihu.com/p/40378224)

<img src="https://pic1.zhimg.com/80/v2-97aa6dc25bdec7c407ea429d4b2c632c_1440w.webp" width=400 img>

神经网络 $y = f_{w}(x)$, $w$ 的真实值是我们的目标，但我们有的只是一些 $x$ 和与之对应的真实的 $y$ 的值（训练集），用这两个值去估计 $w$ 的真实值。则问题被简化为：
$$\min _{w} \sum_{x}\left\|f_{w}(x)-y\right\|^{2}$$

令 $E = \sum_{x}\left\|f_{w}(x)-y\right\|^{2}$ ，即残差的集合，所以问题变成了**梯度下降法**求 $E_{\min}$
#### THE CHAIN RULE
（导数的）链式法则是 BP 的基础和核心

Two functions f and g

$y = g(x); z = f(g(x))$

有 $fg(x) == f(g(x))$

令 $z = h(x)$, so $h = f \circ g$

$\frac{d y}{d x}=g^{\prime}(x), \frac{d z}{d y}=f^{\prime}(y)$

可以有:
$$h^{\prime}(x)=\frac{d z}{d x}=\frac{d z}{d y} \cdot \frac{d y}{d x}$$
**Back Propagation By Example:**

以上图为例，输入层有两个神经元 $x_{1},x_{2}$ ，隐藏层有两个神经元 $h_{1},h_{2}$

令 $x_{1} = 1,x_{2} = 0.5$，$w_{1} - w_{4}$ 分别为 1，2，3，4，$w_{5} = 0.5; w_{6} = 0.6$

$$\begin{array}{l}
h_{1}=w_{1} \cdot x_{1}+w_{2} \cdot x_{2}=2 \\
h_{2}=w_{3} \cdot x_{1}+w_{4} \cdot x_{2}=5 \\
y=w_{5} \cdot h_{1}+w_{6} \cdot h_{2}=4
\end{array}$$
模拟Back Propagation过程，假设只知道 $x_{1} = 1 ; x_{2} = 0.5 ; t = 4$

随机化一下，$w_{1}=0.5, w_{2}=1.5, w_{3}=2.3, w_{4}=3, w_{5}=1, w_{6}=1$

则：
$$
\begin{aligned}
h_{1} & =w_{1} \cdot x_{1}+w_{2} \cdot x_{2}=1.25 \\
h_{2} & =w_{3} \cdot x_{1}+w_{4} \cdot x_{2}=3.8 \\
y & =w_{5} \cdot h_{1}+w_{6} \cdot h_{2}=5.05 \\
E & =\frac{1}{2}(y-t)^{2}=0.55125
\end{aligned}
$$
更新 $w_{5}$ 的值，要求 $\displaystyle\frac{\partial E}{\partial w_5}$ 
$$\displaystyle\frac{\partial E}{\partial w_5}=\dfrac{\partial E}{\partial y}\cdot\dfrac{\partial y}{\partial w_5}$$

$E =\frac{1}{2}(y-t)^{2}=0.55125$, 则 $\dfrac{\partial E}{\partial y} = y - t = 1.05$

$y=w_5*h1+w_6*h2$，则 $\dfrac{\partial y}{\partial w_5}=h_1+0=h_1=1.25$

Thus, $\displaystyle\frac{\partial E}{\partial w_5} = 1.05*1.25 = 1.3125$

运用梯度下降法的公式更新 $w_{5}$:
$$
\begin{aligned}w_5^+&=w_5-\eta\cdot\frac{\partial E}{\partial w_5}\\ &=1-0.1\cdot1.3125\\ &=0.86875\end{aligned}
$$

同理更新出所有 $w^+$, 新的 $w_n^+$ 再来预测一次网络输出值，根据 Feed Forward Pass 得到 $y^+ = 3.1768$ ，新的 $E^+ = 0.55125$，重复这个步骤即可