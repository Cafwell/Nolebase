## Commands

### 查看文件和目录：
```bash
ls
ls -a  # 包含隐藏
```

### 当前位置：
```bash
pwd
mkdir -p ~/edu/lecture/docs # 递归创建目录，即使上级目录不存在，会按目录层级自动创建目录 mkdir -p xx/yy
```

### 文件夹大小
```bash
du -sh file_path
```

```
  -s, --summarize
         display only a total for each argument

  -h, --human-readable
         print sizes in human readable format (e.g., 1K 234M 2G)
```

### 新建文件夹：
```bash
mkdir folder_name
```

### 删除文件夹：
```bash
rmdir folder_name
```
*你必须在目录之外才能使用 `rmdir`  删除它。*

或者：
```bash
rm -rf folder_name
```


### 进入文件夹：
```bash
cd
cd .. # 返回上级，相对路径的方法
cd ../.. # 上两级
```

### / 号代表一定含义！
```bash
cd /step1/step2
# 绝对路径。它说转到根目录（文件夹），然后是 `step1` 文件夹，然后是从 `step1` 目录打开的 `step2` 文件夹。重

cd step1/step2/
# 相对路径。它表示从当前位置转到 `step1` 目录，然后转到 `step2` 目录
```

### ~ 为速记符：
If now in `/Users/ialbert/edu/lecture`
```bash
cd ~/edu/tmp # `~` 为主目录 `/Users/ialbert/`
```

### 返回主目录：
```bash
cd ~ 
cd
```

```bash
ls -l ~
```
total 12   
drwxr-xr-x  5 cafwell cafwell 4096 Mar 12 20:47 Bio-informatics   
drwxr-xr-x  3 cafwell cafwell 4096 Oct 28 11:22 Trans   
drwxr-xr-x 28 cafwell cafwell 4096 Mar 12 15:24 anaconda3   
### 移动：
```bash
mv a.txt folder

mv *.txt folder # `*` 充当通配符
```

### 删除文件：
使用 `rm` （删除）命令。潜在地， `rm` 是一个非常危险的命令；如果你用 `rm` 删除了一些东西，你将无法取回它；
```bash
rm -i file1 file2 file3
```

### 复制：
```bash
touch file1
cp file1 file2
```

### echo：
```bash
echo "Call me Ishmael."
Call me Ishmael.

echo "Call me Ishmael." > hello.txt
```

### print：
```bash
more hello.txt
less hello.txt
```

### cat：
```bash
echo "The primroses were over." >> hello.txt # 使用 `>>` 。该运算符将增补到文件而非覆盖
cat hello.txt
```
```
Call me Ishmael.
The primroses were over.
```

### 统计：
```bash
 $ wc hello.txt
 2  7 42 hello.txt # 42字节
```

```bash
wc -l hello.txt # 计算行数
2 hello.txt
```

### 查找；匹配：
将文字添加到 `hello.txt`：
```bash
vim hello.txt
```

```
Now is the winter of our discontent.
All children, except one, grow up.
The Galactic Empire was dying.
In a hole in the ground there lived a hobbit.
It was a pleasure to burn.
It was a bright, cold day in April, and the clocks were striking thirteen.
It was love at first sight.
I am an invisible man.
It was the day my grandmother exploded.
When he was nearly thirteen, my brother Jem got his arm badly broken at the elbow.
Marley was dead, to begin with.
```

```bash
grep was hello.txt
```

```bash
The Galactic Empire was dying.
It was a pleasure to burn.
It was a bright, cold day in April, and the clocks were striking thirteen.
It was love at first sight.
It was the day my grandmother exploded.
When he was nearly thirteen, my brother Jem got his arm badly broken at the elbow.
Marley was dead, to begin with.
```
-   ignore case when matching (`-i`)  
    匹配时忽略大小写 ( `-i` )
-   only match whole words (`-w`)  
    只匹配整个单词 ( `-w` )
-   show lines that don’t match a pattern (`-v`)  
    显示与模式不匹配的行 ( `-v` )
-   Use wildcard characters and other patterns to allow for alternatives (`*`, `.`, and `[]`)  
    使用通配符和其他模式以允许替代（ `*` 、 `.` 和 `[]` ）
+ lines AFTER or BEFORE (`-A` `-B`)

### 管道符：
```bash
$ grep was hello.txt | wc -c # `-c` 选项来计算匹配行中的字符
316
```

### Flag formats：
Traditionally Unix tools use two flag forms:  
-   short form: single minus `-` then one letter, like `-g`, ’-v`  
-   long form: double minus `--` then a word, like `--genome`, `--verbosity`  

Now some bioinformatics tools do not follow this tradition and use a single `-` character for both short and long options. `-g` and `-genome`.
### 下载：
wget: 最轻巧，纯粹的下载器

若网络不稳定时使用：
```bash
wget –tries=20 -c http://nginx.org/download/nginx-1.8.0.tar.gz
```

其他参数：
+ -b：background，下载大文件时放在后台去用
+ -spider：查看下载链接是否与有效
+ -l：从list中逐个下载，eg. `wget -i url_list.txt`

curl: _curl_ 可以下载，但是长项不在于下载，而在于模拟提交 web 数据。

`-o` 参数将服务器的回应保存成文件，等同于 `wget` 命令。
```bash
curl -o example.html https://www.example.com
```
上面命令将 `www.example.com` 保存成 `example.html`。

`-O` 参数将服务器回应保存成文件，并将 URL 的最后部分当作文件名。
 ```bash
curl -O https://www.example.com/foo/bar.html
```
上面命令将服务器回应保存成文件，文件名为 `bar.html`。

### 压缩包：
1、*.tar* 用 `tar –xvf` 解压  
2、*.gz* 用` gzip -d` 或者 `gunzip` 解压  
3、_.tar.gz 和_ *.tgz* 用 `tar –xzf` 解压  
4、*.bz2* 用 bzip2 -d 或者用 `bunzip2` 解压  
5、*.tar.bz2* 用 `tar –xjf` 解压  
6、*.Z* 用 `uncompress` 解压  
7、*.tar.Z* 用 `tar –xZf` 解压  
8、*.rar* 用 `unrar e` 解压 ([rarlab](https://www.rarlab.com/download.htm) needed)
9、*.zip* 用 `unzip` 解压

其中：
+ **-z**：有gzip属性的
+ **-j**：有bz2属性的  
+ **-Z**：有compress属性的
+ **-v**：显示所有过程
+ **-x**：解压
+ **-f**：使用档案名字，这是**最后一个参数**，后面只能接档案名。

zip文件无法解压、中文乱码
```shell
sudo apt-get install p7zip-full convmv # linux
brew install p7zip # MacOS

LANG=C 7za x file.zip  
convmv -f GBK -t utf8 --notest -r .
```


## A example

```bash
wget http://data.biostarhandbook.com/data/SGD_features.tab 
wget http://data.biostarhandbook.com/data/SGD_features.README
```

```bash
cat SGD_features.tab | wc 
```
prints the number of lines, words, and characters in the stream:  
打印流中的行数、字数和字符数：
```bash
16454  425719 3264490
```

```bash
cat SGD_features.tab | wc -l
```
只打印行数：
```bash
16454
```
>等效于 `wc -l SGD_features.tab`

从查看开头开始：
```bash
cat SGD_features.tab | head
```

寻找匹配：
```bash
cat SGD_features.tab  | grep YAL060W
```

着色版：
```bash
cat SGD_features.tab | grep YAL060W --color=always | head
```

储存匹配至文件：
```bash
cat SGD_features.tab | grep YAL060W > match.tab
```

特征类型（第 2 列）ORF 来表示蛋白质编码基因。您需要通过制表符剪切（cut）第二个字段。
```bash
$ cat SGD_features.tab | cut -f 2 > types.txt
$ head types.txt
ORF
CDS
ORF
CDS
ARS
telomere
telomeric_repeat
X_element
X_element_combinatorial_repeat
ORF
```

多少个基因？
```bash
cat SGD_features.tab | cut -f 2 | grep ORF | wc -l
```

```bash
cat types.txt | sort | head
```

`uniq` 命令将连续的相同单词合并为一个单词。`-c` 标记到 `uniq` 不仅会折叠连续条目，还会打印它们的计数。
```bash
$ cat types.txt | sort | uniq -c
    352 ARS
    196 ARS_consensus_sequence
   7074 CDS
     50 LTR_retrotransposon
   6604 ORF
      2 W_region
     32 X_element
     28 X_element_combinatorial_repeat
      3 X_region
     19 Y_prime_element
      3 Y_region
      3 Z1_region
      2 Z2_region
      6 blocked_reading_frame
     16 centromere
     16 centromere_DNA_Element_I
     16 centromere_DNA_Element_II
     16 centromere_DNA_Element_III
      8 external_transcribed_spacer_region
     24 five_prime_UTR_intron
      3 gene_group
      1 intein_encoding_region
      8 internal_transcribed_spacer_region
    377 intron
    383 long_terminal_repeat
      1 mating_type_region
      8 matrix_attachment_site
     18 ncRNA_gene
      3 non_transcribed_region
    484 noncoding_exon
     55 not in systematic sequence of S288C
     10 not physically mapped
      8 origin_of_replication
     47 plus_1_translational_frameshift
     12 pseudogene
     27 rRNA_gene
      2 silent_mating_type_cassette_array
      6 snRNA_gene
     77 snoRNA_gene
    299 tRNA_gene
      1 telomerase_RNA_gene
     32 telomere
     31 telomeric_repeat
     91 transposable_element_gene
```

对 `uniq` 的输出进行排序：
```bash
$ cat types.txt | sort | uniq -c | sort -rn | head
	7074 CDS
	6604 ORF
	484 noncoding_exon
	383 long_terminal_repeat
	377 intron
	352 ARS
	299 tRNA_gene
	196 ARS_consensus_sequence
	91 transposable_element_gene
	77 snoRNA_gene
```

**The pattern `sort | uniq -c | sort -rn` is perhaps the most useful simple unix command pattern that you will ever learn.**

A better approach：
```bash
$ sudo apt-get install ncbi-entrez-direct
$ cat types.txt | sort-uniq-count-rank | head
	7074    CDS
	6604    ORF
	484     noncoding_exon
	383     long_terminal_repeat
	377     intron
	352     ARS
	299     tRNA_gene
	196     ARS_consensus_sequence
	91      transposable_element_gene
	77      snoRNA_gene
```

One-liner：
```bash
cat SGD_features.tab | cut -f 2 | sort-uniq-count-rank | head
```


## Data compression

一般数据格式：
-   `ZIP`, extension `.zip`, program names are `zip/unzip`. Available on most platforms.
-   `GZIP` extension `.gz`, program names are `gzip/gunzip`. The most common compression format on Unix-like systems.
-   `BZIP2` extension `.bz/.bz2`, program names are `bzip2/bunzip2`.
-   `XZ` extension `.xz`. A more recent invention. Technically `bzip2` and `xz` are improvements over `gzip`. `gzip` is still quite prevalent for historical reasons and because it requires less memory.

In the majority of cases, bioinformatics data is distributed in `gz` and `tar.gz` formats.

**Tar 炸弹：**   
`tarball`是用 tar 创建的归档文件，`tar`可能是在 Linux 和其他类 Unix 操作系统上创建和打开归档文件最常用的程序。大多数`tarball`都被设计为在解压时为其内容创建一个新的专用目录。然而，有时候压缩文件爆炸到现有目录中可能是会造成麻烦，甚至是危险。它可能导致覆盖具有相同名称并且驻留在同一目录中的任何文件和目录。

防止不必要的`tar`爆炸的最简单方法是养成习惯，首先创建一个新的保护目录，然后在解压缩之前将`tarball`移动到其中。

## Bio-environment
Always do this at first:
```bash
conda activate bioinfo
```

If some apps missing, **Do not follow the recommendation below like:**
```bash
sudo apt install ...
```

Instead, do:
```bash
conda activate bioinfo
mamba install ...
```

or this is better:
```bash
conda create --name bedops
conda activate bedops
mamba install bedops
```
你可以根据需要创建尽可能多的环境。一个环境的软件越多，你就越有可能遇到冲突