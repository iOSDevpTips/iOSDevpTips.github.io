---
layout: post
title: "Markdown的基本语法"
category: Tutorials
---

# 标题
\# 一级标题
\## 二级标题
\### 三级标题
# 有序列表
1. cell1
2. cell2
3. cell3

# 无序列表（\*，\+，\-都可以）
\- cell1
\+ cell2
\* cell3
# 超链接
\[Google\]\(http://www.google.co.uk/\)
# 图片
\!\[a picture\]\(http://shayashi.net/wp-content/uploads/2013/07/markdown-mark2.png\)
# 引用
\> this is a quote
# 换行
最后加两个以上的空格就可以换行
# 代码
\`markup\`
或者以制表符或至少4个空格缩进的行
jekyll-bootstrap模版自带Pygments插件，代码段可以这样写（删掉‘}‘后面的空格）:
\{ % highlight lang %\}
code
\{ % endhighlight %\}
# 强调
\*italic\*
\_italic\_
\*\*bold\*\*
\_\_bold\_\_
\*\*\*bold italic\*\*\*
\_\_\_bold italic\_\_\_
# 自动链接
<链接>
[Mastering Markdown](https://guides.github.com/features/mastering-markdown/)
[Markdown维基百科](http://zh.wikipedia.org/wiki/Markdown#.E6.AE.B5.E8.90.BD)
[快速语法入门](http://wowubuntu.com/markdown/basic.html)
[完整语法说明](http://wowubuntu.com/markdown/#backslash)
