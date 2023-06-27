

# `latex`基础:
## 数学模式 ：$
1. 行内公式： `$ f(x) = a+b $`(markdown里面默认没有单行公式，)
$$
     f(x) = a + b
$$


     公式： ` $$ f(x) = a+b $$ `

$$ f(x) = a+b $$

​                                                            

3. 手动编号：`$$ f(x) = a - b \tag{1,1} $$`

$$ f(x) = a - b \tag{1,1} $$

---
## 数学结构：
### 简单运算：
拉丁字母、阿拉伯数字和 +-*/= 运算符均可以直接输入获得，
命令`\cdot`表示乘法的圆点，
命令`\neq`表示不等号，
命令`\equiv`表示恒等于，
命令`\bmod`表示取模,
根号`\sqrt`,分数`\frac`.
`\times`叉乘

1.` $$ x+2-3*4/6 = 4/y+x\cdot y $$`

$$ x+2-3*4/6 = 4/y+x\cdot y $$

2. `$$ 0 \neq 1 \quad x \equiv x \quad 1 = 9 \bmod 2 $$`
$$ 0 \neq 1 \quad x \equiv x \quad 1 = 9 \bmod 2 $$

### 上下标
1.`_`表示下标，`^`表示上标，`'`表示求导，上下标不只一个内容时，用`{}`括起来

2. 命令：`\sqrt`表示平方根，`\sqrt[n]`表示n次方根，`\frac`表示分式

3. 命令：`\overline`, `\underline` 分别在表达式上、下方画出水平线
   * `$$\overline{x+y} \qquad \underline{a+b}$$`
   $$\overline{x+y} \qquad \underline{a+b}$$

### 向量：
 命令：`\vec`表示向量，`\overrightarrow`表示箭头向右的向量，`\overleftarrow`表示箭头向左的向量
   * `$$\vec{a} + \overrightarrow{AB} + \overleftarrow{DE}$$`
      $$\vec{a} + \overrightarrow{AB} + \overleftarrow{DE}$$

### 积分、极限、求和、乘积
命令：`\int`表示积分，`\lim`表示极限， `\sum`表示求和，`\prod`表示乘积，`^`、`_`表示上、下限
```latex
\lim_{x \to \infty} x^2_{22} - \int_{1}^{5}x\mathrm{d}x + \sum_{n=1}^{20} n^{2} = \prod_{j=1}^{3} y_{j}  + \lim_{x \to -2} \frac{x-2}{x} 
```

$$  \lim_{x \to \infty} x^2_{22} - \int_{1}^{5}x\mathrm{d}x + \sum_{n=1}^{20} n^{2} = \prod_{j=1}^{3} y_{j}  + \lim_{x \to -2} \frac{x-2}{x} $$

### 三圆点
命令：`\ldots`点位于基线上，`\cdots`点设置为居中，`\vdots`使其垂直，`\ddots`对角线排列
* `$$ x_{1},x_{2},\ldots,x_{5}  \quad x_{1} + x_{2} + \cdots + x_{n} $$`
$$ x_{1},x_{2},\ldots,x_{5}  \quad x_{1} + x_{2} + \cdots + x_{n} $$


### 重音符号
1. `$ \hat{x} $`

$$
\hat{x}
$$



2. `$ \bar{x} $`

$$
\bar{x}
$$



3. `$ \tilde{x} $`

$$
\tilde{x}
$$









### 矩阵

其采用矩阵环境实现矩阵排列，常用的矩阵环境有matrix、bmatrix[]、vmatrix||、pmatrix()，其区别为在于外面的括号不同
```
$$ 
\begin{bmatrix}
    1 & 2 & \cdots \\
     67 & 95 & \cdots \\
    \vdots  & \vdots & \ddots \\
\end{bmatrix} 
$$
```

$$
\begin{bmatrix}
1&2& \cdots \\
67&95& \cdots \\
\vdots & \vdots & \ddots \\
\end{bmatrix}
$$





### 希腊字母：

![图片描述](https://pic1.zhimg.com/v2-da3e717cf670582fbfbdddee33073524_b.jpg)

### 多选公式
* 公式组合：通过cases环境实现公式的组合，&分隔公式和条件，还可以通过\limits来让x→0位于lim的正下方而非默认在lim符号的右下方显示

```
$$D(x) = \begin{cases}
\lim\limits_{x \to 0} \frac{a^x}{b+c}, & x<3 \\
\pi, & x=3 \\
\int_a^{3b}x_{ij}+e^2 \mathrm{d}x,& x>3 \\
\end{cases}$$
```

* 拆分单个公式：通过split环境实现公式拆分

```
$$\begin{split}
\cos 2x &= \cos^2x - \sin^2x \\
&=2\cos^2x-1
\end{split}$$
```


```python

```

---



# `markdown`中调用`latex`

键入`$$`回车

### 公式左对齐

```latex
\begin{align}

\end{align}

% 如果 是align表示不带编号，不过在typora中好像加不加*都不带编号
```

#  三线表

```latex
\documentclass{article}
\usepackage{booktabs}
\usepackage[UTF8]{ctex}
\usepackage{multirow}
\usepackage{float}%提供float浮动环境
\usepackage{lmodern} % 使用lmodern字体

\begin{document}

%经典三线表
\begin{table}[H]
\caption{\textbf{Example 5}}
\centering
\begin{tabular}{lllllll}% 四个c代表有四列且内容居中，还有l r
\toprule%第一道横线
&\multicolumn{3}{c}{\textbf{RMSE(cycles)}}&\multicolumn{3}{c}{\textbf{平均百分比误差(\%) }}\\
\cmidrule(lr){2-4} % (lr)表示 左右端都对齐
\cmidrule(l){5-7} % 左端对齐，主要是配合使两条下划线之间留空白，不要连在一起


& \textbf{训练集}&\textbf{主要测试集}&\textbf{次要测试集} & \textbf{训练集} & \textbf{主要测试集} & \textbf{将要测试集} \\
\midrule%第二道横线  表格中间的线
% \multirow{2}{*}{Data1}&Data2&Data3&Data4 \\%Data1跨两行，自动表格宽度
'Variance' model & 103 & 138(138) & 196 & 14.1 & 14.7(13.2) &11.4  \\
'Discharge' model & 76 & 91(86) & 173 & 9.8 & 13.0(10.1) & 8.6 \\
'Full' model & 51 & 118(100) & 214 & 5.6 & 14.1(7.5) & 10.7 \\
% \midrule%第三道横线 
% Data5&Data6&Data7&Data8 \\
\bottomrule% 表格最后的线
\end{tabular}
\begin{flushleft} % 左对齐
  注释内容
\end{flushleft}
\end{table}


\end{document}

```

