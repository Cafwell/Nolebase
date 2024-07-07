# 1 What is markdown

+ 写作排版的语法：易读易写。
+ 只需要简单的符号。

## 价值，好处

+ 快速完成排版，即写作排版**不分离**；
+ 纯文本，不分离；
+ 所见即所得；方便转移，markdown的格式为通用的。


# 2 使用
^f98899

## 2.1 工具 
任何纯文本工具  
石墨文档  
Typora
…

## 2.2 语法

### 2.2.1 标题
“# 加空格” 

### 2.2.2 字体？
**加粗**
*斜体*
~~删除线~~
==高亮==
`代码词`
<u> 下划 </u>

<center>居中</center>
```Language
高亮代码
```


### 2.2.3 列表
#### 有序
数+点+空格
1. aa
2. bb
#### 无序
三种方式：
* 星号加空格
-  -号加空格
* +号加空格
	* 列表后使用Tab键可以缩进为子序
1. 有序同理
	1. 可以Tab缩进

### 2.2.4 引用，脚注
> This is a quote, just use '>'

脚注[^1]，快捷键Ctrl+Shift+6


### 2.2.5 分割
“---” 三个英文-号

### 2.2.6 Link
[Name](Markdown.md)

图片link：
<div align=center><img src=https://upload.wikimedia.org/wikipedia/commons/thumb/2/2f/Google_2015_logo.svg/220px-Google_2015_logo.svg.png height=37 width=110></div>

### 2.2.7 Hide
<details>
  <summary>Click</summary>
Hello
</details>

### 2.2.8 Hide a code block
<details>
  <summary><b>Codes</b></summary>
  <pre><code> 
print("Hello world")
    </code></pre>
</details>

### 2.2.9 任务列表
+ Ctrl + Enter
或者：
+ 已完成：**- [x]**+ **空格** + **内容**
* 未完成：**- [ ]**+ **空格** + **内容**
- [ ] Yes
- [x] No

### 2.2.8 双链
[[双链]]
""[[Markdown]]""

### 2.2.9 公式
使用美元符号与matlab语法

$f(x)=sin(x)+12$


# 3 建议
1. 非成对的Markdown 需要空一格
2. 段落之间空一行


[^1]: Here!