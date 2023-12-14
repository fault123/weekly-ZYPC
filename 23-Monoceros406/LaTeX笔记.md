---
title: LaTeX笔记
date: 2023-10-15 20:21:26
tags: LaTeX
mathjax: true
---

# $\LaTeX$笔记

## 基本结构

```latex
\documentclass{ctexart}
\title{书名}
\author{作者}
\date{\today}
\begin{document}
    \maketitle
    \newpage
    \tableofcontents %目录
    \chapter{...}%只ctexbook类型
        \section{...}
            \subsection{...}
                ...
\end{document}
```

## 引用环境

```latex
\begin{quote}
    引用环境
\end{quote}
```

## 自定义

```latex
\newcommand{\emphxy}[1]{\textcolor{red}{\textbf{#1}}}%新定义\emphxy 1表示参数个数
\newenvironment{Abstract}{%定义Abstract环境分别为begdef和enddef
    \begin{center}
        \normalfont
        \bfseries
        摘要
    \end{center}
    \begin{quote}
}{
    \end{quote}
    \par
}
\begin{Abstract}
\end{Abstract}
```

## 文字

```latex
\usepackage{ctex}%中文
%同时排版多国语言：
\usepackage{fontspec}%不用PdfLaTeX
\setmainfont{CMU Serif}
\usepackage{xeCJK}%可换ctex
```

## 符号

```latex
%重音符号
\'a \-a \v{a} \c{a} \`a \=a \H{a} \d{a} \^a \.a \t{aa} \b{a} \"a \u{a} \r{a}
%特殊符号
\AA \OE \IJ \O \aa \oe \ij \o \AE \SS \L \i \ae \ss \l \j
\# \$ \% \& \{ \} \_ \textbackslash
\usepackage{alltt}
\begin{alltt}
    可特殊字符 # $ % ^ & ~ _
\end{alltt}

%标点符号
This is `left single quatationi mark'.
这是‘左单引号’。
%英文单引号：伪代码
\usepackage{listings}
\lstset{upquote}
\begin{lstlisting}
    aaa'aaa"aaa
\end{lstlisting}
\verb<char>text<char>:例如&：\verb|&| \verb=&=等
\verb* 空格
```

## 程序段

```latex
\usepackage{verbatim}
\begin{verbatim}
    ...
\end{verbatim}
```

## 打印数学公式的$\LaTeX$文档

```latex
\usepackage{fancyvrb}
\begin{Verbatim}
    不会自动换行 不能太长
\end{Verbatim}
```

## 字体修饰

```latex
\textbf{加粗}
\usepackage{CJKfntef}
    \CJKunderdot{着重号}
\usepackage{ulem}
    \uline{aaa}单下划线
    \uwave{aaa}波浪线
    \xout{aaa}斜删除线
    \dotuline{aaa}下加点
    \uuline{aaa}双下划线
    \sout{aaa}删除线
    \dashuline{aaa}虚线
\usepackage{soul}
    \caps{aaa}转大写
    \so{aaa}拉宽
    \ul{aaa}同uline
    \hl{aaa}背影：浅灰 加载color宏包：黄
    \st{aaa}同sout
```

## 空格

```latex
\xspace \quad \qquad
\phantom{...}%和内容一样长的空格
    \hphantom{...} \vphantom{...} 水平、垂直
```

未完待续...
