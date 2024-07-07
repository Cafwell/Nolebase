# Functional Python

Define a simple add function:
```python
def add(x, y):
    """Simple function returns the sum of the arguments"""
    return x + y

add(3, 4)
a = add

a(5, 6)
```

## Properties of a Function:
Just as “integer” and “string” have properties, so to does “function”.

Type into the Python Console:
```python
add.__name__
'add'

add.__doc__
'Simple function returns the sum of the arguments'
```

## Functions as Arguments
Pass functions as arguments.
```python
def call_function(func, arg1, arg2):
    """
    Simple function that calls the function 'func' with  
    arguments 'arg1' and 'arg2', returning the result
    """
    return func(arg1, arg2)
call_function(add, 3, 7)
---
10

def diff(x, y):
    """
    Simple function that returns the difference of
    its arguments
    """
    return x - y
call_function(diff, 9, 2)
---
7
```

Change the function a little:
```python
def call_function_2(func, arg1, arg2):
    """
    Simple function that returns the difference of
    its arguments
    """
    print(f"Calling function '{func.__name__}' with arguments {arg1} and {arg2}.")
    result = func(arg1, arg2)
    print(f"The result is {result}")
    return result

call_function_2(diff, 6, 2)
---
Calling function 'diff' with arguments 6 and 2. The result is 4
4
```

And more:
```python

def multiply(x, y):
    """
    Simple function that returns the product of the
    two arguments
    """
    return x * y

call_function_2(multiply, 3, 4)
```


## Mapping Functions

### `zip()`函数
```python
zip() function intro:
a = [1,2,3]
b = [4,5,6]
c = [4,5,6,7,8]
zipped = zip(a,b) # 一个包，显示的是包所在的地址 <zip object at ...>
print(zipped) 
list(zipped) # 化为列表，查看包中的内容
---
<zip object at 0x000001F871FEED00>
[(1, 4), (2, 5), (3, 6)]
```


```python
list(zip(a,c)) # 元素个数与最短的列表一致
---
[(1, 4), (2, 5), (3, 6)]
```

#### Unzip
```python
print(*zip(a, b)) # 将 zip 对象变成原先组合前的数据
---
(1, 4) (2, 5) (3, 6)
```

```python
d = zip(*zip(a, b)) # 与 zip 相反，zip (*) 可理解为解压，返回二维矩阵式
for i in d:
    print(list(i))
---
[1, 2, 3] [4, 5, 6]
```

### Call append with zip
```python
a = [1, 2, 3, 4, 5]
b = [6, 7, 8, 9, 10]

result = []
zipped = list(zip(a, b))

for i, j in zipped:
    r = add(i, j)
    result.append(r)

print(result)
---
[7, 9, 11, 13, 15]
```

### Mapper function
```python
def mapper(func, arg1, arg2):
    """
    This will map the function 'func' to each pair
    of arguments in the list 'arg1' and 'arg2', returning
    the result
    """

    res = []

    for i, j in zip(arg1, arg2):
        r = func(i, j)
        res.append(r)

    return res

mapper(multiply, a, b)
---
[6, 14, 24, 36, 50]
```

### More
A new one function used for the calculation of the distance between two points:
$$|AB|=√[(x1−x2)2+(y1−y2)2+(z1−z2)2]$$

```python
import math

def calc_distance(point1, point2):
    """
    Function to calculate and return the distance between
    two points
    """

    dx2 = (point1[0] - point2[0]) ** 2
    dy2 = (point1[1] - point2[1]) ** 2

    return math.sqrt(dx2 + dy2)

points1 = [(1.0, 1.0), (2.0, 2.0), (3.0, 3.0)]
points2 = [(4.0, 4.0), (5.0, 5.0), (6.0, 6.0)]

mapper(calc_distance, points1, points2)
---
[4.242640687119285, 4.242640687119285, 4.242640687119285]
```

### Build-in function
Actually, python already built `map()` in as a standard Python function
```python
map(calc_distance, points1, points2)
---
<map at 0x1f871abeb00>
```

```python
def square(x) :         # 计算平方数
  return x ** 2

n = list(map(square, a))
print(n)
---
[1, 4, 9, 16, 25]
```

### Exercise

Write a Python script, called countlines.py, that will count the total number of lines in each of these Shakespeare plays.

To do this, first write a function that counts the number of lines in a file.

Then, use the standard map function to count the number of lines in each Shakespeare play, printing the result as a list.
```python
import urllib.request
import tarfile
urllib.request.urlretrieve("https://milliams.com/courses/parallel_python/shakespeare.tar.bz2", "shakespeare.tar.bz2")
with tarfile.open("shakespeare.tar.bz2") as tar:
    tar.extractall()
```

Normal edition:
```python
import os

current_dir = os.getcwd()
names = []
lines = []
os.chdir('D:\Study\Programming_language\Python\Advanced Py\shakespeare')
works = os.getcwd()
files = os.listdir(works)

for file in files:
    names.append(file)
    with open(file, 'r') as f:
        lines_in_file = len(f.readlines())
        lines.append(lines_in_file)

print(list(zip(names, lines)))
```

Map edition:
```python
import os

current_dir = os.getcwd()
lines = []

os.chdir('D:\Study\Programming_language\Python\Advanced Py\shakespeare')
works = os.getcwd()
files = os.listdir(works)

def count_lines_in_file(filename):
    with open(filename) as f:
        return len(f.readlines())

play_line_count = map(count_lines_in_file, files)

print(list(zip(files, play_line_count)))
```

Result:
```
[('allswellthatendswell', 4515), ('antonyandcleopatra', 5998), ('asyoulikeit', 4122), ('comedyoferrors', 2937), ('coriolanus', 5836), ('cymbeline', 5485), ('hamlet', 6045), ('juliuscaesar', 4107), ('kinglear', 5525), ('loveslabourslost', 4335), ('macbeth', 3876), ('measureforemeasure', 4337), ('merchantofvenice', 3883), ('merrywivesofwindsor', 4448), ('midsummersnightsdream', 3115), ('muchadoaboutnothing', 4063), ('othello', 5424), ('periclesprinceoftyre', 3871), ('README', 2), ('romeoandjuliet', 4766), ('tamingoftheshrew', 4148), ('tempest', 3399), ('timonofathens', 3973), ('titusandronicus', 3767), ('troilusandcressida', 5443), ('twelfthnight', 4017), ('twogentlemenofverona', 3605), ('winterstale', 4643)]
```



## Reduce Functions
If we don't use `reduce()`, we generally use a `for` loop to sum the list elements.
Using Reduce is a more elegant way, and the code will be very concise.

```python
from functools import reduce

e = [1,2,3,4]
print(reduce(add, e))
---
10
```

How reduce works:
<img src="https://pic1.zhimg.com/80/v2-f27f5e88e2e4bdd9d72fac02c2e351c0_1440w.webp" width=300>

### Exercise_2
Uses reduce to print out the total number of lines in all Shakespeare plays.

```python
import os
from functools import reduce

current_dir = os.getcwd()
lines = []

os.chdir('D:\Study\Programming_language\Python\Advanced Py\shakespeare')
works = os.getcwd()
files = os.listdir(works)

def count_lines_in_file(filename):
    with open(filename) as f:
        return len(f.readlines())

play_line_count = list(map(count_lines_in_file, files))
all_lines = reduce(lambda x,y: x+y, play_line_count)

print(list(zip(files, play_line_count)))
print("\n")
print(f"Total number of lines is {all_lines}")
---
[('allswellthatendswell', 4515), ('antonyandcleopatra', 5998), ('asyoulikeit', 4122), ('comedyoferrors', 2937), ('coriolanus', 5836), ('cymbeline', 5485), ('hamlet', 6045), ('juliuscaesar', 4107), ('kinglear', 5525), ('loveslabourslost', 4335), ('macbeth', 3876), ('measureforemeasure', 4337), ('merchantofvenice', 3883), ('merrywivesofwindsor', 4448), ('midsummersnightsdream', 3115), ('muchadoaboutnothing', 4063), ('othello', 5424), ('periclesprinceoftyre', 3871), ('README', 2), ('romeoandjuliet', 4766), ('tamingoftheshrew', 4148), ('tempest', 3399), ('timonofathens', 3973), ('titusandronicus', 3767), ('troilusandcressida', 5443), ('twelfthnight', 4017), ('twogentlemenofverona', 3605), ('winterstale', 4643)] 

Total number of lines is 119685
```




# Multicore (local) Parallel Programming

```python
import os

os.cpu_count()
---
16
```

Each of these cores is available to do work, in parallel, as part of your Python script. For example, 16 cores in this computer, then your script should ideally be able to do 16 things at once.

## Concurrent.futures
When using Python processing tasks, limited to single-threaded processing capacity, and tasks need to be parallelized to multiple threads or multiple processes to perform.

```python
# all imports should be at the top of your script
import concurrent.futures
import os
import sys


# all function and class definitions must be next
def add(x, y):
    """Function to return the sum of the two arguments"""
    return x + y

def product(x, y):
    """Function to return the product of the two arguments"""
    return x * y

if __name__ == "__main__":
    # __name__ 是当前模块名，当模块被直接运行时模块名为 __main__
    # 当模块被直接运行时，代码将被运行，当模块是被导入时，代码不被运行。
    # 起到了一个开关的作用。通常用于在作为程序运行的时候进行一些初始化操作
    a = [1, 2, 3, 4, 5]
    b = [6, 7, 8, 9, 10]

    # Now write your parallel code...
    etc. etc.
```

## Pool
`concurrent.futures` is one such library that allows users to easily parallelize tasks with two models:
+ 多线程 `ThreadPoolExecutor` 
+ 多进程 `ProcessPoolExecutor`

