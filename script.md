[Toc]





# I. Bioinfo Module

### 1. 测序全流程

https://luohao-brian.gitbooks.io/gene_sequencing_book/content/di-3-8282-shu-ju-zhi-kong.html

### 2. 名词解释

- **DHS**: DNase I Hypersensitivity Site, 常用于指示**TF binding site, mostly, enhancers and promoters**
- **UMI**: unique molecular identifiers 

### 3. Bioinfo数据格式详细信息

<img src="https://raw.githubusercontent.com/mglnbk/picgo/main/data_format.png?token=ANOO3FPMJQ7GSFRCJB5G6IDC3KG5Q" style="zoom:50%;" />

#### I. FASTA数据格式

**FASTA 格式文件中的每个序列信息由两个部分组成：**

**1. 描述行** (The description line, Defline, Header or Identifier line): 以一个大于号(">")开头，内容可以随意，但不能有重复，相当于身份识别信息。

**2. 序列行** (Sequence Line)：一行或多行的核苷酸序列或肽序列，其中碱基对或氨基酸使用单字母代码表示。

- 文件每行的字母一般不应超过80个字符

```shell
>VIT_1_a_b_c_3|123131|ref|NM_aaaa|mus musculus
ACGTGCTAGCTAGCTGTTTGCTCCGA
>VIT_1_a_b_4|123131|ref|NM_aaaa|mus musculus
AACGTCCCGGGATAGCTGCTACGATGCATGCTCGCTAG
```

![](https://raw.githubusercontent.com/mglnbk/picgo/main/read_alignment.png?token=ANOO3FPVKXNQZTZDJUUR5YDC3IVGI)

可以说每一个FASTA的序列信息代表着一个READ

**要注意FASTA格式也可以用于表示多种测序结果，比如蛋白质氨基酸**

```shell
>gi|129295|sp|P01013|OVAX_CHICK GENE X PROTEIN (OVALBUMIN-RELATED)
QIKDLLVSSSTDLDTTLVLVNAIYFKGMWKTAFNAEDTREMPFHVTKQESKPVQMMCMNNSFNVATLPAE
KMKILELPFASGDLSMLVLLPDEVSDLERIEKTINFEKLTEWTNPNTMEKRRVKVYLPQMKIEEKYNLTS
FLFLIKHNPTNTIVYFGRYWSP
```

#### II. FASTAQ数据格式

相比FASTA信息多了一个Quality Control的ASCII码，pos2pos

```shell
@EAS54_6_R1_2_1_413_324
CCCTTCTTGTCTTCAGCGTTTCTCC
+
;;3;;;;;;;;;;;;7;;;;;;;88
@EAS54_6_R1_2_1_540_792
TTGGCAGGCCAAGGCCGATGGATCA
+
;;;;;;;;;;;7;;;;;-;;;3;83
@EAS54_6_R1_2_1_443_348
GTTGCTTCTGGCGTGGGTGGGGGGG
+EAS54_6_R1_2_1_443_348
;;;;;;;;;;;9;7;;.7;393333
```

- 第一行：@开头的read标志符号
- 第二行：序列信息
- 第三行：+号后可加可不加
- 第四行：ASCII码质量分数pos2pos

#### III. BAM/SAM数据格式

- **SAM**

  SAM(Sequence Alignment/Map)格式是⼀种通⽤的⽐对格式,⽤来存储reads到参考序列的⽐对信息。

- **BAM**

#### IV. BED/BEDPE数据格式

1. **Chromosome**
2. **Chr Start**
3. **Chr End**
4. **Supplementary Info**



### 4. 什么叫做富集分析

首先明确富集分析需要至少两个对象，才能比较是否富集了。下描述富集分析的概念，

我们作两个样本之间的基因谱表达时，我们会得到一组不同表达的基因，记为$n$个。

我们想要分析的是某一个功能(GO Term或者KEGG或者其他Reactome等)在我们这两个样本间是否有差异。但是由于各种误差等因素，我们无法确认这个$n$个基因是否确实对该功能产生了影响，我们寻求统计学帮助。我们遵循这样一个原则，如果我们选出来的$n$个基因对某一个功能而言不是随机选出来的，我们就认为这$n$个基因对这个功能产生了影响。
$$
假设参与分析的总基因数量为N，我们通过基因谱表达得到了n个差异表达基因。\\
我们选定一个Go\, term, 我们想要看是否这n个基因对这个功能产生了影响。 \\
$$

- **建立假设**
  $$
  H_0: 这选取的n个基因从N个基因中选取得到的观测分布与从N个基因中随机选取n个基因的分布相同
  $$

- **分布假设**
  $$
  N个基因中带有某个功能的基因数量为M个，那么从中选取n个，令X,r.v.代表n个中带有功能的基因数量 \\
  P(X=k)=\frac{{M \choose k}{N-M \choose n-k}}{N \choose n} \\
  即X服从超几何分布
  $$

- **建立统计量**

​		这一步骤略去，即寻找一个有解析式或者无解析式的统计量，该统计量满足一个特定的分布，该统计量是关于差异表达的。

- **检验**

  将差异表达的样本数据带入，通常这些数据都是频率样本数据。得到统计量大小，再按照统计量分布计算双尾或者单尾的概率，如果小于 $1-置信度\alpha$。那么就拒绝$H_0$假设，则这些差异表达基因确实在这Go term中产生了影响。故此也可以计算$p$值。



### 5. 设置端口转移

```shell
ssh -N -L 9898:localhost:9898 sunzehui@172.16.75.132
```

要注意到的是Pycharm的端口只适用于本地的Localhost端口，而不适用于直接的服务器端口互联。



### 6. CRISPERi-FLOWish

- 两个生物大分子，Cas9蛋白和gRNA（guide RNA）组成了CRISPR/Cas9基因编辑系统。在细胞内，Cas9蛋白与gRNA形成复合物，能特异性鉴别出靶序列。此过程中，Cas9蛋白负责将复合物定点到靶DNA和剪切靶DNA。Cas9蛋白有6个结构域，分别是Rec I、Rec II、Bridge Helix、PAM Interacting、**HNH**和**RuvC**。Rec I是6个中最大的一个结构域，负责结合gRNA。一旦结合了靶DNA，Bridge Helix就负责启动剪切。PAM interacting结构域赋予Cas9对PAM序列的特异性要求，负责启动与靶DNA的结合。**HNH和RuvC都是核酸酶结构域，剪切单链DNA**。
- dCas9蛋白指的是通过往**RuvC和NHN**两个核酸酶结构域分别导入氨基酸突变D10A和H840A，使得Cas9蛋白失去切割DNA活性，但仍保留结合DNA的能力，称为dead Cas9
- 为了进一步提高转录抑制的效率，dCas9融合了一个基因抑制结构域，如**KRAB（krüppel-associated box）结构域**，这样的蛋白称之为dCas9-KRAB
- **RNA-FISH**：如果待检测的细胞或组织切片上的靶核酸与所用的核酸探针是同源互补的，二者经变性-退火-复性，即可形成靶核酸与核酸探针的杂交体。将核酸探针的某一种核苷酸标记上报告分子如生物素或直接标记荧光素，可利用该报告分子与荧光素标记的特异亲和素之间的免疫化学反应或直接经荧光检测体系在镜下对待测核酸(mRNA、lncRNA、circRNA、miRNA)进行定性、半定量或相对定位分析的一种实验方法。
- **FACS**：根据荧光流式分选

![](https://raw.githubusercontent.com/mglnbk/picgo/main/CRISPERi-FlowFISH.jpg?token=ANOO3FKZ3WEH3653L3PEAXDC3E54M)



### 7. Hi-C dataset Normalization Method

- To compensate for the bias brought by high variation in the genomic data
- Six methods: <https://doi.org/10.2144/btn-2019-0105>



### 8. Hi-C data Query

Power Law?

- can be downloaded from JuiceBox database maintained by Aiden Lab. 



### 9. eQTL 表达数量性状基因位点

eQTL指的是在染色体DNA条带上存在的一段可以控制基因的表达数量性状的DNA片段，分为cis-eQTL以及trans-eQTL，cis主要调控周围邻近1Mb左右的基因区域，而trans则是远程的区域，一般来说我们需要去结合SNP信息和基因表达矩阵来进行eQTL分析，找出表达位点。



### 10. single-cell RNA sequencing

![](https://github.com/mglnbk/picgo/blob/main/read_alignment.png)

Each cell is labeled with a unique molucule identifier with which can be identified at the last step of clustering. 

- IVT can be used to avoid the bias brought by PCR
- Unique label is called UMI. Each UMI represents a single cell



### 11. `samtools`的最常见用法 sort and index

```shell
samtools sort -@ 8 test.bam -o test.sorted.bam` && `samtools index -@ 8 test.sorted.bam
```



### 12. macs2 call peaks 参数解释

```shell
macs2 callpeak \
-t example_chr22/input_data/Chromatin/wgEncodeUwDnaseK562AlnRep1.chr22.bam \
-n wgEncodeUwDnaseK562AlnRep1.chr22.macs2 \
-f BAM \
-g hs \
-p .1 \
--call-summits \
--outdir example_chr22/ABC_output/Peaks/ 

```

- `-t` Treatment file, 要处理的对象包括 `ELAND`, `BED`, `ELANDMULTI`, `ELANDEXPORT`, `SAM`, `BAM`, `BOWTIE`, `BAMPE`, or `BEDPE`格式，required

- `-n` Saved file name

- `-f` 文件格式`ELAND`, `BED`, `ELANDMULTI`, `ELANDEXPORT`, `SAM`, `BAM`, `BOWTIE`, `BAMPE`, or `BEDPE`格式

- `-g` Genome size, 

  The default *hs* -- 2.7e9 is recommended for human genome

- `-p` The p-value cutoff. If `-p` is specified, MACS2 will use p-value instead of q-value.

- `--CALL-SUMMITS` MACS will now reanalyze the shape of signal profile (p or q-score depending on the cutoff setting) to deconvolve subpeaks within each peak called from the general procedure. It's highly recommended to detect adjacent binding events. While used, the output subpeaks of a big peak region will have the same peak boundaries, and different scores and peak summit positions.

- `--outdir` 输出文件夹

- 输出文件：

  - NAME_peaks.xls
  - NAME_peaks.narrowPeak
  - NAME_summits.bed
  - NAME_peaks.broadPeak
  - NAME_peaks.gappedPeak
  - NAME_model.r: `Rscript ....r`





### 13. bedtools 用法

- narrowPeak文件进行sort

  ```bedtools sort -faidx chrom_size -i your_narrowPeak > your_narrowPeak.sorted```



### 14. pip pkg `pyfaidx`

- 可以用来快速获知某个参考基因组的染色体大小

  `faidx -i chromsizes input.fa > output.chromsizes`





### 15. 使用igv overlay tracks 实现不同的IGV可视化

- overlay之后可以对不同的track设置不同颜色，从而对不同的file实现可视化



### 16. 使用UCSC-bigWigMerge工具进行bigwig的merge



### 17. pip install 和 anaconda

using pip install will only install tensorflow-datasets in system python installation, which cant be used inside anaconda environment

so I need to use conda install instead


### 18. pandas的`stack()`函数、`pivot_table()`、`melt()`

`stack()`：行列的excel-like的表格转entry-like的条目式表格

`pivot_table()`：entry-like的条目式表格转行列的excel-like的表格

`melt()`：行列的excel-like的表格转entry-like的条目式表格

### 19. git的网络设置
由于墙的原因，Github在一些时候会有http timeout的现象发生。故此我们给出以下的解决方案，在挂起VPN时查看其混合代理接口，只需要将终端git指令转发到该VPN代理端口即可。
```bash
git config --global http.proxy 127.0.0.1:7890
git config --global https.proxy 127.0.0.1:7890
```

### 20. git的缓冲区清空
由于一些自己的胡乱submit的缘故，导致缓冲区有点混乱，此时可以使用以下命令清理缓存。
```bash
git rm -r --cached .
```
在之后使用以下命令将working tree上的版本记录退回。
```bash
git reset --soft HEAD~1
```

# II. Server Module

> 大部分都是描述了实验中遇到的一系列问题和解决方法 

### 1. Pycharm Jupyter的配置

Pycharm配置jupyter

1. 下载jupyter notebook
2. 验证jupyter notebook是否已经下载完毕
3. 在Bash内部选取需要的虚拟环境
4. 输入`jupyter notebook --no-browser`来启动当前环境下的jupyter notebook服务端，Bash终端会显示当前环境下的Jupyter服务端口号和相应的Token
5. 在Pycharm里面新建一个ipynb文件，选择`Configure Jupyter Server`，在命令行选取该服务端口号粘贴复制进Configured Server点击Apply and OK即可



### 2. _tkinter.TclError: no display name and no $DISPLAY environment variable解决办法

https://blog.csdn.net/asty9000/article/details/88577872 方法二

```bash
#当前shell
export MPLBACKEND=Agg
#只为当前脚本test.py
MPLBACKEND=Agg python test.py
```

 

### 3. 应用正则表达式筛选特定行

```python
sp_phenotype[['icgc_specimen_id', 'specimen_type']][sp_phenotype['specimen_type'].str.contains(r'Metastatic', regex=True)]
```

如果遇到不是`str`类型的话，则增添一个类型转换即可

```python
[sp_phenotype['specimen_type']..astype(str).str.contains(r'Metastatic', regex=True)]
```



### 4. 使用Counter类进行计数活动

- 可以用于相当多的可迭代对象，包括pandas.series

```python
from collections import Counter
cout = Counter(iterable)
# 几个常用的类函数
cout.elements() # 返回一个迭代器 
cout.most_common(n) # 选取前n个最多的
cout.subtract(cout2) # 计数对应元素相减，等价于cout-cout2 
```



### 5. 使用rename来更改列名

```python
# 常用的做法是传入一个字典
df.rename(columns = {'a':'c'}, inplace = True)
```



### 6. 更改索引

- 数字索引更改

```python
df.reindex(index=list(range(1, df.shape[0])))
```



### 7. 筛选Dataframe的特定行

- 筛选在一个series中出现的行

  `df.loc[df['column_name'].isin(some_values)]  # some_values是可迭代对象`



### 8. set_index和reset_index

- `set_index`
- `reset_index`



### 9. Vim-Learn

**需要安装的插件**

- NERDTree
- YouCompleteMe



**ESC mode**:

- i 切换至INSERT
- o 切换到下一行并转入INSERT
- e 跳到词尾
- E 同e
- /+{find patterns} 寻找相应的字符串
- :wq,q,q!
- G 跳到文件末尾
- gg 跳到文件头
- shift+6 跳到行的第一个字符
- shift+4 跳到行的最后一个字符
- u 撤销，U大撤销
- Ctrl+r 恢复撤销

**INSERT mode**:

- i 进入INSERT模式 





### 10. df.apply()

```python
df.apply(lambda x: get_hg38_pos(x["#chr"], x["start"]), axis=1)
```
- axis = 1: 对行做apply, 沿着列进行
- lambda x: 相当于自定义函数$x = f(x)$
- note: replace is true, no replication



### 11. Useful snippet to download 

```python
import requests, json

def get_meta_data(fname):
    # Force return from the server in JSON format
    headers = {'accept': 'application/json'}

    url = f"https://www.encodeproject.org/files/{fname}/"

    # GET the object
    response = requests.get(url, headers=headers)

    # Extract the JSON response as a Python dictionary
    biosample = response.json()

    return biosample

# move files

def fmove(src, dest):
    # src = '../data/encode/' + src
    # dest = '../data/encode/' + dest
    dest_dir = os.path.dirname(dest)
    try:
        os.makedirs(dest_dir)
    except os.error as e:
        pass #Assume it exists.  This could fail if you don't have permissions, etc...
    shutil.move(src,dest)

```



### 12. 本地端口监听服务器端口

```shell
ssh -N -L 9897:localhost:9898 sunzehui@x.x.x.x
```
- 本地的9897端口监听远端服务器的9898端口，方便Pycharm的jupyter server configuration, 因为pycharm不能远程连接jupyter server port



### 13. 远程jupyter的启动位置决定了工作目录

[I 10:09:03.499 NotebookApp] Serving notebooks from local directory:



### 14. Git push 时候报错 SSL 服务证书未经过签名导致

解决办法设置 将ssl认证为false

```shell
git config --global http.sslVerify "false"
```



### 15. Git push 时候由于使用VPN代理后导致push time_out

运行以下代码即可

```shell
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy http://127.0.0.1:1080
git config --global --unset http.proxy
git config --global --unset https.proxy
```

即设置代理再取消代理

https://greasyfork.org/zh-CN/scripts/412245-github-%E5%A2%9E%E5%BC%BA-%E9%AB%98%E9%80%9F%E4%B8%8B%E8%BD%BD



### 16. nohup + command + &

后台挂起，按Ctrl+C，让进程在后台运行。



### 17. 查看文件行数

```shell
wc -l [file_name] 
```



### 18. infoseq [fastaq] 查看文件

- infoseq 包含在emboss的软件包中

```shell
infoseq [file_name]
```



### 19. Gene Naming

- Typically standardized gene naming is assigned by the HUGO Gene Nomenclature Committee (HGNC)



### 20. 下载文章中的Data来源（SRA，GEO）

使用NCBI 提供的 Entrez接口

- **已知SRA Project ID号**

```shell
esearch -db sra -query PRJEB13208 | efetch -format runinfo > runinfo.csv
```

- **已知GEO Project ID号**

```shell
esearch -db gdb -query GSE1212312 | efetch -format runinfo > runinfo.csv
```

- **使用cut命令，提取SRR符号**

```shell
cat runinfo.csv | cut -f 1,22 -d , | head
```



### 21. 存储DNA data的网站

- **NCBI**: National Center for Biotechnology Information3
- **EMBL**: European Molecular Biology Laboratory4
- **DDBJ**: DNA Data Bank of Japan5



### 22. 使用scp下载远程的文件与本地

```shell
scp -r sunzehui@ip_address:/path/to/the/file_or_directory /local/diretory
```

- 当传输文件夹的时候只需要使用`-r`即可，需要设置端口的时候使用**大写**的P来进行设置

```shell
scp -P port_nb sunzehui@ip:file_path local_path
```



### 23. conda fails to create environment from yml

> scripted from https://stackoverflow.com/questions/55554431/conda-fails-to-create-environment-from-yml
- Note that `.yml` is platform specific, to prevent it from happening

```shell
conda env export -n py36 -f py36.yml --no-builds
```
 this command can be done at some issue, hwoever, we recommend the command below

```shell
conda env export -n py36 -n py36 -f py36.yml --from-history
```
The commands above will create a new .yml file which you can use
- 要注意到这样创建的yml文件的首行name是你在运行命令时候的环境名字，为避免重复需要去改变一下

```shell
conda env create -f environment.yml
```



### 24. Use zsh and Oh-My-Zsh in the remote server

1. Install ZSH in your server
2. `echo $SHELL` prints out the main shell type you use
3. modify the `.bashrc` or `.profile` or `.bash_profile`, add the code below
```shell
export SHELL=`which zsh`
[ -z "$ZSH_VERSION" ] && exec "$SHELL" -l
```
- Note that the three files above may not be equally choosed in different OS, prefer to write in `.profile` or `.bash_profile`
4. log out and login
5. install oh-my-zsh using official cmd line tool
6. modify the `.zshrc` as you like
7. Enjoy



### 25. Export 路径

- 在工作中如果需要检索某一个特定的包或者路径，可以将路径在`.bash_profile`或者`.zshrc`中添加，这取决于用的shell是bash还是zsh，格式如下：

```shell
export PATH="path/to/some/where:$PATH"
```



### 26. iterm2 Learn

- 使用 `cmd+opt+上下左右`来切换iterm中的单个tab分屏
- 使用 `cmd+d` 或者 `cmd+shift+d` 来进行左右分屏或者上下分屏
- 使用 `cmd+w` 来关闭当前的分屏
- 使用 `ctrl+w` 撤销前面输入的一个词
- 使用 `ctrl+u` 撤销一整句话
- 使用 `ctrl`



### 27. zsh必备插件

- powerlevel10k
- zsh-autosuggestions
- zsh-autohighlighting



### 28. 安装在本地用户`./local`

一般来说，我们在安装文件的时候，默认是安装在`/usr/bin`，然而这需要一个`sudo`权限，这时候我们可以利用`./config --prefix=$HOME/.local`在下载命令前添加来实现自定义下载





### 29. `awk`指令用途

- 添加一列并且用指定delimeter

  ```shell
  awk 'BEGIN{OFS="\t"}{print $1,$2,$3,'0',$5,log($11),$12}' inputfile > outputfile
  ```

  - `$INT` refers to which col, $5 refers to fifth col
  - you can insert specific symbol to form a new col anywhere you want, like `'0'` in the above example which is inserted between `$3` and `$5`



### 30. `tmux`的配置





### 31. vim解决一些包依赖安装需要root权限的方法

```shell
mkdir ~/usr/local
cd <path_to_ncurses>
tar xzvf <ncurses>.tar.gz # change the tar command if it is not a tar.gz
cd <ncurses>
./configure --prefix=$HOME/usr/local
make
make install
cd <path_to_vim>
LDFLAGS=-L$HOME/usr/local/lib ./configure # then add any options e.g. --prefix=$HOME/usr/local
make
make install
```



```shell
./configure --with-features=huge \
            --enable-multibyte \
            --enable-python3interp=yes \
            --with-python3-config-dir=/usr/bin/python3-config \ # 指定python3-config的位置，如若出现版本问题，可换为虚拟环境
            --enable-gui=gtk2 \
            --enable-cscope \
            --enable-fail-if-missing \
            --prefix=/usr/local
```

### 32. unzip and zip

- unzip xxx.zip -d some\_destinations

- zip -s- zip\_file.zip -O zip\_file\_full.zip # first concat
  unzip zip\_file\_full.zip # then unzip all\_file\_zip







## III. R Language

#### Tidyverse 小技巧

First, remember, the funtion in tidyverse mostly deals with variables (column names e.g)

**增删改查**是关键



1. 搞清楚tidyselect对象，何时使用`any_of`,` all_of` selection language: https://tidyselect.r-lib.org/reference/language.html

2. `across`, `if_all()` and `if_any()` uses selection language. 事实上，唯一能用到这三个函数的地方大部分只有`mutate()` and `summarise()` 尤其是我们要同时处理多列（多个变量一起的时候尤为重要）。更广泛地说，只要有多列选择的地方，selection 语法就能用，比如`drop_na()`，`filter()`

3. 利用`join()`函数可以实现绝大部分的aggregate的功能以及merge and shrink功能

4. `mutate()` 函数和`select()`相结合可以实现绝大多数增加列，修改列，删除列的功能

5. `filter()` 根据列查行

6. `relocate()`可以改动少量列的相对位置，很有意思的小函数

7. 

   

### 1. R saveRDS() and readRDS() function

They handle the "module" objects io.



### 2. Scale features

Scale() function center and scale the feture columns across samples. It is used to make them comparable. Even if when we do cluster over feature, we also need to scale feature ops across samples (do it by column for sample by features data)



### 3. `NbClust()`

A good Grid Search clustering method. But takes time when it comes to a large dataset.

一般用cindex，









