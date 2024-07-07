numpy - numerical python   
Compared with Python's built-in List and Array data structure, it supports more standardized data types and extremely rich operating interfaces, and it is faster.

> import numpy as np

# Creating `arrays`

Use `np.array` to create:
```Python
my_list = [1, 2, 3]
my_array = np.array(my_list)

# or you can pass it in directly:
my_array = np.array([1, 2, 3])

print(my_array)
print(my_array[0])
---
[1 2 3]
1
```

## Multiple dimensions

A very powerful feature of numpy arrays is that it is good at handling multi-dimensional data:

```Python
grid = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(grid)
---
[[1 2 3]
 [4 5 6]
 [7 8 9]]
```

Some function may contain some useful information:
```Python
print(grid.ndim)  
print(grid.shape)
---
2
(3, 3)
# That means it is a two-dimensional array and is 3x3 

print(grid[1, :])  
print(grid[:, 1])
---
[4 5 6]
[2 5 8]
# use ':' to slicing
```

how about 3-dimension?
```Python
cube = np.array([[[1,2], [3, 4]], [[5, 6], [7, 8]]])
print(cube)
print(cube.ndim)
---
[[[1 2]
  [3 4]]

 [[5 6]
  [7 8]]]
3
```

![[../../../assets/Coding/numpy_array.png]]

## setting values

Values in an array can be changed:

```Python
grid[0, 0] = 999
print(grid)
---
[[999   2   3]
 [  4   5   6]
 [  7   8   9]]


grid[:, 1] = 10
print(grid)
---
[[999  10   3]
 [  4  10   6]
 [  7  10   9]]
```

## Pre-filled arrays

```Python
print(np.zeros(3))  
print(np.ones(3))  
---
[0. 0. 0.]
[1. 1. 1. 1.]


print(np.zeros((3, 5)))
---
[[0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0.]]
```


# Random

An important part of any simulation is an element of randomness.

use `default_rng()` as a [Random Generator](https://numpy.org/doc/stable/reference/random/generator.html)

```Python
from numpy.random import default_rng  
  
rng = default_rng()  
print(rng)
print(rng.random())
---
Generator(PCG64)
0.0041859585036266855


print(rng.random(size=3))  # to get three random numbers
---
[0.3253968  0.65733652 0.59486694]


print(rng.integers(1000))  # from 0 to 1000
---
524

# use low and high to choice the range, and endpoint=T means include the upper bound number(without it in this case, it will not out put 31)
print(rng.integers(low=6, high=31, endpoint=True, size=6))
---
[27 29 29 12 31 12]
```

## Seed

Pseudo-randomness - 伪随机   
Using seed can help it always give the same output:

```Python
seeded_rng_1 = default_rng(seed=31)
print(seeded_rng_1.random(size=5))
---
[0.90317181 0.06767826 0.67273081 0.47254471 0.67742778]

# with the same seed but different name, we can get the same output:
seeded_rng_2 = default_rng(seed=31)  
print(seeded_rng_2.random(size=5))
---
[0.90317181 0.06767826 0.67273081 0.47254471 0.67742778]
```