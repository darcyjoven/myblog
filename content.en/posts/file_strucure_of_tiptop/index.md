---
title: "目录/文件结构"
date: 2022-04-14T09:44:13+08:00
lastmod: 2022-04-14T09:44:13+08:00
draft: false
tags: ["ERP", "TIPTOP"]
categories: ["TIPTOP", "ERP"]
# bookComments: false
# bookSearchExclude: false
---

## TOPGP

```
$TOP
|-- aap
    |-- 42f
    |-- 42m
    |-- 42mm
    |-- 4fd
    |-- 4gl
    |-- per
    `-- sdd
|-- ...
|-- czz
|-- config
|-- demo
|-- ds4gl2
|-- lib
|-- log
|-- p_cron
|-- qry
|-- schema
|-- script
|-- setup
|-- sub
|-- tmp
|-- tool
|-- trigger
`-- work
```

## T100

![](/post/mk_img/2022-04-14-09-46-11.png)

| 文件后缀名 | 来源                                            | 功能说明                   |
| ---------- | ----------------------------------------------- | -------------------------- |
| 4gl        | 用户新增                                        | 实际逻辑代码，运行时不使用 |
| 42m        | 4gl 编译后产生的文件                            | 实际运行文件               |
| 42r        | 程序连接后产生的文件                            | 保存函数调用路径文件       |
| 4fd        | studio 产生的画面挡                             | 可读的画面挡               |
| per        | fgl 自己的画面挡，通过 gsform 可与 per 互相转化 | 兼容性强的画面挡           |
| 42f        | 4fd/per 编译后产生的文件                        | 实际调用画面               |
| sch        | 数据库汇出的资料结构                            | 方便程序调用文件           |
