## Scripts used records

> 大部分都是描述了实验中遇到的一系列问题和解决方法  

#### 1. Pycharm Jupyter的配置

Pycharm配置jupyter

1. 下载jupyter notebook
2. 验证jupyter notebook是否已经下载完毕
3. 在Bash内部选取需要的虚拟环境
4. 输入`jupyter notebook --no-browser`来启动当前环境下的jupyter notebook服务端，Bash终端会显示当前环境下的Jupyter服务端口号和相应的Token
5. 在Pycharm里面新建一个ipynb文件，选择`Configure Jupyter Server`，在命令行选取该服务端口号粘贴复制进Configured Server点击Apply and OK即可

#### 2. _tkinter.TclError: no display name and no $DISPLAY environment variable解决办法

https://blog.csdn.net/asty9000/article/details/88577872 方法二

```bash
#当前shell
export MPLBACKEND=Agg
#只为当前脚本test.py
MPLBACKEND=Agg python test.py
```

 

#### 3. 应用正则表达式筛选特定行

```python
sp_phenotype[['icgc_specimen_id', 'specimen_type']][sp_phenotype['specimen_type'].str.contains(r'Metastatic', regex=True)]
```

如果遇到不是`str`类型的话，则增添一个类型转换即可

```python
[sp_phenotype['specimen_type']..astype(str).str.contains(r'Metastatic', regex=True)]
```



#### 4. 使用Counter类进行计数活动

- 可以用于相当多的可迭代对象，包括pandas.series

```python
from collections import Counter
cout = Counter(iterable)
# 几个常用的类函数
cout.elements() # 返回一个迭代器 
cout.most_common(n) # 选取前n个最多的
cout.subtract(cout2) # 计数对应元素相减，等价于cout-cout2 
```



#### 5. 使用rename来更改列名

```python
# 常用的做法是传入一个字典
df.rename(columns = {'a':'c'}, inplace = True)
```



#### 6. 更改索引

- 数字索引更改

```python
df.reindex(index=list(range(1, df.shape[0])))
```

#### 7. 筛选Dataframe的特定行

- 筛选在一个series中出现的行

  `df.loc[df['column_name'].isin(some_values)]  # some_values是可迭代对象`



#### 8. set_index和reset_index



#### 9. 什么叫做富集分析

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

  将差异表达的样本数据带入，通常这些数据都是频率样本数据。得到统计量大小，再按照统计量分布计算双尾或者单尾的概率，如果小于$1-置信度\alpha$。那么就拒绝$H_0$假设，则这些差异表达基因确实在这Go term中产生了影响。故此也可以计算$p$值。



#### 10. 设置端口转移

```shell
ssh -N -L 9898:localhost:9898 sunzehui@172.16.75.132
```

要注意到的是Pycharm的端口只适用于本地的Localhost端口，而不适用于直接的服务器端口互联。



#### 11. CRISPERi-FLOWish

- 两个生物大分子，Cas9蛋白和gRNA（guide RNA）组成了CRISPR/Cas9基因编辑系统。在细胞内，Cas9蛋白与gRNA形成复合物，能特异性鉴别出靶序列。此过程中，Cas9蛋白负责将复合物定点到靶DNA和剪切靶DNA。Cas9蛋白有6个结构域，分别是Rec I、Rec II、Bridge Helix、PAM Interacting、**HNH**和**RuvC**。Rec I是6个中最大的一个结构域，负责结合gRNA。一旦结合了靶DNA，Bridge Helix就负责启动剪切。PAM interacting结构域赋予Cas9对PAM序列的特异性要求，负责启动与靶DNA的结合。**HNH和RuvC都是核酸酶结构域，剪切单链DNA**。
- dCas9蛋白指的是通过往**RuvC和NHN**两个核酸酶结构域分别导入氨基酸突变D10A和H840A，使得Cas9蛋白失去切割DNA活性，但仍保留结合DNA的能力，称为dead Cas9
- 为了进一步提高转录抑制的效率，dCas9融合了一个基因抑制结构域，如**KRAB（krüppel-associated box）结构域**，这样的蛋白称之为dCas9-KRAB
- **RNA-FISH**：如果待检测的细胞或组织切片上的靶核酸与所用的核酸探针是同源互补的，二者经变性-退火-复性，即可形成靶核酸与核酸探针的杂交体。将核酸探针的某一种核苷酸标记上报告分子如生物素或直接标记荧光素，可利用该报告分子与荧光素标记的特异亲和素之间的免疫化学反应或直接经荧光检测体系在镜下对待测核酸(mRNA、lncRNA、circRNA、miRNA)进行定性、半定量或相对定位分析的一种实验方法。
- **FACS**：根据荧光流式分选

#### 12. Vim-Learn
ESC mold:
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
- u 撤销
- Ctrl+r 恢复撤销


INSERT mold:
- i 进入INSERT模式 


#### 13. Hi-C dataset Normalization Method
- To compensate for the bias brought by high variation in the genomic data
- Six methods: <https://doi.org/10.2144/btn-2019-0105>

### 14. df.apply()
```python
df.apply(lambda x: get_hg38_pos(x["#chr"], x["start"]), axis=1)
```
- axis = 1: 对行做apply, 沿着列进行
- lambda x: 相当于自定义函数$x = f(x)$
- note: replace is true, no replication

### 15. Hi-C data Query

Power Law?
- can be downloaded from JuiceBox database maintained by Aiden Lab. 

### 16. Useful snippet to download 

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

### 17. 本地端口监听服务器端口
```shell
ssh -N -L 9897:localhost:9898 sunzehui@x.x.x.x
```
- 本地的9897端口监听远端服务器的9898端口，方便Pycharm的jupyter server configuration, 因为pycharm不能远程连接jupyter server port

### 远程jupyter的启动位置决定了工作目录

[I 10:09:03.499 NotebookApp] Serving notebooks from local directory:

### eQTL 表达数量性状基因位点

eQTL指的是在染色体DNA条带上存在的一段可以控制基因的表达数量性状的DNA片段，分为cis-eQTL以及trans-eQTL，cis主要调控周围邻近1Mb左右的基因区域，而trans则是远程的区域，一般来说我们需要去结合SNP信息和基因表达矩阵来进行eQTL分析，找出表达位点。

### 


