---
title: latex导言区常用设置
categories: Latex
tags: Latex
icon: code
---

```latex
% Article setting
% Article type
\documentclass[12]{article}

% Page margin
\usepackage{geometry}
%\geometry{a4paper,scale=0.8}
\geometry{a4paper,left=2cm,right=2cm%,top=2cm,%bottom=2cm, includefoot,heightrounded
}

% set title
\title{}
\author{}
\date{}

% import package
\usepackage{amsmath, amsfonts, bm}
% \numberwithin{equation}{section} % Equation numbering by section
\numberwithin{equation}{subsection} % Equation numbering by subsection
% \numberwithin{equation}{subsubsection}


% Chinese support
\usepackage{xeCJK}
\usepackage{fontspec} 
\setCJKmainfont[AutoFakeBold]{SimSun}
\setCJKmonofont{SimSun}
\setmainfont{Times New Roman}

\renewcommand\contentsname{目录}
\renewcommand\refname{参考文献}
\renewcommand{\figurename}{图}
\renewcommand{\tablename}{表}

% Indent
\usepackage{indentfirst}
\setlength{\parindent}{2em}

% Python code
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

% Import image
\usepackage{graphicx}
\usepackage{subcaption}

% Delete : of image caption
\usepackage{caption}
\DeclareCaptionLabelSeparator{twospace}{\ ~}
\captionsetup{labelsep=twospace}

%Adjust line space
\usepackage{setspace}
\renewcommand{\baselinestretch}{1.4}
% \setlength{\baselineskip}{20pt}

% Adjust table height
\usepackage{array}
\renewcommand\arraystretch{1.5}

% Insert algorithm
\usepackage{algorithm, algorithmic} 

% Reference
\usepackage{cite}
\usepackage{natbib}
\usepackage{hyperref}
\hypersetup{
    colorlinks=true,
    linkcolor=black,
    filecolor=blue,      
    urlcolor=blue,
    citecolor=blue,
}


% Delete : of image caption
\usepackage{caption}
\DeclareCaptionLabelSeparator{twospace}{\ ~}
\captionsetup{labelsep=twospace}

%Adjust line space
\usepackage{setspace}
\renewcommand{\baselinestretch}{1.4}
% \setlength{\baselineskip}{20pt}

% Adjust table height
\usepackage{array}
\renewcommand\arraystretch{1.5}

% Insert algorithm
\usepackage{algorithm, algorithmic} 

% Reference
\usepackage{cite}
\usepackage{natbib}
\usepackage{hyperref}
\hypersetup{
    colorlinks=true,
    linkcolor=black,
    filecolor=blue,
    urlcolor=blue,
    citecolor=blue,
}
```    citecolor=blue,
}
```