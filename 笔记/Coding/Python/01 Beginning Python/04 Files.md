## Files
Reading and writing files is the most common IO operation. Python has built-in functions for reading and writing files

There is a test.txt, contents are：
```
i like the movie
i ate an egg
```

test2.txt are:
```
31
4
20
12
```

### Read( )
```Python
f = open("sxl.txt", "r")   # "r" indicates read
lines = f.read()
print lines
f.close()
---
i like the movie
i ate an egg
```
Note that the file must be **closed** when it is finished, because the file object will occupy the os resources


### With open( )...as
```Python
with open("test2.txt") as f:
    for line in f:
        number = int(line)  # Should do a type conversion caz they were 'str'
        new_number = number + 17 
        print(new_number)
---
48
21
37
29

with open("test2.txt") as f:
    i = 0
    for line in f:
        number = int(line)
        i += number
    print("Sum is", i)
---
Sum is 135
```

#### A problem
```
Start by making a file called calc.txt with the following contents:  
4 * 6  
5 + 6  
457 - 75  
54 / 3  
4 + 6  
Make sure that you have the spaces between each number and the operator.
```

It should output something like:  
4 * 6 is 24  
5 + 6 is 11  
457 - 75 is 382  
54 / 3 is 18.0  
4 + 6 is 10  
There are some extra functions you will need to do this.
+ Firstly, the split function which takes a string and returns a list containing the string, split by spaces.   
+ Secondly, you may need the strip function which removes any whitespace from the beginning and end of a string, including removing newlines.

```Python
with open("calc.txt") as f:
    for line in f:
        a = str(eval(line))  # eval( ): Evaluates a string as a valid expression and returns the result of the evaluation.
        print(str.strip(line), "is", a, "\n", end="")  # str.strip( ): Removes the characters specified at the beginning and end of the string (either spaces or newlines by default)
        
# ps: use while and readline():
with open ("calc.txt") as f:
    line = f.readline()
    while line:
        b = str(eval(line))
        print(str.strip(line), "is", a, "\n", end="")
        line = f.readline()
```


_The example file only has integers in it, can you adjust your program so that it can accept floating point numbers as well?_   
_Can you adapt your program so that it can support code with or without spaces either side of the operator?_
