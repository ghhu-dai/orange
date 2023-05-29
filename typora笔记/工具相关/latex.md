

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
拉丁字母、阿拉伯数字和 +-*/= 运算符均可以直接输入获得，命令`\cdot`表示乘法的圆点，命令`\neq`表示不等号，命令`\equiv`表示恒等于，命令`\bmod`表示取模

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
   * `$$  \lim_{x \to \infty} x^2_{22} - \int_{1}^{5}x\mathrm{d}x + \sum_{n=1}^{20} n^{2} = \prod_{j=1}^{3} y_{j}  + \lim_{x \to -2} \frac{x-2}{x} $$`
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

