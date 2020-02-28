---
title: $\LaTeX$常用代码段
categories: Latex
tags: Latex
icon: code
---

## 1 公式
**一些小技巧：**
>- 公式环境加`*`号会参与自动编号
>- 公式环境可以嵌套。内层嵌套不能使用`*`号，且要>加上`ed`。
>- 最好不要留多余空行，否则可能报错。

1. 行内公式
`$\a+b=c$`

2. 单行行间公式
```latex
\begin{equation}
X(i, j) = \begin{cases}
0, \quad X(i, j)\text{缺失}\\
1, \quad \text{其它}
\end{cases}
\end{equation}
```
3. 多行推导公式
```latex
\begin{align*}
 f(x) &= (x+a)(x+b) \\
 &= x^2 + (a+b)x + ab
\end{align*}
```

4. 多行对齐公式
```latex
	\begin{gather*}
		E(Y_t) = \mu \\
		E\left(Y_{t}-\mu\right)^{2}=E\left(\epsilon_{t}+\theta \epsilon_{t-1}\right)^{2}=\left(1+\theta^{2}\right) \sigma^{2} \\
		E\left(Y_{t}-\mu\right)\left(Y_{t-1}-\mu\right)=\theta \sigma^{2} \\	
  \end{gather*}
```
5. 多行条件公式
```latex

\begin{equation}
\begin{cases}
	123&a=5\\
	433434&a=64554\\
\end{cases}
\end{equation}
```
6. gather内嵌套align
```latex
gather*内嵌套aligned：
\begin{gather*}
c=53223
\begin{aligned}
a&=1\\
&=332
\end{aligned}
\end{gather*}
```

7. 分段函数
```latex
smooth_{L_{1}}(x)=\begin{cases}
0.5x^{2}, &\left |x \right |\leq 1 \cr \left |x \right|-0.5, & otherwise
\end{cases}

```

## 2 图
1. 一行一图
```latex
\begin{figure}  %图
\centering  %插入的图片居中表示
\includegraphics[width=\textwidth]{./figures/estimate.jpg}
\renewcommand{\figurename}{Fig.}\renewcommand{\figurename}{图}
\caption{不同数据缺失率下的数据补全结果}  %图片的名称
\label{fig1}   %标签，用作引用
\end{figure}
```
2. 一行多图
```latex
\usepackage{subcaption}

	\begin{figure}[H]
		\begin{subfigure}{0.5\textwidth}
		\includegraphics[width=\textwidth]{./figures/ar_1_acf.png}
		\label{fig:3_classes}
		\caption{}
		\end{subfigure}
		\begin{subfigure}{0.5\textwidth}
		\includegraphics[width=\textwidth]{./figures/ar_1_pacf.png}
		\label{fig:5_classes}
		\caption{}
		\end{subfigure}
		\caption{ACF和PACF}
	  \end{figure}
```

## 3 列表
```latex
% 有序列表
\begin{enumerate}[(1)]
% \begin{enumerate}[{[1]}]
% \begin{enumerate}[(i)]
\item 有编号的列表
\item ...
\end{enumerate}

% 无序列表
\begin{itemize}
\item 无编号的列表
\item ...
\end{itemize}
```

## 4 表格
1. 简单表格
```latex
	\begin{table}[H]
	\centering
	\caption{预测误差分布}  
		\begin{tabular}{|c|c|c|c|c|} % 竖线表示是否在该列加竖线
		\hline  
		 & 均值  & 均值理论值 &方差&方差理论值\\  
		\hline  
		1步  & 0.0103 & 0 & 0.9927 & 1 \\  
		2步 & 0.0141 & 0 & 1.2549 & 1.25 \\
		\hline
	\end{tabular}  
	\end{table}
```
2. 三行线表格
```latex
\begin{table}[htp]
		\centering
		\caption{MA($\infty$)和GRU的最大可预测步数比较}
		\label{Tab03}
		% shorten cline length
		\begin{tabular}{@{\extracolsep{12pt}}cccccccccc@{}}
		\hline
		\multirow{2}{*}{参数} & \multicolumn{3}{c}{AR(1)} & \multicolumn{3}{c}{AR(2)} & \multicolumn{3}{c}{MA(2)} \\
		\cline{2-4}  \cline{5-7} \cline{8-10}
		& 0.1 & 0.5 & 0.9 & -0.1 & -0.5 & -0.7 & -0.1 & -0.2 & -0.3 \\
		\hline
		MA($\infty$)& 1 & 3& 19 & 3 & 6 & 11 & 2 & 2 & 2\\
		
		GRU& 1 & 3 & 20 & 3 & 5 & 11 & 2 & 2 & 2\\
		\hline
		\end{tabular}
		\end{table}
```
3. 去掉表格标题
```latex
% Delete : of image caption
\usepackage{caption}
\DeclareCaptionLabelSeparator{twospace}{\ ~}
\captionsetup{labelsep=twospace}
```
4. 多行表格
```latex
\begin{table}[htp]
	\centering
	\caption{不同条件下一些中间结果的动态范围}
	\label{cha_nonlinear}
	\begin{tabular}{@{\extracolsep{12pt}}cccccc@{}}
		\hline
		\multirow{2}{*}{序列编号} & \multirow{2}{*}{条件}    & \multicolumn{4}{c}{动态范围}\\
		\cline{3-6}& & $\mathbf{A}_{enc}$  & $X_{attn}$ & $\mathbf{A}_{enc\_dec}$ & $\hat{\mathbf{x}}_{pre}$\\
		\hline
		\multirow{4}{*}{1} & 无线性投影，无位置编码 & 1  & 2 & 3 & 4\\
		& 有线性投影，无位置编码 & 1  & 2 & 3 & 4\\
		& 无线性投影，有位置编码 & 1  & 2 & 3 & 4 \\
		& 有线性投影，有位置编码 & 1  & 2 & 3 & 4\\
		\hline
	\end{tabular}
\end{table}
```


## 5 调整页边距
```latex
\usepackage{geometry}
\geometry{a4paper,scale=0.8}
\geometry{a4paper,left=2cm,right=2cm,top=1cm,bottom=1cm}
```

## 6 参考文献
1. 一般参考文献
```latex
\usepackage{cite}
\usepackage{natbib}
\usepackage{hyperref}

% Place to the location of reference
\bibliographystyle{plain}
\bibliography{reference}

% Refer in the text
\cite{Wille1982}
% \citep{Wille1982}
```

2. URL参考文献（无超链接）
```latex
\usepackage{url}
@Misc{timmurphy.org,
howpublished = {\url{http://timmurphy.org/2009/07/22/line-spacing-in-latex-documents/}},
note = {Accessed April 4, 2010},
title = {Line Spacing in LaTeX documents},
author = {Murphy, Timothy I}
}
```
3. 超链接参考文献（可点击）
```latex
\usepackage{hyperref}
% 设置各种颜色
\hypersetup{
    colorlinks=true, % 使用颜色代替框
    linkcolor=black, % 目录之类的颜色
    filecolor=blue,  
    urlcolor=blue, % 引用url颜色
    citecolor=blue, % 引用的颜色
}
@unpublished{Survey2014,
  title={Survey on the access to
finance of enterprises},
  author={Sophie Doove and Petra Gibcus and
Ton Kwaak and Lia Smit and Tommy Span},
  year=2014,
  note ={Accessed 16 June 2017.  \href{http://wwwe.ansa.it/documents/1415814222451\_Rapporto.pdf/}{http://wwwe.ansa.it/documents/1415814222451/_Rapporto.pdf/}},
}
```

## 7 页码
```latex 
\thispagestyle{empty} % 在不需要设置页码的下面加
\setcounter{page}{1} % 强制设置页码值
```

## 8 python代码
```latex
\usepackage{listings}
\usepackage{color}

\definecolor{dkgreen}{rgb}{0,0.6,0}
\definecolor{gray}{rgb}{0.5,0.5,0.5}
\definecolor{mauve}{rgb}{0.58,0,0.82}

\lstset{frame=tb,
	language=Python,
	aboveskip=3mm,
	belowskip=3mm,
	showstringspaces=false,
	columns=flexible,
	basicstyle={\small\ttfamily},
	numbers=none,
	numberstyle=\tiny\color{gray},
	keywordstyle=\color{blue},
	commentstyle=\color{dkgreen},
	stringstyle=\color{mauve},
	breaklines=true,
	breakatwhitespace=true,
	tabsize=3
}

\begin{lstlisting}
print ('Starting Iterations')
for iter in range(n_iterations):
for i in range(n):
Users[i] = np.dot(np.dot(np.dot(np.linalg.inv(np.dot(np.dot(Items,np.diag(R[i])),Items.T) +
lambda_ * np.eye(n_factors)),Items),np.diag(R[i])),A[i]).T

for j in range(m):
Items[:,j] = np.dot(np.dot(np.dot(np.linalg.inv(np.dot(np.dot(Users.T,np.diag(R[:,j])),
Users) +
lambda_ *np.eye(n_factors)),Users.T),np.diag(R[:,j])),A[:,j])
#print ('Error after solving for Item Matrix:%f' % get_error(A, Users, Items, R))
NMAE_List.append(get_error(A, Users, Items, R))
print ('%sth iteration is complete...' % iter)
print('Iteration finish')
\end{lstlisting}
```)
\end{lstlisting}
```