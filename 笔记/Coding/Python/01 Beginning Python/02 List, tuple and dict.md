## List
List is the most basic data structure in Python.

To creat a list, use:
```Python
my_list = ["cat", "dog", 261]
print(my_list)
---
['cat', 'dog', 261]
```
The items in the list do not need to be of the same type, which means a list can contain ints, strings at the same time.

In addition, you can make an empty list:
```Python
my_list2 = [ ]
```

### Indexing
Each value in the list has a corresponding positional value, called an index, with the first index being 0, the second index being 1, and so on.

![[../../../assets/Coding/positive-indexes.png]]

Indexes can also start at the end, with an index of -1 for the last element, -2 for the previous bit, and so on.

![[../../../assets/Coding/positive-indexes.png]]

```Python
list = ['red', 'green', 'blue', 'yellow', 'white', 'black']
print(list[0])
print(list[1])
print(list[-1])
---
red
green
black
```

### Slicing
Using the form of square brackets [ ]
```Python
list1 = [3, 5, "green", 5.3, "house", 100, 1]
print(list1[2:5])
---
['green', 5.3, 'house']
```
That's because:

![[../../../assets/Coding/slicing.jpg]]

Between 2 and 5 are green, 5.3 and house.

Use [:num] or [num:] is OK too!
```Python
print(list1[:4])
print(list1[4:])
---
[3, 5, 'green', 5.3]
['house', 100, 1]
```

### Update the list
+ Use = can modify or update the data items in the list
+ Use the append () method to add list items
+ Use the del() function to delete the elements of the list

```Python
list2 = ['Google', 'Bing', 'Yahoo']
print(list1)
---
['Google', 'Bing', 'Yahoo']

list2[2] = 'Yandex'
print(list1)
---
['Google', 'Bing', 'Yandex']

list2.append('Baidu')
print(list1)
---
['Google', 'Bing', 'Yandex', 'Baidu']

del list2[2]
print(list1)
---
['Google', 'Bing', 'Baidu']
```

Furthermore, '+' can be used to combine lists
```Python
list3 = [1, 2, 3] + [4, 5, 6]
print(list3)
---
[1, 2, 3, 4, 5, 6]
```

## Tuple
Tuples and lists are of the same sequence type, and both can hold a set of data in a particular order, with no restrictions on data types.

The big difference between a tuple and a list is that the elements in the list can be changed as much as they want, while the elements in the tuple can not be changed, unless you replace the tuple as a whole.

```Python
tup1 = ('physics', 'chemistry', 1997, 2000)
tup2 = (1, 2, 3, 4, 5 )
print(tup1[0])
---
physics
```
The value of an element in a tuple is not allowed to be modified, but we can concatenate the composition of tuples:
```Python
tup3 = tup1 + tup2
print(tup3)
---
('physics', 'chemistry', 1997, 2000, 1, 2, 3, 4, 5)
```

## Dictionary
Each 'key => value' pair of the dictionary is separated by a colon, each pair is separated by a comma (,) , and the entire dictionary is enclosed in curly braces{ }
```Python
d = {key1 : value1, key2 : value2, key3 : value3 }
# Variable names are not recommended as 'dict'
```

The key must be unique, but the value is not.

```Python
# It can be written vertically
d1 = {
    'Name': 'Zelda',
    'Age': 18,
    'Class': 'First'
}
>>> print("tinydict['Name']:", tinydict['Name'])
tinydict['Name']: Zara
```
If one key have more than one value, they should be a list;
```Python
d2 = {
    'a' : [1, 2, 3],
    'b' : [4, 5]
}
```

### Update the Dict
```Python
d1['Age'] = 20 # Update
d1['School'] = " HappySchool " # Add new item
```

### Function & Method
```Python
sounds = {"cat": "meow", "dog": "woof", "horse": "neigh"}
print(len(sounds))
print(str(sounds))
---
3
{'cat': 'meow', 'dog': 'woof', 'horse': 'neigh'}
```

Built-in methods:
+ dict.items() Returns a list containing a tuple for each key value pair
+ dict.keys() Returns a list containing the dictionary's keys
+ dict.values() Returns a list of all the values in the dictionary
+ dict.clear() Removes all the elements from the dictionary
