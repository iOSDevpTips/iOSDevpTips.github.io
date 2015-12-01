---
layout: post
title: "在Mac上学习LaTeX"
category:  LaTeX
---

1. 安装MacTeX  
https://tug.org/mactex/
下载MacTeX.pkg然后安装
2. 配置TeXShop  
Preferences->Source->Encoding
改成Unicode(UTF-8)
3. 开始

```LaTeX
%-*- coding: UTF-8 -*-
% MathPaper.tex
%!TEX TS-program = xelatex
%!TEX encoding = UTF-8 Unicode
\documentclass[a4paper, 12pt]{article}
%设置中文字体
\usepackage{fontspec}
\setromanfont{STSongti-SC-Regular}
\XeTeXlinebreaklocale "zh"  
\XeTeXlinebreakskip = 0pt plus 1pt minus 0.1pt %文章内中文自动换行

%版面设置x
\usepackage{geometry}
\geometry{left=2cm, right=2cm, top=1cm, bottom=2cm}

%首行缩进
\usepackage{indentfirst}
\setlength{\parindent}{2.45em}

%全局行间距
\linespread{1.6}

\title{Title}
\author{Author \\Phone: 0123456789 \\Email: xxx@xxx.com}
\date{}

\begin{document}

\maketitle

\section{标题}
正文
\end{document}
```
