## For
A Python for loop can traverse any sequence of items, such as a list or a string.

```Python
words = ["are", "you", "good", "now"]  #list
for a in words:
    print(a)
---
are
you
good
now

sounds = {"cat": "meow", "dog": "woof", "horse": "neigh"}  #dict
for s in sounds.items():
    print(s)
---
('cat', 'meow')
('dog', 'woof')
('horse', 'neigh')

for letter in 'python':  #string
    print("Current letter: % s" % letter) # %s is used to format
---
p
y
t
h
o
n
```

### Else*
[参考](https://segmentfault.com/a/1190000039197454)
```Python
for i in [1, 2, 3]:
    print(i)
else:
    print('done')
---
1
2
3
done    
```

### Ranges
A built in function in Python that provides numbers in a range:

**range(start, stop, step)**
+ start: The default is from 0. For example, Range (5) is equivalent to Range (0, 5);
+ stop: Count to the end of the stop, but excluding Stop.For example: Range (0, 5) is end at 4 but not 5.
+ step: The default is 1. For example: Range (0, 5) is equivalent to Range (0, 5, 1)

```Python
for number in range(0, 5, 2):
    print(number)
---
0
2
4
```

### Enumerating
Used to combine a traversable data object (such as a list or a string) into an indexed sequence. Always used in for loops:

**enumerate(sequence, [start=0])**

```Python
seasons = ['Spring', 'Summer', 'Fall', 'Winter']
print(list(enumerate(seasons)))
---
[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]

seq = ['one', 'two', 'three']
for i, element in enumerate(seq, 1):
    print(i, element)
---
1 one
2 two
3 three
```


## While
**_For_** is used when the number of loops is known;  
**_While_** is used to repeat operations, not necessarily a specific number of times, as long as the condition is satisfied.

```Python
a = 2
while a < 10:
    print(a)
    a += 3 #same to a = a + 3
---
2
5
8
```
The while statement has two other important commands, continue and break. Continue to skip the loop, and break to exit the loop.

### Continue
```Python
i = 1
while i < 10:   
    i += 1
    if i%2 == 0:     # % means get the remainder, and the useage of if will show below
         continue
    print(i)         # get the odd nums
---
3
5
7
9
```
### Break
```Python
var = 1
while var == 1:  # Foever true，loop will execute indefinitely
    print(var)
#If the program is running from the command line, you are able to press Ctrl+C to force it to exit.

var = 1
while var == 1:
    print(var)
    break
---
1
 # With a break can stop it
```

## If
Conditional statements, a block of code that determined by the result of the execution of one or more statements (True or False).

It exits after only one cycle, so it often used together with loops.

```Python
ran = range(5)
for qwq, i in enumerate(ran,1):
    if i < 2:
        print(qwq, i, "is less than 2")
    elif i == 2:
        print(qwq, i, "is equal to 2")
    else:
        print(qwq, i, "is bigger than 2")
---
1 0 is less than 2
2 1 is less than 2
3 2 is equal to 2
4 3 is bigger than 2
5 4 is bigger than 2
```

### Booleans
If statements can be judged by some mathematical symbols to express their relationship： 
```
1 < 2  # Less than
12 == 10 + 2  # Are they equal to each other?
3.14159 != 3  # Are they *not* equal to each other
2 <= 23  # Less than or equal to
34 >= 3  # Greater than or equal to
```

### Elif
Used like example above. Used when a condition is judged to be more than one value.

``` Python
num = 10
if num < 0 or num > 10:    # you can use 'or' in the if
    print 'hello'
else:
    print 'undefine'
---
undefine
```
