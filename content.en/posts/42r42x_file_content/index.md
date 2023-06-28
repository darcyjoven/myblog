---
title: "42r/42x文件功能"
weight: 1
# bookFlatSection: false
bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

> 42x/42r 是半文字的文件，功能在于标识function 在哪个42m文件中。至于42m是是通过全局变量FGLLDPATH进行逐个寻找。

程序运行后，从MAIN到各个function是透过42r/42x搜索在42m中位置。

> r.l/r.l2 后有function 未找到，除了确认编译问题，还可以看看function对应路径是否存在。p_link/azzi070中42m设定是否是有效的。
