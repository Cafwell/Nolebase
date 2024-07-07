## Data Analysis

### Some simple analyses
Mean:
```Python
import pandas as pd
import numpy as np

rain = pd.read_csv("https://milliams.com/courses/data_analysis_python/rain.csv")
print(rain.mean())
---
Cardiff       97.400000
Stornoway    100.316892
Oxford        54.787500
Armagh        68.803571
dtype: float64
```

You can also learn more about the details of the data: ask how many years the average rainfall in each city exceeds 100mm:
```Python
rain[rain > 100].count()
---
Cardiff      13
Stornoway    74
Oxford        0
Armagh        1
dtype: int64
```

### File processing
Use [city_pop.csv](https://github.com/Cafwell/Learning-Python/blob/main/Data%20analysis/city_pop.csv)

![](https://raw.githubusercontent.com/Cafwell/Learning-Python/main/imgs/Data%20Analysis/city_pop.png)

```Python
import pandas as pd
import numpy as np

city_pop_file = "https://milliams.com/courses/data_analysis_python/city_pop.csv"
print(pd.read_csv(city_pop_file))
---
                          This is an example CSV file
0   The text at the top here is not part of the da...
1   to describe the file. You'll see this quite of...
2                     A -1 signifies a missing value.
3                              year;London;Paris;Rome
4                              2001;7.322;2.148;2.547
5                                   2006;7.652;;2.627
6                                      2008;-1;2.211;
7                                 2009;-1;2.234;2.734
8                                        2011;8.174;;
9                                 2012;-1;2.244;2.627
10                                       2015;8.615;;

```

It didnt need start from 'This is an example CSV file', use skiprow:
```Python
a = pd.read_csv(city_pop_file,
                skiprows= 5  # Start from line 5 (line start from 0)
    )
print(a)
---
   year;London;Paris;Rome
0  2001;7.322;2.148;2.547
1       2006;7.652;;2.627
2          2008;-1;2.211;
3     2009;-1;2.234;2.734
4            2011;8.174;;
5     2012;-1;2.244;2.627
6            2015;8.615;;
```

Separate the columns, use sep:
```Python
a = pd.read_csv(city_pop_file,
                skiprows= 5,
                sep=";",
    )
print(a)
---
   year  London  Paris   Rome
0  2001   7.322  2.148  2.547
1  2006   7.652    NaN  2.627
2  2008  -1.000  2.211    NaN
3  2009  -1.000  2.234  2.734
4  2011   8.174    NaN    NaN
5  2012  -1.000  2.244  2.627
6  2015   8.615    NaN    NaN
```
We use sep=";" because the origin csv use ; to seperate nums.   
If seperated by blank characters, we use:
+ sep=" "
+ sep="\s+"
+ delim_whitespace=True


There still some missing nums, if we want to add some custom values equivalent to missing values, use na_values:
```Python
a = pd.read_csv(city_pop_file,
                skiprows= 5,
                sep=";",
                na_values="-1" # All -1 will seem as NaN
    )
print(a)
---
   year  London  Paris   Rome
0  2001   7.322  2.148  2.547
1  2006   7.652    NaN  2.627
2  2008     NaN  2.211    NaN
3  2009     NaN  2.234  2.734
4  2011   8.174    NaN    NaN
5  2012     NaN  2.244  2.627
6  2015   8.615    NaN    NaN
```

Last, we want the 'year' column as the index for this DataFrame, use index_col:
```Python
a = pd.read_csv(city_pop_file,
                skiprows= 5,
                sep=";",
                na_values="-1"
                index_col="year"
    )
print(a)
---
      London  Paris   Rome
year                      
2001   7.322  2.148  2.547
2006   7.652    NaN  2.627
2008     NaN  2.211    NaN
2009     NaN  2.234  2.734
2011   8.174    NaN    NaN
2012     NaN  2.244  2.627
2015   8.615    NaN    NaN
```

#### Exercise
Read the file at https://milliams.com/courses/data_analysis_python/meantemp_monthly_totals.txt into Pandas (this data is originally from the Met Office and there's a description of the format there too under "Format for monthly CET series data").
This contains some historical weather data for a location in the UK. Import that file as a Pandas DataFrame using read_csv(), making sure that you set the index column, skip the appropriate rows, separate the columns correctly and cover all the possible NaN values.

<details>
  <summary><b>Answer</b></summary>
  <pre><code> 
import pandas as pd
temperature = pd.read_csv(
    "https://milliams.com/courses/data_analysis_python/meantemp_monthly_totals.txt",  # file name
    skiprows=4,  # skip first 6 rows of the header
    delim_whitespace=True,  # whitespace separated columns
    index_col="Year",  # Set the index
    na_values=["-99.9"],  # NaNs
)
temperature.head()
    </code></pre>
</details>

> Note: Set the pandas display parameters in Pycharm (maximum column number, row width, maximum column width) to display the full information.
> See https://www.pypandas.cn/docs/user_guide/options.html#overview


```
import pandas as pd
pd.set_option('display.max_colwidth', 1000)

{pd.set_option('display.max_columns', 1000)
pd.set_option('display.width', 1000)}
```

### Query the data
```Python
import pandas as pd
import numpy as np

tips = pd.read_csv("https://milliams.com/courses/data_analysis_python/tips.csv")
print(tips)
---
     total_bill   tip   day    time  size
0         16.99  0.71   Sun  Dinner     2
1         10.34  1.16   Sun  Dinner     3
2         21.01  2.45   Sun  Dinner     3
3         23.68  2.32   Sun  Dinner     2
4         24.59  2.53   Sun  Dinner     4
..          ...   ...   ...     ...   ...
239       29.03  4.14   Sat  Dinner     3
240       27.18  1.40   Sat  Dinner     2
241       22.67  1.40   Sat  Dinner     2
242       17.82  1.22   Sat  Dinner     2
243       18.78  2.10  Thur  Dinner     2

[244 rows x 5 columns]


print(tips["total_bill"])
---
0      16.99
1      10.34
2      21.01
3      23.68
4      24.59
       ...  
239    29.03
240    27.18
241    22.67
242    17.82
243    18.78
Name: total_bill, Length: 244, dtype: float64
```
Use [ ] will returns an Series!

```Python
tips[["total_bill", "tip"]]
---
     total_bill   tip
0         16.99  0.71
1         10.34  1.16
2         21.01  2.45
3         23.68  2.32
4         24.59  2.53
..          ...   ...
239       29.03  4.14
240       27.18  1.40
241       22.67  1.40
242       17.82  1.22
243       18.78  2.10

[244 rows x 2 columns]
```
This gives you back another DataFram.

#### Getting rows
use the .loc function:
```Python
print(tips.loc[2]) # get the 3rd row
---
total_bill     21.01
tip             2.45
day              Sun
time          Dinner
size               3
Name: 2, dtype: object
```
To grab a single value, add a name:
```Python
print(tips.loc[2,"total_bill"]) # get the 3rd row and only the value of "total_bill"
---
total_bill     21.01
```

### Statistics
```Python
tips["total_bill"].sum()  # sum up all num in "total_bill"
---
4827.77

tips["total_bill"].mean()  # find the mean of "total_bill"
---
19.78594262295082

tips["total_bill"].max()  # find out the biggest num
---
50.81

tips["total_bill"].min()  # find out the smallest num
---
3.07

tips["total_bill"].idxmax()  # find out the max num (50.81) came from which row 
---
170
```

### Acting on columns
You can apply the operation(+ - * /) to each row:
```Python
tips["tip"] * 100 
---
0       71.0
1      116.0
2      245.0
3      232.0
4      253.0
       ...  
239    414.0
240    140.0
241    140.0
242    122.0
243    210.0
Name: tip, Length: 244, dtype: float64
```

In addition, combine columns is also fine:
```Python
tips["tip"] / tips["total_bill"]
---
0      0.041789
1      0.112186
2      0.116611
3      0.097973
4      0.102887
         ...   
239    0.142611
240    0.051508
241    0.061756
242    0.068462
243    0.111821
Length: 244, dtype: float64
```

Adding new columns:
```Python
tip_percent = (tips["tip"] / tips["total_bill"]) * 100
tips["percent"] = tip_percent  # add a new column called "percent"
print(tips)
---
     total_bill   tip   day    time  size    percent
0         16.99  0.71   Sun  Dinner     2   4.178929
1         10.34  1.16   Sun  Dinner     3  11.218569
2         21.01  2.45   Sun  Dinner     3  11.661114
3         23.68  2.32   Sun  Dinner     2   9.797297
4         24.59  2.53   Sun  Dinner     4  10.288735
..          ...   ...   ...     ...   ...        ...
239       29.03  4.14   Sat  Dinner     3  14.261109
240       27.18  1.40   Sat  Dinner     2   5.150846
241       22.67  1.40   Sat  Dinner     2   6.175562
242       17.82  1.22   Sat  Dinner     2   6.846240
243       18.78  2.10  Thur  Dinner     2  11.182109

[244 rows x 6 columns]
```
