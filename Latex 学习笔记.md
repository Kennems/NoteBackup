# Latex 学习笔记

## 文档设置

文档类型设置

```latex
\documentclass[12pt]{article}
```

文档开始

```latex
\begin{document}
\end{document}
```

标题（title）

```latex
\title{All Attention you need!}
```

日期（date）

```latex
\date{}
```

作者（author）

```latex
\author{Kenneth}
```

生成标题

```latex
\maketitle
```

摘要（abstract）

```latex
\begin{abstract}
	abstract content
\end{abstract}
```

各个部分（section)

```latex
\section{Introduction}
```

## 括号、数学公式以及希腊字母

```latex
\section{math}

$$x^{23432}$$
$$y=x^{2e+3}$$
$$ \sqrt[9]{x^2+y^9} $$
$$ \sqrt{\sqrt{ x^2 + y^2 }}$$
$$ \frac{1}{x*x}$$
$$ \frac{\sqrt{x-1}}{\sqrt{x+1}}$$

\section{greek letters}
$\alpha, \beta \gamma \lambda$

\section{brackets}
$$(x+1)$$
$$x[x+x(x-1)]$$
$$\{x,y,z\}$$
$$x\left(\frac{2}{4}\right)$$
$$x\left[\frac{2}{4}\right]$$
$$x|\left[\frac{2}{4}\right]|$$
$$\left\{x+2\right.$$
```

## 列表

```latex
\begin{itemize}
    \item book
    \begin{itemize}
        \item {Math}
    \end{itemize}
    
    \item cup
    
\end{itemize}

\begin{enumerate}
    \item book
    \item cat
\end{enumerate}
\begin{enumerate}
    \item book 
    \item cat
\end{enumerate}
```

## 图像

```latex
\section{picture}
I have many beautiful pictures\ref{fig:1}.\\
\begin{figure}[htp]
    \centering
    \includegraphics[scale = 0.1]{dog.png}
    \caption{Dog}
    \label{fig:1}
\end{figure}
```

## 表格

```latex
\section{table}
This is a test table \ref{tab:my_label}
\begin{table}[htp]
    \centering
    \begin{tabular}{|c|c|c|c|}
        \hline
        Country&Population&GDP&Index\\
        \hline
        None&323&13&123  \\
         & 
    \end{tabular}
    \caption{economics Index}
    \label{tab:my_label}
\end{table}
```

## 参考文献

```latex
\section{reference}
reference :\cite{cite1}

\begin{thebibliography}{1}
    \bibitem{cite1}
    Y. Zhang, J. Qin, D. S. Park, W. Han, C.-C. Chiu, R. Pang, Q. V. Le, and Y. Wu. Pushing
    the limits of semi-supervised learning for automatic speech recognition. Proc. of NeurIPS
    SAS Workshop, 2020b.
\end{thebibliography}
```

