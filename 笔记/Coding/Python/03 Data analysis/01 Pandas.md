<div align=center><img src=https://pandas.pydata.org/static/img/pandas.svg height=139.2 weight=344.5></div>


## Introduction
A powerful tool set that based on Numpy (an open source numerical computing extension to Python) for analyzing structured data.

Offcial documents: https://pandas.pydata.org/pandas-docs/stable   
Chinese(中文网): https://www.pypandas.cn/docs

Start always from
```
import pandas as pd
import numpy as np
```

## Reading File
Pandas provides functionality for reading in all sorts of file formats, including:
+ Comma separated tables (or tab-separated or space-separated etc.)
+ Excel spreadsheets
+ HDF5 files
+ SQL databases

Use pd.read_[ ] function to read:

```Python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

rain = pd.read_csv("https://milliams.com/courses/data_analysis_python/rain.csv")
print(rains)
---
        Cardiff  Stornoway  Oxford  Armagh
1853      NaN        NaN    57.7    53.0
1854      NaN        NaN    37.5    69.8
1855      NaN        NaN    53.4    50.2
1856      NaN        NaN    57.2    55.0
1857      NaN        NaN    61.3    64.6
...       ...        ...     ...     ...
2016     99.3      100.0    54.8    61.4
2017     85.0      103.1    48.1    60.7
2018     99.3       96.8    48.9    67.6
2019    119.0      105.6    60.5    72.7
2020    117.6      121.1    64.2    71.3

[168 rows x 4 columns]
# NaN- Not a Number - Missing data


Or use head(n) to output n lines
print(rain.head(3))
---
        Cardiff  Stornoway  Oxford  Armagh
1853      NaN        NaN    57.7    53.0
1854      NaN        NaN    37.5    69.8
1855      NaN        NaN    53.4    50.2
```

Similarly
```
df_txt = pd.read_table('address')  # Read txt
df_excel = pd.read_excel('address')  # Read xls or xlsx (need install xlrd package)
```


## Data
There are two data structures:
+ Series(one-dimensional data)
+ DataFrame(two-dimensional data)

### Series
Create a Series:
```Python
s = pd.Series(np.random.randn(5),index=['a','b','c','d','e'],name='This is a Series',dtype='float64')
print(s)
---
a   -0.128472
b   -0.100648
c   -0.578571
d    1.251349
e   -0.268120
Name: This is a Series, dtype: float64
```

> About function np.random.randn()   
> For one parameter, np.random.randn(n) - it will output a one-dimensional array of n columns in a row.  
> For two parameter, np.random.randn(x,y) - outputs a two-dimensional array of x rows and y columns.

The most commonly used attributes are values, indexes, names, and dtype.

### DataFrame
Creat a DataFrame
```Python
df = pd.DataFrame({'c1':list('abcde'),'c2':range(5,10),'c3':[1.3,2.5,3.6,4.6,5.8]}, index=['one', 'two', 'three', 'four', 'five'])
print(df)
---
      c1  c2   c3
one    a   5  1.3
two    b   6  2.5
three  c   7  3.6
four   d   8  4.6
five   e   9  5.8
```
