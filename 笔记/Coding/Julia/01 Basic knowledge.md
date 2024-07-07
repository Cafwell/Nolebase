# Julia bases 

## 一个特性

```julia
abstract type Animal end
struct Fox <: Animal
    weight::Float64
end
struct Chicken <: Animal
    weight::Float64
end
```

```julia
fiona = Fox(4.2)
big_bird = Chicken(2.9)
combined_weight(A1::Animal, A2::Animal) = A1.weight + A2.weight
```
    combined_weight (generic function with 1 method)

```julia
function naive_trouble(A::Animal, B::Animal)
    if A isa Fox && B isa Chicken
        return true
    elseif A isa Chicken && B isa Fox
        return true
    elseif A isa Chicken && B isa Chicken
        return false
    end
end
naive_trouble(fiona, big_bird)
```
    true

```julia
trouble(F::Fox, C::Chicken) = true
trouble(C::Chicken, F::Fox) = true
trouble(C1::Chicken, C2::Chicken) = false

trouble(fiona, big_bird)
```
    true

```julia
dora = Chicken(2.2)
trouble(dora, big_bird)
```
    false


## 变量

使用 = 号给予变量：
```julia
name = "Julia"
age = 9
```

```julia
typeof(age)
```
    Int64

对于Int64能做什么操作？
```julia
first(methodswith(Int64),5)
```
5-element Vector{Method}:<ul><li> AbstractFloat(x::<b>Int64</b>) in Base at <a href="https://github.com/JuliaLang/julia/tree/17cfb8e65ead377bf1b4598d8a9869144142c84e/base/float.jl#L243" target="_blank">float.jl:243</a><li> Float16(x::<b>Int64</b>) in Base at <a href="https://github.com/JuliaLang/julia/tree/17cfb8e65ead377bf1b4598d8a9869144142c84e/base/float.jl#L146" target="_blank">float.jl:146</a><li> Float32(x::<b>Int64</b>) in Base at <a href="https://github.com/JuliaLang/julia/tree/17cfb8e65ead377bf1b4598d8a9869144142c84e/base/float.jl#L146" target="_blank">float.jl:146</a><li> Float64(x::<b>Int64</b>) in Base at <a href="https://github.com/JuliaLang/julia/tree/17cfb8e65ead377bf1b4598d8a9869144142c84e/base/float.jl#L146" target="_blank">float.jl:146</a><li> Int64(x::<b>Union{Bool, Int32, Int64, UInt32, UInt64, UInt8, Int128, Int16, Int8, UInt128, UInt16}</b>) in Core at <a href="file://C:\Users\YYL\AppData\Local\Programs\Julia-1.8.5\share\julia\base\boot.jl" target="_blank">boot.jl:764</a></ul>

## 数据类型

![[../../assets/Coding/Julia_datatypes.png|400]]

Int64类型的定义是这样的：  
`primitive type Int64 <: Signed 64 end`

这里应该重点关注的是`Int64 <: Signed`。操作符`<:`的含义是，其左侧的类型是其右侧类型的直接子类型。因此，Int64类型是Signed类型的直接子类型，或者说Int64类型直接继承了Signed类型。

当然，两个类型之间的关系也可以是间接的。例如，Signed类型的定义如下：  
`abstract type Signed <: Integer end`

```julia
Integer <: Number
```
    true

数值的内置表示被称作原始数值类型（numeric primitive），且整数和浮点数在代码中作为立即数时称作数值字面量（numeric literal）

### 整数类型
|类型|带符号？|比特数|最小值|最大值|
|---|---|---|---|---|
|Int8|	✓	|8| -2^7|2^7 - 1|
|UInt8|		|8| 0	|2^8 - 1|
|Int16|	✓	|16| -2^15|2^15 - 1|
|UInt16|	|16| 0	|2^16 - 1|
|Int32|	✓	|32| -2^31|2^31 - 1|
|UInt32|	|32| 0	|2^32 - 1|
|Int64|	✓	|64| -2^63 |2^63 - 1|
|UInt64|	|64| 0	|2^64 - 1|
|Int128|✓   |128| -2^127 |2^127 - 1|
|UInt128|	|128| 0	|2^128 - 1|
|Bool|N/A|8	|false (0)|	true (1)|

+ Signed是默认的，表示这个变量是有符号的，可以存储整数和负数。
+ Unsigned 则需要显示给出表示这个变量，没有符号值能存储数的大小，而且不能表示正负.

### 抽象类型

抽象类型不能被实例化，它组织了类型等级关系，方便程序员编程。如，编程时可针对任意整数类型，而不需指明是哪种具体的整数类型。

### 复合类型

复合类型的定义需要由关键字 struct 开始，由end结束：
```julia
# use struct
struct Language
    name::String
    title::String
    year_of_birth::Int64
    fast::Bool
end
```

```julia
fieldnames(Language)
```
    (:name, :title, :year_of_birth, :fast)

```julia
julia = Language("julia", "Rapid", 2012, true)
```
    Language("julia", "Rapid", 2012, true)


Struct是不可变的，要可变，需创建mutable struct：
```julia
mutable struct MLanguage
    name::String
    title::String
    year_of_birth::Int64
    fast::Bool
end
```

```julia
julia_mutable = MLanguage("Python", "Easy", 1991, false)
```
    MLanguage("Python", "Easy", 1991, false)

```julia
julia_mutable.title = "Nice"
julia_mutable
```
    MLanguage("Python", "Nice", 1991, false)


### Bool类型
```julia
!true
```
    false

```julia
(true && false) || (!false)
```
    true

```julia
1 == 1
```
    true



```julia
2 >= 1
```


    true

```julia
(1 != 10.0) || 3.14 < 4
```
    true

## Functions
```julia
function add(x::Int64,y::Int64)
    return x+y
end

function round_num(x::AbstractFloat)
    return round(x)
end

round_num(5.33)
```
    5.0


>Bash.show 打印方法
```julia
Base.show(io::IO, l::Language) = print(
    io, l.name, ", ",
    2023 - l.year_of_birth, " years old, ",
    "It is really ", l.title
)
julia
```
    julia, 11 years old, It is really Rapid


在函数调用时，分号是可选的：可以调用plot(x, y, width=2)或plot(x, y; width=2)，但前者的风格更为常见。

显式的分号只有在传递可变参数或下文中描述的需计算的关键字时是必要的。
```julia
function loge(x::Real; base::Real=2.7182818284590)
    return log(base, x)
end

loge(10)
```
    2.3025850929940845

```julia
# 指定已经默认了的参数
loge(10, base = 10)
```
    1.0

### 更简洁的方法
```julia
f(x,y) = x - y
f(4,1)
```
    3

```julia
∑(x,y) = x*y
∑(4,6)
```
    24

### 匿名函数
同python，一般在映射函数中使用。它的语法特别简单， 只需使用 ->

-> 的左侧定义参数名称。 -> 的右侧定义了想对左侧参数进行的操作
```julia
# e的x次方
map(x -> 2.7182818284590^x, loge(2))
```
    2.0

### If-Elseif-Else
```julia
a = 1
b = 2
if a < b
    "a is less than b"
elseif a > b
    "a is greater than b"
else
    "a is equal to b"
end
```
    "a is less than b"

### For
```julia
for i in 1:5 
    println(i)  #print() 在打印文本之后不包括换行符，而 println()包括换行符
end

for i in 1:10 
    print(i)
end
```
    1
    2
    3
    4
    5
    12345678910

### While
是for和if的结合体
```julia
i = 1
while i <= 6
    println(i)
    global i +=1
    while i == 6 +1
        println(i)
        break
    end
end
```
    1
    2
    3
    4
    5
    6
    7   

## 数据结构
### 广播（向量化）
```julia
[1,2,3,4] .+ 1
```
    4-element Vector{Int64}:
     2
     3
     4
     5

理论上来说，加减乘除也是一种函数，因此函数也可以进行广播操作：
```julia
loge.([1,2,3])
```
    3-element Vector{Float64}:
     0.0
     0.6931471805599569
     1.0986122886681282

变异函数：

如果要对函数参数进行写的操作的话，函数名后面要加!，这种函数叫做 “变异函数”(mutating) 或者 “取代函数”(in-place) 因为这种函数不只返回一个结果，还会在过程中修改参数的值。
```julia
function add_one!(V)
    for i in 1:length(V)
        V[i] += 1
    end
    return nothing
end

my_data = [1, 2, 3]
add_one!(my_data)

my_data
```
    3-element Vector{Int64}:
     2
     3
     4

### 字符串
```julia
typeof("This is a string")
```
    String

```julia
s = """
    This is a big multiline string with a nested "quotation".
    As you can see.
    It is still a String to Julia.
    """
 println(s)
```
    This is a big multiline string with a nested "quotation".
    As you can see.
    It is still a String to Julia.

#### 字符串操作
```julia
# combine
a = "Hello"
b = "Goodbye"
a * b
```
    "HelloGoodbye"

```julia
#有空格的连接
join([a, b], " ")
```
    "Hello Goodbye"

```julia
# $号，类似于f“{}”
"$a"
```
    "Hello"

```julia
# 查找
julia_string = "Julia is an amazing open source programming language"
println(contains(julia_string, "Julia"))
println(startswith(julia_string, "Julia"))
println(endswith(julia_string, "Julia"))
println(lowercase(julia_string))
println(uppercase(julia_string))
println(titlecase(julia_string))
println(lowercasefirst(julia_string))
```
    true
    true
    false
    julia is an amazing open source programming language
    JULIA IS AN AMAZING OPEN SOURCE PROGRAMMING LANGUAGE
    Julia Is An Amazing Open Source Programming Language
    julia is an amazing open source programming language  

```julia
replace(julia_string, "amazing" => "awesome") # 临时替换
```
    "Julia is an awesome open source programming language"

```julia
split(julia_string, " ")
```
    8-element Vector{SubString{String}}:
     "Julia"
     "is"
     "an"
     "amazing"
     "open"
     "source"
     "programming"
     "language"

```julia
my_num = 31
typeof(string(my_num))

```
    String

```julia
my_string = "31"
parse(Int64, my_string) + 5
```
    36

```julia
# tryparse不会转化非数字字符串，返回nothing，以此避免出错
tryparse(Int64, "A very non-numeric string")
```

### 元组
大体与python相同。是包含多种不同类型的固定长度容器且是不可变对象。
```julia
my_namedtuple = (i=1, f=3.14, s="Julia")
my_namedtuple.s
```
    "Julia"

```julia
x = 31
y = 3.14158
z = "python"
my_quick_namedtuple_2 = (; x, y, z)
```
    (x = 31, y = 3.14158, z = "python")


### 数组
```julia
col = collect(1:2:10) # 2为步长， collect会产生一个数组。
print(col)
```
    [1, 3, 5, 7, 9]

```julia
#自建数组
my_array = [1, 7, 8, 20]
my_array_2 = ["text", 4, :Symbol]
```
    3-element Vector{Any}:
      "text"
     4
      :Symbol


```julia
print(my_array_2[3])
```
    Symbol

```julia
my_vector = Array{Float64}(undef, 10)  # undef 表示數組尚未初始化為已知的值。
```
    10-element Vector{Float64}:
     0.0
     6.95309808894054e-310
     6.95310067990063e-310
     0.0
     6.95309808894054e-310
     6.95309808752395e-310
     0.0
     6.95309808894054e-310
     6.9530984144634e-310
     0.0

```julia
my_metrix = Matrix{Float64}(undef, 10, 2) #10 行 2 列
```
    10×2 Matrix{Float64}:
     1.5e-323  0.0
     5.0e-324  5.0e-324
     5.0e-324  1.0e-323
     0.0       5.0e-324
     0.0       5.0e-324
     0.0       5.0e-324
     0.0       5.0e-324
     0.0       1.0e-323
     0.0       0.0
     0.0       0.0

```julia
my_vector_zeros = zeros(10)
my_vector_fill = fill(3.0, 5, 5)
println(my_vector_zeros)

my_vector_fill
```
    [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
  
    5×5 Matrix{Float64}:
     3.0  3.0  3.0  3.0  3.0
     3.0  3.0  3.0  3.0  3.0
     3.0  3.0  3.0  3.0  3.0
     3.0  3.0  3.0  3.0  3.0
     3.0  3.0  3.0  3.0  3.0

```julia
my_matrix_π = Matrix{Float64}(undef, 2, 2)
fill!(my_matrix_π, 3.14)
```
    2×2 Matrix{Float64}:
     3.14  3.14
     3.14  3.14

#### 数组推断
```julia
ac = [x*y for x in 1:2 for y in 3:6]
print(ac)
```
    [3, 4, 5, 6, 6, 8, 10, 12]

```julia
ac_if = Float64[x^2 for x in 1:16 if isodd(x)]
print(ac_if)
```
    [1.0, 9.0, 25.0, 49.0, 81.0, 121.0, 169.0, 225.0]

```julia
cat_v = cat(ac,ac_if,dims=1)
print(cat_v)
```
    [3.0, 4.0, 5.0, 6.0, 6.0, 8.0, 10.0, 12.0, 1.0, 9.0, 25.0, 49.0, 81.0, 121.0, 169.0, 225.0]

```julia
cat_h = cat(ac,ac_if,dims=2)
```
    8×2 Matrix{Float64}:
      3.0    1.0
      4.0    9.0
      5.0   25.0
      6.0   49.0
      6.0   81.0
      8.0  121.0
     10.0  169.0
     12.0  225.0

#### 数组检测
```julia
println(eltype(cat_h))
println(length(cat_h))
println(ndims(cat_h))
println(size(cat_h)) # 8行2列
println(size(cat_h,2))
```
    Float64
    16
    2
    (8, 2)
    2

#### 操纵数组
1. 切片
2. 变换
3. 批量运算
4. 迭代

切片：
```julia
my_example_vector = [1, 2, 3, 4, 5]
my_example_matrix = [[1 2 3]
                     [4 5 6]
                     [7 8 9]]
```
    3×3 Matrix{Int64}:
     1  2  3
     4  5  6
     7  8  9

```julia
println(my_example_vector[2])  # 第一个 和 最后一个 元素定义了特殊的关键字： begin 和 end
println(my_example_matrix[2, 1])
my_example_matrix[2, :]
```
    2
    4

    3-element Vector{Int64}:
     4
     5
     6

```julia
my_example_matrix[3, :] = [17, 16, 15]
my_example_matrix
```
    3×3 Matrix{Int64}:
      1   2   3
      4   5   6
     17  16  15

变换：
```julia
six_vector = [1, 2, 3, 4, 5, 6]
three_two_matrix = reshape(six_vector, (3, 2))
three_two_matrix
```
    3×2 Matrix{Int64}:
     1  4
     2  5
     3  6

```julia
println(reshape(three_two_matrix, (6, )))
```
    [1, 2, 3, 4, 5, 6]
  
批量：
```julia
my_example_matrix .+ 100
```
    3×3 Matrix{Int64}:
     101  102  103
     104  105  106
     117  116  115

```julia
map(x -> x+200, my_example_matrix)
```
    3×3 Matrix{Int64}:
     201  202  203
     204  205  206
     217  216  215

```julia
map(x -> x + 100, my_example_matrix[:, 3])
```
    3-element Vector{Int64}:
     103
     106
     115

按行或列 - 行（dims=1）；列（dims=2）

理解为 dim=1 , 每一列向下加，结果得到的是行。 dim=2 亦然
```julia
mapslices(sum, my_example_matrix; dims=1) 
```
    1×3 Matrix{Int64}:
     22  23  24

```julia
mapslices(sum, my_example_matrix; dims=2) 
```
    3×1 Matrix{Int64}:
      6
     15
     48

迭代：
```julia
simple_vector = [1, 2, 3]
empty_vector = Int64[]
for i in simple_vector
    push!(empty_vector, i) # push!(pts,xk), pts是数组，xk是要添加的元素
end
empty_vector
```
    3-element Vector{Int64}:
     1
     2
     3

```julia
reduce(+, simple_vector)
```
    6

#### Pair
是一种包含两个对象的数据结构 （一般属于彼此）。
```julia
my_pair = "Julia" => 42 # = and >
```
    "Julia" => 42

```julia
println(first(my_pair))
println(last(my_pair))
```
    Julia
    42

### 字典
```julia
# 2 ways
name2number_map = Dict([("one", 1), ("two", 2)])
name2number_map = Dict("one" => 1, "two" => 2)
```
    Dict{String, Int64} with 2 entries:
      "two" => 2
      "one" => 1

```julia
name2number_map["one"]
```
    1

```julia
name2number_map["three"] = 3
name2number_map
```
    Dict{String, Int64} with 3 entries:
      "two"   => 2
      "one"   => 1
      "three" => 3

```julia
delete!(name2number_map, "three")
# pop!(name2number_map, "three")，会返回"three"的值3
```
    Dict{String, Int64} with 2 entries:
      "two" => 2
      "one" => 1

✲ zip方法构建字典
```julia
A = ["one", "two", "three"]
B = [1, 2, 3]
name2number_map = Dict(zip(A, B))
```
    Dict{String, Int64} with 3 entries:
      "two"   => 2
      "one"   => 1
      "three" => 3

### Symbol

Symbol不是一种数据结构，而是一种类型，是一个未评价的或带引号的表达式，类似于字符串。使用Symbol的好处是会少键入一个字符，即:some_text相对于"some text".

它通过在名称前加一个冒号来创建的。符号对于元编程是很有用的，元编程是指编写可以操作其他代码的代码。例如，你可以使用符号来构造表达式，这些表达式可以在以后被评估。
```julia
sym = :a_symbol
```
    :a_symbol

```julia
s = string(sym)
```
    "a_symbol"

```julia
sym = Symbol(s)
```
    :a_symbol

### Splat运算符
```julia
add_ele(a,b,c,d,e) = a+b+c+d+e
my_collection = [2,3,4,4,5]

add_ele(my_collection...)

```
    18

展开运算符...，它将接收一个集合（通常是数组，向量，元组，或 range）并将其转化为参数序列而进行计算
```julia
add_ele(1:5...)
```
    15