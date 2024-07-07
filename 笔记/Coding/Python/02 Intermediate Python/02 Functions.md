## Using functions

In terms of *calling* functions, we have seen:
+ free functions like print() and range()
+ functions on objects (also called "methods") like my_list.append() and my_str.split()

There are also codes like:
+ data literals like 10, "hello" and [1, 2]
+ syntax like for house in town: and if height > 10:

There is a official web tells all those things:
https://docs.python.org -> 
1. [Library Reference](https://docs.python.org/3/library/index.html)
2. [Language Reference](https://docs.python.org/3/reference/index.html)

### Importing functions

The math module
```Python
>>> import math
# the square root function
>>> math.sqrt(25)
5.0
```
You can also grab a single function from a module:
```Python
>>> from math import sqrt
>>> math.sqrt(25)
5.0
```
A import * way
```Python
>>> from math import *
>>> print(cos(pi))
-1.0
```

## Writing fuctions
<details>
  <summary><i>Source</i></summary>
<i> This part based on https://zhuanlan.zhihu.com/p/54977805 / https://zhuanlan.zhihu.com/p/55167010 </i>
</details>

You can write your own functions:
```
def function_name():    # def is the keyword for a function
   
   contents of function     
```

A simple example is
```Python
def print_name():
     print("*"*20)
     print("== Hello Function ==")
     print("*"*20)

>>> print_name()
********************
== Hello Function ==
********************
```

### Function with parameters
Obviously the function above is not universal at all, so we need some Parameters.

+ **The parameter in ( ) at the time of definition, used to receive the parameter, are called parameter (formal parameter)**
+ **The parameter in ( ) at call time, used to pass to the function, are called argument (actual parameter)**

```Python
from decimal import *
def o_to_g(o):
    if o > 0:
        g = Decimal (o) * Decimal ('28.3495')
        return g
    else:
        print("value is a negtive num")
        return("Try again")

>>> o_to_g(12)
340.1940
```

```Python
def sum(a,b):
   print(f"{a} + {b} = {a + b}")

>>> sum(12,22)
12 + 22 = 34
None
```

### Return
The return value of a function in Python is the result given to the caller after the function has done something in the program.

```Python
def salary():
   s=10000
   print(f'Salary is {s}')
   return s

>>> salary()
Salary is 10000
10000
```
If you do not explicitly specify a return value, Python will, by default, return a value of None:
```
return None
```


#### Multiple return values in the function

A wrong way is like:
```Python
def func():
   a=1
   b=2
   c=3
   return a
   return b
   return c
print(func())
---
1
```

Illustration:
![[../../../assets/Coding/Return.PNG]]

So, to return multiple function values, you need a variable to store multiple function values and then return that variable:

```Python
# First way
d=[a,b,c]
  return d
# Second way  
  return [a,b,c]
# Third way
  return (a,b,c)
```

### Variable
There are:
+ Local variables
Useful only to a certain extent
```
def fun1():
   a = 6
   print(a + 1)
# Because a belongs to the fun () , a is a local variable and only works in this function.

def fun2():
   a = 7
   print(a + 1)
# Different functions can define local variables with the same name
```

+ Global variables
Defined outside the function
```Python
b = 1
def fun3():
   a = 5
   print(a + 2)
```

Functions can not modify global variables directly
```Python
c = 2
def fun4():
   print(c + 2)
fun4()
print(c)
---
2
```

To manipulate a global variable inside the function, you need to declare it global inside the function:
```Python
x = 1
def fung():
    global x
    x = 31  # After this declaration, the value of the global variable x has changed from 1 to 31
fung()
print(x)
---
31
```  

Ps: When the list and the Turtle as the global variables, you can use it without global declaration.

---

**Exercise：**

Having a list like: my_list = [5, 7, 34, 5, 3, 545]
if num > 10, print them like [34, 545]

<details>
  <summary><b>Answer</b></summary>
  <pre><code> 
def big(numbers):
    a = []
    for num in numbers:
        if num > 10:
            a.append(num)
    return a
my_list = [5, 7, 34, 5, 3, 545]
large_numbers = big(my_list)
print(large_numbers)
---
[34, 545]
    </code></pre>
</details>

---

## Modules
Make a file called convert.py：
```Python
def ounces_to_grams(weight):
    new_weight = weight * 28.3495
    return new_weight
```

Then you can to the console and run:
```
>>> import convert
>>> convert.ounces_to_grams(10)
283.495
```

### About calling
Creat a file - module.py
```Python
def hello():
    print("hello")
def bye():
    print("bye-bye")
```
And another file:
```Python
import module
hello() 
bye()
```
NameError!
Because the hello () and bye () are found only in this program.

To use module, there are two ways:
```Python
from module import hello, bye
hello()
bye()

or

import module
module.hello()
module.bye()
```

---

**Exercise：**

<details>
  <summary><b>Morse code module (morse.py):</b></summary>
  <pre><code> 
def code():
   convert = {
      'a': '.-', 'b': '-...', 'c': '-.-.', 'd': '-..', 'e': '.', 'f': '..-.',
      'g': '--.', 'h': '....', 'i': '..', 'j': '.---', 'k': '-.-', 'l': '.-..', 'm': '--',
      'n': '-.', 'o': '---', 'p': '.--.', 'q': '--.-', 'r': '.-.', 's': '...', 't': '-',
      'u': '..-', 'v': '...-', 'w': '.--', 'x': '-..-', 'y': '-.--', 'z': '--..',
      '0': '-----', '1': '.----', '2': '..---', '3': '...--', '4': '....-',
      '5': '.....', '6': '-....', '7': '--...', '8': '---..', '9': '----.', ' ': '/'
   }
   message = str(input("Your message is: "))
   morse_code = []
   for letter in message:
      m = convert[letter]
      morse_code.append(m)
   final_message = " ".join(morse_code)
   return final_message
    </code></pre>
</details>

```Python
import morse
a = morse.code()
print(a)
---
Your message is: thank you
- .... .- -. -.- / -.-- --- ..-
```
