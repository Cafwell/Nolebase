
# 加载数据
向R语言中导入数据，然后进行分析，是一般的数据处理流程；这些数据包括 csv ，txt，tsv等。

## `.csv` files
导入csv方法很多，但这里使用 [vroom](https://github.com/r-lib/vroom) package来完成。

下载，调用包可以使用：
```R
install.packages("tidyverse", dependencies = T)
```

若从github中下载：
```R
install.packages("devtools") # 先安装devtools

library("devtools") # 调用

install_github("r-lib/vroom")
```

完成后调用
```R
library(vroom)

#  if you are using MacOS then the file path will be with “`/`”, if you are using windows then they will need to be “`\`”
wad_dat <- vroom("C:\Study\Biology\Bristol\rain.csv")

# or import from github raw data 
covid <- vroom("https://raw.githubusercontent.com/chrit88/Bioinformatics_data/master/Workshop%203/time_series_covid19_deaths_global.csv")
```

Note：读取多文件，使用 `vroom::vroom()`，见[文档](https://bookdown.org/zyf19940501/Rbook/data-vroom.html)

## `.RData` files
RData直接使用`load ()`函数加载到 R 中，其方式与. csv 文件非常相似；
但其缺点是不能在其他程序中打开它（如csv 文件可以在 Excel 中打开并查看）。
```R
##load in some RData
load("my_data\pathway\my_data.RData")
```

# 写出数据
导出`.csv`文件也可以通过`vroom`：
```R
##write out a .csv file
vroom_write(my_data, "a pathway\a data folder\the_name_of_my_data.csv")
```

若是`.RData`文件，使用`save`：
```R
save(my_data, file = "a pathway\a data folder\the_name_of_my_RData.RData")
```

# 数据处理

## 安装Tidyverse：
```R
install.packages("tidyverse", dependencies = T)

library(tidyverse)
── Attaching packages ──────────────────────────────────────────────── tidyverse 1.3.2 ──
✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
✔ tibble  3.1.8      ✔ dplyr   1.0.10
✔ tidyr   1.2.1      ✔ stringr 1.4.1 
✔ readr   2.1.2      ✔ forcats 0.5.2 
── Conflicts ─────────────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
```

tidyverse包的不同之处在于它使用管道（pipeline）- 一种将一系列函数链接在一起的一种方式。
pipeline函数为：`%>%`
类似与Linux 上的管道符 “|”
```R
# Useage:
my_data %>% function_1() %>% function_2()

# Example:
iris %>% head(n = 2)
---
     Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1          5.1         3.5          1.4         0.2  setosa
2          4.9         3.0          1.4         0.2  setosa

```

### data.table
快速，且能处理“大” 数据 (over 10GB)     
![data|650](https://img-blog.csdnimg.cn/img_convert/4648920de4fcfb97c0249f7d0b4e0183.png)

see [blog.](https://blog.csdn.net/Nh_code/article/details/125650578)

## 数据总览

首先了解*tibble* - 它是一种简单数据框，相对于传统的 data.frame 做出了一些修改。tibble包是 tidyverse 的核心 R包之一。

tibble 类型的类属依次为 `spec_tbl_df`, `tbl_df`, `tbl`, `data.frame`：

```R
library(tidyverse)
class(covid)
---
"spec_tbl_df"  "tbl_df"  "tbl"  "data.frame"
```

```R
##change the first two names of our data frame
names(covid)[1:2] <- c("Province.State", "Country.Region")
covid
```

可以使用`Hmisc`包中的`describe()`来获得数据的summary：
```R
library(Hmisc)
describe(covid_dat$`1/22/20`)
---
covid_dat$`1/22/20` 
       n  missing distinct     Info     Mean      Gmd 
     266        0        2    0.011  0.06391   0.1278 
                      
Value          0    17
Frequency    265     1
Proportion 0.996 0.004
```

## 数据重塑
### Rename
```R
names(covid_dat)[1:2] <- c("Province.State", "Country.Region")
head(covid_dat, n = 4)
---
A tibble: 4 x 180
  Provinc~1 Count~2   Lat  Long 1/22/~3 1/23/~4 1/24/~5 1/25/~6 1/26/~7 1/27/~8 1/28/~9 1/29/~*
  <chr>     <chr>   <dbl> <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
1 NA        Afghan~  33.9 67.7        0       0       0       0       0       0       0       0
2 NA        Albania  41.2 20.2        0       0       0       0       0       0       0       0
3 NA        Algeria  28.0  1.66       0       0       0       0       0       0       0       0
4 NA        Andorra  42.5  1.52       0       0       0       0       0       0       0       0
# ... with 168 more variables: `1/30/20` <dbl>, `1/31/20` <dbl>, `02/01/2020` <dbl>,
#   `02/02/2020` <dbl>, `02/03/2020` <dbl>, `02/04/2020` <dbl>, `02/05/2020` <dbl>,
#   `02/06/2020` <dbl>, `02/07/2020` <dbl>, `02/08/2020` <dbl>, `02/09/2020` <dbl>,
#   `02/10/2020` <dbl>, `02/11/2020` <dbl>, `02/12/2020` <dbl>, `2/13/20` <dbl>,
#   `2/14/20` <dbl>, `2/15/20` <dbl>, `2/16/20` <dbl>, `2/17/20` <dbl>, `2/18/20` <dbl>,
#   `2/19/20` <dbl>, `2/20/20` <dbl>, `2/21/20` <dbl>, `2/22/20` <dbl>, `2/23/20` <dbl>,
#   `2/24/20` <dbl>, `2/25/20` <dbl>, `2/26/20` <dbl>, `2/27/20` <dbl>, `2/28/20` <dbl>, ...
# i Use `colnames()` to see all variable names
```

---

### 理解`pivot_`

![Wide and long|400](https://raw.githubusercontent.com/gadenbuie/tidyexplain/main/images/static/png/original-dfs-tidy.png)

```R
# so this says take our data frame called covid_dat
covid_long <- covid_dat %>%
                ##and then apply this function
                pivot_longer(cols = -c(Province.State:Long),
                             names_to = "Date",
                             values_to = "Deaths")
head(covid_long, 5)
```    
其中，%>% 这个符号叫pipe，意思是把数据传递到后续的code中

```R
covid_long %>%
  pivot_wider(names_from = Date,
              values_from = Deaths)
head(unique(covid_long$Date), 20)
```                              

```R
covid_long$Date <- mdy(covid_long$Date)
covid_long$Date <- format(covid_long$Date, "%d-%m-%Y")
head(covid_long$Date, 20)
```


### Filter - 筛选

下载  [wbstats](https://github.com/gshs-ornl/wbstats) 包，调用，打开数据：
```R
library(wbstats)
library(tidyverse)
pop_data <- wb_data(indicator = "SP.POP.TOTL", 
                    start_date = 2002, 
                    end_date = 2020)
pop_data <- as_tibble(pop_data) # change into tibble form
```

Filter the data to include data from the year 2020 only:
```R
pop_2020 <- pop_data %>% 
            filter(date == 2020)

---
## # A tibble: 217 × 9
##    iso2c iso3c country             date SP.PO…¹ unit  obs_s…² footn…³ last_upd…⁴
##    <chr> <chr> <chr>              <dbl>   <dbl> <chr> <chr>   <chr>   <date>    
##  1 AF    AFG   Afghanistan         2020  3.89e7 <NA>  <NA>    <NA>    2022-09-16
##  2 AL    ALB   Albania             2020  2.84e6 <NA>  <NA>    <NA>    2022-09-16
##  3 DZ    DZA   Algeria             2020  4.39e7 <NA>  <NA>    <NA>    2022-09-16
##  4 AS    ASM   American Samoa      2020  5.52e4 <NA>  <NA>    <NA>    2022-09-16
##  5 AD    AND   Andorra             2020  7.73e4 <NA>  <NA>    <NA>    2022-09-16
##  6 AO    AGO   Angola              2020  3.29e7 <NA>  <NA>    <NA>    2022-09-16
##  7 AG    ATG   Antigua and Barbu…  2020  9.79e4 <NA>  <NA>    <NA>    2022-09-16
##  8 AR    ARG   Argentina           2020  4.54e7 <NA>  <NA>    <NA>    2022-09-16
##  9 AM    ARM   Armenia             2020  2.96e6 <NA>  <NA>    <NA>    2022-09-16
## 10 AW    ABW   Aruba               2020  1.07e5 <NA>  <NA>    <NA>    2022-09-16
## # … with 207 more rows, and abbreviated variable names ¹​SP.POP.TOTL,
## #   ²​obs_status, ³​footnote, ⁴​last_updated
```


### Summarize - 总结

```R
iris %>%
  summarize(mean(Sepal.Width))
---
    mean(Sepal.Width)
1            3.057333

# summarize是一个function，自己没有实际意义，它做什么，取决于summarize括号中放什么function；mean(Sepal.Width) 用来计算sepal.width的平均数
# 等效于mean(iris$Sepal.Length)
```
Note：可以指定名称，使用 `summarize("name" = function())`

### Group_by - 分组

```R
iris %>%
   group_by(Species)
```
group_by 当中可以调填入任何的categorical variable，他的作用是根据category，把数据分为小组。如果仅仅使用group_by，实际上没有什么作用，他需要和别人连用才可以。

```R
iris %>%
     group_by(Species) %>%
     summarize(mean(Sepal.Width))
---
# A tibble: 3 × 2
  Species    `mean(Sepal.Width)`
  <fct>                    <dbl>
1 setosa                    3.43
2 versicolor                2.77
3 virginica                 2.97
```
可以看到，sepal.width根据三种species分好了组然后输出了平均值。

同时，可以用summarize输出多个结果：
```R
iris %>%
  group_by(Species) %>%
  summarize(avg.sw = mean(Sepal.Width), 
			sd.sw = sd(Sepal.Width), 
			max.sw = max(Sepal.Width)
			)
--- 
# A tibble: 3 x 4
  Species    avg.sw sd.sw max.sw
  <fct>       <dbl> <dbl>  <dbl>
1 setosa       3.43 0.379    4.4
2 versicolor   2.77 0.314    3.4
3 virginica    2.97 0.322    3.8
```

### Join - 连接

#### a. 内连接 `inner_join`

内连接是最简单的一种连接，只要两个观测的键是相等的，即可匹配。
```R
x <- tribble(
  ~key, ~val_x,
     1, "x1",
     2, "x2",
     3, "x3")
y <- tribble(
  ~key, ~val_y,
     1, "y1",
     2, "y2",
     4, "y3")
x %>% 
  inner_join(y, by = "key")
---
> # A tibble: 2 x 3
>     key val_x val_y
>   <dbl> <chr> <chr>
> 1     1   x1    y1   
> 2     2   x2    y2
```

![[../../assets/Coding/join-inner.png|550]]

#### b. 外连接

外连接则保留**至少存在于一个表中**的观测。有 3 种类型：  
・左连接 `left_join`：保留 x 中的所有观测。   
![[../../assets/Coding/join-left.png|550]]
```R
x %>%
left_join(y, by = "key")
---
# A tibble: 3 x 3
    key val_x val_y
  <dbl> <chr> <chr>
1     1   x1    y1   
2     2   x2    y2   
3     3   x3    <NA> 
```

・右连接 `right_join`：保留 y 中的所有观测；   
![[../../assets/Coding/join-right.png|550]]
```R
x %>%
right_join(y, by = "key")
---
# A tibble: 3 x 3
    key val_x val_y
  <dbl> <chr> <chr>
1     1   x1    y1   
2     2   x2    y2   
3     4   <NA>  y3 
```

・全连接 `full_join`：保留 x 和 y 中的所有观测。   
![[../../assets/Coding/join-full.png|550]]
```R
x %>%
full_join(y, by = "key")
---
# A tibble: 4 x 3
    key val_x val_y
  <dbl> <chr> <chr>
1     1   x1    y1   
2     2   x2    y2   
3     3   x3    <NA> 
4     4   <NA>  y3 
```

以韦恩图 (Venn diagram)表示如下：   
![[../../assets/Coding/join-venn.png|400]]


### nest嵌套和嵌套还原

tidyr（tidyverse中的包） 提供了 `nest` 和 `unnest` 函数，嵌套函数指的是将指定列的对应元素‘折叠’为 list，缩小原有数据框的大小。
```R
iris %>% nest(data = -Species)
---
# A tibble: 3 × 2
  Species    data             
  <fct>      <list>           
1 setosa     <tibble [50 × 4]>
2 versicolor <tibble [50 × 4]>
3 virginica  <tibble [50 × 4]>
```

对数据框用 `group_by` 分组后调用 `nest` 函数就可以生成每个组的子数据框。
```R
iris %>% group_by(Species) %>% nest()
---
# A tibble: 3 × 2
# Groups:   Species [3]
  Species    data             
  <fct>      <list>           
1 setosa     <tibble [50 × 4]>
2 versicolor <tibble [50 × 4]>
3 virginica  <tibble [50 × 4]>
```

### mutate
可以用 `mutate` 保存为新的列表类型的列， 如果结果是数值型标量也可以保存为普通的数据框列。   
![[../../assets/Coding/mutate.jpg|400]]

上图中我们是要创建一个名为 new_variable 的新变量。分配给 new_variable 的值为 existing_var 乘以 2 的值。

> 这个函数只能用于数据框，不能在列表，矩阵，向量或其他数据结构中使用。