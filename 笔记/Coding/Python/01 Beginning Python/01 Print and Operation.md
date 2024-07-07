## Start from print
``` Python
print("Hello from Python")
---
Hello from Python
```
The print () method is used to print the output and is the most common function.

## Data type

### String
**Use quotation marks (' ' or " ") to create a string.**  So "Hello from Python" is a string. 

Strings can also contain numbers: "114514"   
Or even be empty: " "  

### Floats
Or floating point numbers, are a way of representing numbers with decimal places : 3.14159

### Integers
For representing whole numbers : 42

## Assignment and operation

### Assignment
Assign a value with = 
``` Python
pi = 3.14159
print(pi)
---
3.14159
```
### Calculate
Just like a simple calculator - enter an expression in the interpreter, it will output the value of the expression.
``` Python
2 + 2
---
4

50 - 5*6
---
20

(50 - 5*6) / 4
---
5.0

8 / 5  # Always returns a floating point number
---
1.6
```

There will be errors in the calculation of floating point numbers; therefore the operation and comparison of floating-point numbers need decimal() to achieve:

``` Python
a = 1.12
b = a - 1.10
print(b)
---
0.020000000000000018

from decimal import *
getcontext().prec = 1
c = 1.13
d = 1.11
e = Decimal(c) - Decimal(d)
print(e)
---
0.02
# from import* see: https://blog.csdn.net/qq_30815237/article/details/93203934
```

### Combining variables
``` Python
greeting = "Hello "
name = "Python"
message = greeting + name
print(message)
---
Hello Python
```

Numerical variables can be calculated
``` Python
distance_in_miles = 30
distance_in_km = distance_in_miles * 1.60934
print(distance_in_km)
---
48.2802
```
