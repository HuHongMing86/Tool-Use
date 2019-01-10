## LaTeX使用

### 1 设置页边距

首先使用`geometry`宏包，然后使用`\geometry{}`命令来设置

```latex
\usepackage{geometry}
\geometry{top=... , bottom=..., left=... , right=....}
```

### 2 设置文字居中

使用`\center`环境

```latex
\begin{center}
......
\end{center}
```

### 3 设置中文字体字号

直接使用`\heti  \zihao{-3}`命令即可，具体对应见LaTeX入门那本书。

```latex
\heiti \zihao{-3} 哈哈哈    % 黑体小三号字
```

### 4 设置缩进距离、行距以及段间距

首先使用`indentfirst` 以及`setspace`包，然后使用下面的命令：

```latex
\usepackage{indentfirst}
\usepackage{setspace}
\setlength{\parindent}{10pt/2em/3cm}   # 设置首行缩进距离
\setlength{\parskip}{0.3\baselineskip} # 设置段间距为行间距的多少.设置的是下一段和上一段最后一行之间的间距,

\setlength{\baselineskip}{20pt} # 设置行间距.
\renewcommand{\baselinestretch}{1.0} # 设置为单倍间距二倍间距等.

```

**设置行间距以及段间距要在定义字号字后,不然无法生效. 同时需要先设置行间距再设置段间距**

### 5 设置分栏

首先需要使用`multiclo`宏包,然后在想要使用分栏的地方使用下面的环境

````latex
\usepackage{multicol}

\begin{multicols}{2}
	......
\end{multicols}
````

**注意:包的名字是multicol,但是环境的名字是multicols** 

### 6 添加脚注以及超链接

插入脚注直接使用`\footnote{脚注内容}` ，超链接使用`href{URL}{Text}` 。

其中使用超链接需要使用`\usepackage[colorlinks,linkcolor=blue]{hyperref}`宏包。

上述只能够超链接到URL，如果向超链接到文章的其他部分，见下表：

```latex
\href{URL}{text}
\url{URL}
\nolinkurl{URL}
\hyperbaseurl{URL}
\hyperimage{imageURL}{text}
\hyperdef{category}{name}{text}
\hyperref{URL}{category}{name}{text}
\hyperref[label]{text}
\hyperlink{name}{text}
\hypertarget{name}{text}
\phantomsection
\cleardoublepage
\phantomsection
\addcontentsline{toc}{chapter}{\indexname}
\printindex
\autoref{label}
```



### 7 填充整列

在使用模板的时候，可能上一个标题没有排版完，下一个标题跳到下页了，可以使用`\vfill`命令来将这一页填充到底。

### 8 插入图片与表格

需要先使用一下宏包

```latex
\usepackage{graphicx} % 插入图片
\usepackage{float}  % 使用浮动体环境
```

然后如果插入图片：

```latex
\begin{figure}[htbp]  % 表示摆放的位置.[H]表示强制放在这里。
\centering   %居中
\includegraphicx[图片大小]{图片路径}
\caption{图片描述}
\label{标记}       % 可以在文章的其他部分添加应用\ref{标记}
\end{figure}
```

插入表格：

```latex
\begin{table}
\centering
\begin{tabular}{|c|c|c|}
\hline
1 & 2 & 3 \\
\hline
\end{tabular}
\caption{表格描述}
\label{标记}
\end{table}
```

**说明：**

- 如果需要在双栏中插入单栏的图片或者表格，需要使用在后面使用[H].
- 需要在双栏中使用跨栏图片或者表格，需要使用`figure*`或者`table*`
- 如果需要控制表格宽度，可以在`|c|c|c|`中将c修改为`|p{2cm}|p{3cm}|`需要居中的话可以在表项中使用`\centering`.
- 需要并排插入多张图片可以直接百度做法[文献](https://tug.org/TUGboat/tb34-1/tb106thurnherr.pdf)。

### 9 插入参考文献

- 首先需要将参考文献的信息存在一个文件中。建立一个后缀为`.bib`的纯文本文件，比如`acl.bib`
- 然后将文章的信息放在文件中。如下面所示：

```
@article{lafferty2001conditional, title={Conditional random fields: Probabilistic models for segmenting and labeling sequence data}, author={Lafferty, John and McCallum, Andrew and Pereira, Fernando CN}, year={2001} }
```

可以直接将想要的论文放在google scholar中搜索，然后点`cite`,选择`BibTex`即可。 如果有多个参考文献，另起一行即可。

- 在`.tex`文件中使用。

直接在tex文件中插入下面的命令：

```latex
\bibliographystyle{plain}  % 设置参考文献的格式
\bibliography{acl}       % 插入参考文献
```

其中第一行指定参考文献的呈现方式，常见的预设样式有plain, unsrt, alpha, abbrv。

第二行指定之前生成的.bib文件。不要写成`acl.bib`

- 在文章中引用某个文献的时候，直接使用`\cite{lafferty2001conditional}` 其中大括号中的内容是.bib文件中相应文章的第一行。

但是这种默认的方式只会出现文中引用了的文献，所以希望将所有的文献显示出来，按照下面的操作;


首先在`\biblio....`前面加入`\nocite{*}`,然后点击工具 -> 清除辅助文件，然后重新构建即可。


[参考文献1](https://yq.aliyun.com/ziliao/283831)

[参考文献2](https://yq.aliyun.com/ziliao/524500)

### 10 插入附录

一共需要的有下面几个命令

```latex
\usepackage{appendix}
\appendix
\appendixpage
\addappheadtotoc

\section{第一部分}

内容


\section{第二部分}

内容
```

其中`\appendix`说明后面的内容为附录。`\appendixpage`将专门添加一个附录页

`\addappheadtotoc`将附录添加到目录中。

如果有不同的附录的话，可以使用`\section`命令。

参考文献: [1](https://blog.csdn.net/golden1314521/article/details/39992745)  [2](https://www.cnblogs.com/tsingke/p/6503462.html) 





  
