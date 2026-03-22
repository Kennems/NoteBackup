---
title : 'LaTeX 学习笔记'
date : 2025-03-16T10:30:13+08:00
lastmod: 2025-03-16T10:40:13+08:00
description : "LaTeX 学习笔记" 
categories : ["LaTeX"]
tags : ["LaTeX"]
---

---

# LaTeX 学习笔记

## 一、文档基本设置

### 1.1 文档类型、标题、作者、日期等

```latex
\documentclass[12pt]{article} % 指定文档类型和字号

\title{All Attention you need!} % 设置标题
\author{Kenneth}             % 设置作者
\date{}                     % 设置日期（为空时不显示日期）

\begin{document}
\maketitle                  % 生成标题

\begin{abstract}            % 摘要环境
    abstract content
\end{abstract}
```

### 1.2 分节与段落

```latex
\section{Introduction}      % 一级标题
\subsection{Background}     % 二级标题
\subsubsection{Details}     % 三级标题
```

---

## 二、数学公式

LaTeX 以其强大的数学排版功能著称，以下分别介绍常见的数学符号及其用法。

### 2.1 行内公式与独立公式

- **行内公式**：使用`$...$`包含，例如：`$E=mc^2$`  
- **独立公式**：使用`$$...$$`或`\[...\]`，例如：
  
  ```latex
  $$E=mc^2$$
  ```
  
  $$E=mc^2$$
  
  或
  
  ```latex
  \[
  E=mc^2
  \]
  ```

### 2.2 上标与下标

- **上标**：使用 `^` 实现，例如：

  ```latex
  $x^{2}$   % 显示为 x²
  $e^{i\pi}+1=0$   % 欧拉公式
  ```

  - $x^{2}$ 
    $e^{i\pi}+1=0$

- **下标**：使用 `_` 实现，例如：

  ```latex
  $a_{i}$   % 显示为 a₍ᵢ₎
  $a_{ij}$  % 显示为 a₍ᵢⱼ₎
  ```

  - $a_{i}$ 
    $a_{ij}$  

- **组合使用**：

  ```latex
  $a_{ij}^{2}$   % a 的 ij 元素的平方
  ```

  - $a_{ij}^{2}$   

### 2.3 分数与根号

- **分数**：使用 `\frac{分子}{分母}` 例如：
  
  ```latex
  $$\frac{1}{2}$$
  $$\frac{\frac{1}{x}}{\frac{1}{y}}$$
  ```

- **根号**：
  
  - 一般平方根：
  
    ```latex
    $$\sqrt{x}$$
    ```
  
    - $$\sqrt{x}$$
  
  - 指定次数的根号：
  
    ```latex
    $$\sqrt[n]{x}$$
    ```
  
    - $$\sqrt[n]{x}$$
  
- **嵌套根号**：

  ```latex
  $$\sqrt{\sqrt{ x^2 + y^2 }}$$
  ```

  - $$\sqrt{\sqrt{ x^2 + y^2 }}$$

### 2.4 括号自动调整大小

使用 `\left` 与 `\right` 命令，使括号大小自动匹配内部内容：

```latex
$$\left(\frac{2}{4}\right)$$
$$\left[ \frac{a}{b} \right]$$
$$\left\{ x+2 \right\}$$
```

$$\left(\frac{2}{4}\right)$$
$$\left[ \frac{a}{b} \right]$$
$$\left\{ x+2 \right\}$$

### 2.5 积分、求和与极限

- **积分**：使用 `\int` 表示积分符号。上下限分别用下标和上标表示：
  
  ```latex
  $$\int_{a}^{b} f(x) \,dx$$
  $$\iint_{D} g(x,y) \,dxdy$$  % 二重积分
  ```

  - $$\int_{a}^{b} f(x) \,dx$$
    $$\iint_{D} g(x,y) \,dxdy$$  % 二重积分
  
- **求和**：使用 `\sum` 表示求和符号：

  ```latex
  $$\sum_{i=1}^{n} i$$
  $$\sum_{\substack{0\le i\le n \\ i\ \text{even}}} i$$  % 多行条件
  ```

  - $$\sum_{i=1}^{n} i$$
    $$\sum_{\substack{0\le i\le n \\ i\ \text{even}}} i$$  % 多行条件

- **极限**：使用 `\lim` 表示极限：

  ```latex
  $$\lim_{x \to 0} \frac{\sin x}{x}$$
  ```

  - $$\lim_{x \to 0} \frac{\sin x}{x}$$

- **无穷大**：

  ```latex
  $$\infty$$
  ```

  - $$\infty$$

### 2.6 常见运算符

- **乘号、点乘等**：
  
  ```latex
  $a \cdot b$    % 点乘
  $a \times b$   % 叉乘
  ```

  - $a \cdot b$    % 点乘
    $a \times b$   % 叉乘
  
- **其他符号**（如集合、逻辑符号等）：

  ```latex
  $\cup, \cap, \subset, \in, \notin$
  ```

  $\cup, \cap, \subset, \in, \notin$

---

## 三、希腊字母与其他符号

LaTeX 中提供了大量希腊字母与特殊符号。

### 3.1 希腊字母

```latex
$\alpha, \beta, \gamma, \lambda, \Delta, \Omega$
```

$\alpha, \beta, \gamma, \lambda, \Delta, \Omega$

### 3.2 特殊符号

例如：

```latex
$\forall, \exists, \nabla, \partial$
```

$\forall, \exists, \nabla, \partial$

---

## 四、列表、图像与表格

### 4.1 列表

- **无序列表**：
  
  ```latex
  \begin{itemize}
      \item Book
      \begin{itemize}
          \item Math
      \end{itemize}
      \item Cup
  \end{itemize}
  ```

- **有序列表**：
  
  ```latex
  \begin{enumerate}
      \item Book
      \item Cat
  \end{enumerate}
  ```

### 4.2 图像

```latex
\section{picture}
I have many beautiful pictures\ref{fig:1}.\\
\begin{figure}[htp]
    \centering
    \includegraphics[scale=0.1]{dog.png}
    \caption{Dog}
    \label{fig:1}
\end{figure}
```

### 4.3 表格

```latex
\section{table}
This is a test table \ref{tab:my_label}
\begin{table}[htp]
    \centering
    \begin{tabular}{|c|c|c|c|}
        \hline
        Country & Population & GDP & Index \\
        \hline
        None & 323 & 13 & 123  \\
        \hline
    \end{tabular}
    \caption{economics Index}
    \label{tab:my_label}
\end{table}
```

---

## 五、矩阵与行列式

LaTeX 支持多种矩阵环境，常见的有 `pmatrix`、`bmatrix`、`vmatrix` 等。

### 5.1 矩阵

- **小括号矩阵**：
  
  ```latex
  $$\begin{pmatrix}
  a & b \\
  c & d
  \end{pmatrix}$$
  ```

  - $$\begin{pmatrix}
    a & b \\
    c & d
    \end{pmatrix}$$
  
- **中括号矩阵**：

  ```latex
  $$\begin{bmatrix}
  1 & 2 & 3 \\
  4 & 5 & 6 \\
  7 & 8 & 9
  \end{bmatrix}$$
  ```

  - $$\begin{bmatrix}
    1 & 2 & 3 \\
    4 & 5 & 6 \\
    7 & 8 & 9
    \end{bmatrix}$$

- **竖线矩阵（行列式）**：

  ```latex
  $$\begin{vmatrix}
  a & b \\
  c & d
  \end{vmatrix}$$
  ```
  $$\begin{vmatrix}
  a & b \\
  c & d
  \end{vmatrix}$$
---

## 六、参考文献

使用 `\cite` 命令引用文献，参考文献列表用 `thebibliography` 环境。

```latex
\section{reference}
reference :\cite{cite1}

\begin{thebibliography}{1}
    \bibitem{cite1}
    Y. Zhang, J. Qin, D. S. Park, W. Han, C.-C. Chiu, R. Pang, Q. V. Le, and Y. Wu. Pushing
    the limits of semi-supervised learning for automatic speech recognition. Proc. of NeurIPS
    SAS Workshop, 2020b.
\end{thebibliography}\section{reference}
reference :\cite{cite1}

\begin{thebibliography}{1}
    \bibitem{cite1}
    Y. Zhang, J. Qin, D. S. Park, W. Han, C.-C. Chiu, R. Pang, Q. V. Le, and Y. Wu. Pushing
    the limits of semi-supervised learning for automatic speech recognition. Proc. of NeurIPS
    SAS Workshop, 2020b.
\end{thebibliography}
```

---


