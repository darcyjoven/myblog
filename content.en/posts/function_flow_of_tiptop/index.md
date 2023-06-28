---
title: "程序函数流程"
date: 2022-04-14T10:02:00+08:00
lastmod: 2022-04-14T10:02:00+08:00
draft: false
tags: ["程序函数流程","FLOW"]
categories: ["ERP", "TIPTOP"]
# bookComments: false
# bookSearchExclude: false
---

## Module 架构

> 一个完整的作业，由程序（4gl）和画面档（per/4fd）组成

> 程序内部可以分为两个部分，程序前部分的设定、函数。前部分的设置位汇入程序库、参照资料库、全域或模组变量。

## IMPORT 汇入程序库

> FGL 允许引入自定编写的 FGL、Java、以及 C 程序库。语法：

- 汇入 4GL ：IMPORT FGL 42m_modulde 42m 需要变迁存在于 FGLLDPATH 环境变量下。
- 汇入 Java： IMPORT JAVA class_name 程序库需要存在于 CLASSPATH 目录下。
- 需要匯入 C-extension 或系统标准函数库：IMPORT package 名称 程序库需要再 PATH/LD_LIBRARY_PATH 环境变量下。

示例:

```plsql
IMPORT FGL myutils
IMPORT FGL account #可以引用多个，注意不能造成死循环
DEFINE proginfo t_prog_info #TYPE 定义來自 myutils
MAIN
 LET proginfo.name = "program"
 LET proginfo.version = "0.99"
 LET proginfo.author = "scott"
 CALL myutils.init() #加上 42m 名称作为前置
 CALL set_account("CFX4559") #不加前置，如果函数重名，会报错。
END MAIN

```

## SCHEMA/DATABASE

> database_id 在 $FGLPROFILE 或$FGLDIR/etc/fglprofile 中设定

```plsql
SCHEMA ds
DATABASE ds1
MAIN
 DEFINE lc_zz01 LIKE zz_file.zz01
 SELECT zz01 INTO lc_zz01 FROM zz_file WHERE zz01='tiptop'
END MAIN
```

## MIAN 函数

```plsql
MAIN
 DISPLAY "Hello, world!"
END MAIN
```

## DEFER

```plsql
DEFER { INTERRUPT | QUIT }
```

- INTERRUPT 中端键（ Control-C / Delete ）后触发 ,ON ACTION interrupt 会同时触发
- QUIT 离开键， QUIT_FLAG 变为 TRUE。

## OPTIONS

![](/post/mk_img/2022-04-14-10-14-27.png)

## Exceptions

```plsql
WHENEVER [ANY] ERROR { CONTINUE | STOP | CALL function | GOTO label }
```

```plsql
MAIN
 OPTIONS #改变一些系统设置值
 		INPUT NO WRAP, #输入的方式：不循环
		FIELD ORDER FORM #整个画面会按照p_per所设定的栏位顺序
 DEFER INTERRUPT
 WHENEVER ERROR STOP #当发生 SQL Error 时停止运行
 DISPLAY “Change Exception!”
 WHENEVER ERROR CALL chk_err #此处CALL 没有括号
END MAIN
FUNCTION chk_err( )
 DISPLAY “Error Happened!”
END FUNCTION

```

## FUNCTION

> 执行某个特定功能的子函数，程序中可以将某些功能独立编写位一个个函数，可以提供多个地方调用。一个程序中 FUNCTION 名称不能重复。

```plsql
[PUBLIC | PRIVATE ] FUNCTION function_name( [arg [ , … ] ] )
```

> PRIVATE 设置定，只能本程序调用，连结在 42x 和 42r 文件，也无法被调用。PUBLIC 可以省略。

```plsql
MAIN
 CALL say_hello_public()
 CALL say_hello_private()
END MAIN
FUNCTION say_hello_public()
 DISPLAY "Hello, world!"
END FUNCTION
PRIVATE FUNCTION say_hello_private()
 DISPLAY “Hello, Private!”
END FUNCTION

```

## REPORT 函数

> 专门用来资料整理的函数格式，常用来报表打印。

```plsql
REPORT test_rep(sr)
 ...
 FORMAT
 PAGE HEADER
 BEFORE GROUP
 ON EVERY ROW
 AFTER GROUP
 ．．．
END REPORT
```

## 4GL 注释

```plsql
REPORT test_rep(sr)
 ...
 {FORMAT
 PAGE HEADER
 BEFORE GROUP
 } #批量注释
 # ON EVERY ROW 单行注释
 -- AFTER GROUP 单行注释
 ...
END REPORT

```

## 函数调用

```plsql
MAIN
 DEFINE var1 CHAR(10)
 DEFINE var2 CHAR(2)
 LET var1 = foo()
 DISPLAY "var1 = " || var1
 CALL foo() RETURNING var2
 DISPLAY "var2 = " || var2
END MAIN
FUNCTION foo()
 RETURN "Hello"
END FUNCTION
```

```plsql
MAIN
 IF foo() THEN
 DISPLAY "Choice is OK!"
 END IF
END MAIN
FUNCTION foo()
 RETURN TRUE
END FUNCTION

```

## 内置函数

> 常用内置函数

- ARG_VAL(N) 回传第 N 个位置上的外部传入参数值。N=0 时表执行程序名称。
- NUM_ARGS() 回传外部传入参数的总个数
- ARR_COUNT() 在 INPUT ARRAY 指令內可回传传入的阵列行数
- ARR_CURR() 回传 DISPLAY ARRAY 或 INPUT ARRAY.指令下现在游標所在行数
- ERRORLOG() 将现有的错误信息写入事先定义好的 error log 记录档
- FGL_BUFFERTOUCHED() 当栏位输入的暂存区被异动到时回传 TRUE
- FGL_GETENV() 抓取指定的环境变量资料
- FGL_SETENV() 设定指定的环境变量资料
- FGL_GETFILE() 从 GDC 端抓取档案到主机上
- FGL_PUTFILE() 从主机放档案到 GDC 端
- FGL_GETPID( ) 回传本次程序执行使用的 PID
- FGL_GETRESOURCE( ) 回传指定 FGLPROFILE 內的 entry 值
- FGL_SET_ARR_CURR() 设定移动到指定 ARRAY 位置
- DOWNSHIFT() 将传入的字串全部变成小写字串
- UPSHIFT() 将传入的字串全部变成大写字串
- LENGTH() 回传字串（CHAR/VARCHAR）的長度

