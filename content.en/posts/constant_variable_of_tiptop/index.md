---
title: "变量和常量的定义"
date: 2022-04-14T10:00:07+08:00
lastmod: 2022-04-14T10:00:07+08:00
draft: false
tags: ["变量和常量","CONSTANT","VARIABLE"]
categories: ["ERP", "TIPTOP"]
# bookComments: false
# bookSearchExclude: false
---


## 定义变量/常量

> 强类型语言

```plsql
DEFINE 变量名称 数据类型
CONSTANT 常量名称 （数据类型） = 常量值
```

| 类型名称          | 说明                                                                   |
| ----------------- | ---------------------------------------------------------------------- |
| CHAR[(n)]         | Fixed size character strings（固定长度字符串）                         |
| VARCHAR[(n[,r])]  | Variable size character strings（可变长度字符串）                      |
| STRING            | Dynamic size character strings（动态长度字符串）                       |
| DATE              | Simple calendar dates（日期）                                          |
| DATETIME q1 TO q2 | High precision date and hour data（高精度日期）                        |
| INTERVAL q1 TO q2 | High precision time intervals（高精度时间间隔）                        |
| BIGINT            | 8 byte signed integer（8 字节整数）                                    |
| INTEGER           | 4 byte signed integer（4 字节整数）                                    |
| SMALLINT          | 2 byte signed integer（2 字节整数）                                    |
| TINYINT           | 1 byte signed integer（1 字节整数）                                    |
| FLOAT[(p)]        | 8 byte floating point decimal（8 字节浮点十进制）                      |
| SMALLFLOAT        | 4 byte floating point decimal（4 字节浮点十进制）                      |
| DECIMAL[(p[,s])]  | High precision decimals（高精度小数）                                  |
| MONEY[(p[,s])]    | High precision decimals with currency formatting（高精度货币格式小数） |
| BYTE              | Large binary data (images)（大二进制数据）                             |
| TEXT              | Large text data (plain text)（大文本数据）                             |
| BOOLEAN           | TRUE/FALSE boolean（是/否 布尔类型）                                   |

```plsql
DEFINE variable-definition  LIKE [dbname:]tabname.colname
ex: DEFINE stu_id LIKE student.id
```

利用 LIKE 可以直接根据数据库字段类型定义变量/常量

### 变量的设定

```plsql
LET variable = expression
```

### string 类型

> string 类型除了存储资料，还有一些特殊功能

| 方法名称                                            | 功能说明                                  |
| --------------------------------------------------- | ----------------------------------------- |
| append( str STRING ) RETURNING STRING               | 将传入字串加到原来的 STRING 后。          |
| equals( src STRING ) RETURNING INTEGER              | 判断原字串与传入字串是否相等。            |
| equalsIgnoreCase( src STRING ) RETURNING INTEGER    | 判断原字串与传入字串是否相等（忽略大小写  |
| 差异）。                                            |
| getCharAt( pos INTEGER ) RETURNING STRING           | 抓取指定位置的字元。                      |
| getIndexOf( str STRING, spos INT) RETURNING INTEGER | 从 INT 处开始寻找，判断是否含有传入字串。 |
| getLength( ) RETURNING INTEGER                      | 计算此字串总长度。                        |
| subString( spos INT, epos INT ) RETURNING STRING    | 切截出指定起点至终点的子字串。            |
| toLowerCase( ) RETURNING STRING                     | 将字串转换为小写字。                      |
| toUpperCase( ) RETURNING STRING                     | 将字串转换为大写字。                      |
| trim( ) RETURNING STRING                            | 切掉字串头尾两侧的空白字元。              |
| trimLeft( ) RETURNING STRING                        | 切掉字串头端的空白字元。                  |
| trimRight( ) RETURNING STRING                       | 切掉字串尾端的空白字元。                  |

## 结构体/变量集合

> 只有变量，没有方法，构造函数等功能

```plsql
DEFINE variable RECORD
  member  datatype | LIKE [dbname:]tabname.colname,
  ...
END RECORD

ex:
DEFINE student RECORD
  stu_id varchar(20),
  stu_name varchar(40),
  stu_age INTEGER,
  stu_sex char(1)
END RECORD

DEFINE variable RECORD LIKE [dbname:]tabname.*

ex:
DEFINE student RECORD LIKE student.*
```

## 数组

> Genero 支持数组，多维数组。index 索引从"1"开始。

```plsql
DEFINE variable ARRAY [ size [,size  [,size] ] ] OF datatype
DEFINE variable DYNAMIC ARRAY [ WITH DIMENSION rank ] OF datatype


ex:
DEFINE students ARRAY[10] OF RECORD LIKE [dbname:]tabname.*
   students[1]
   students[2]
ex:
DEFINE arr_int DYNAMIC ARRAY  WITH DIMENSION rank  2 OF INTEGER
   arr_int[1,1]
   arr_int[2,2]
```

| 方法名称                       | 说明                                   |
| ------------------------------ | -------------------------------------- |
| getLength( ) RETURNING INTEGER | 获取数组长度                           |
| clear()                        | 动态数组，所有记录会被删除             |
| 固定数组，所有制清空为 NULL    |
| appendElement()                | 在动态数组后新增一笔                   |
| insertElement( INTEGER )       | 在指定位置新增一笔记录，之前资料会后移 |
| deleteElement( INTEGER )       | 移除指定位置的记录，或者设置为 NULL    |

## 资料结构

```plsql
TYPE t_customer RECORD
            cust_num INTEGER,
            cust_name VARCHAR(50),
            cust_addr VARCHAR(200)
    END RECORD

MAIN
    DEFINE custrec t_customer
    DEFINE custarr DYNAMIC ARRAY OF t_customer
    DEFINE index INTEGER

    LET custrec.cust_num = 123
    ...

    LET custarr[index].* = custrec.*
    ...

END MAIN
```

## 预定义完成变量

| 变量名称 | 说明                                          |
| -------- | --------------------------------------------- |
| INT_FLAG | 系统"中断键"时，变为 TURE，返回程序便会 FALSE |
| STATUS   | 保存上个指令执行状态                          |
| SQLCA    | 一组预定义的 Records，常用项目如下：          |

SQLCA.SQLCODE：SQL 执行结果(0=Ok，100=Not Found，<0=error)
SQLCA.SQLERRD[1]：暂未使用（T100 未用）
SQLCA.SQLERRD[2]：异动资料序号或<0 的错误码（100 未用)
SQLCA.SQLERRD[3]：异动资料笔数
SQLCA.SQLERRD[4]：估计耗费 CPU 资源数（COST，T100 未用）SQLCA.SQLERRD[5]：SQL 错误描述（T100 未用）
SQLCA.SQLERRD[6]：最后异动资料的 rowid（T100 未用） |
| SQLSTATE | 列出上一次发生的 SQL 错误状态码（数据库提供） |
| SQLERRMESSAGE | 列出上一次发生的 SQL 错误说明（数据库提供） |

## 变量生命周期

### 全局变量

> Global Variables

```plsql
GLOBALS
  declaration-statement
  [,...]
END GLOBALS

GLOBALS "filename"
```

定义位置： 由 GLOBALS 及 END GLOBALS 所包围的变量。
生命周期：使用的所有 MODULE 的公共变量。

### 模组变量

> Module Variables

```plsql
DEFINE g_tty CHAR(32)
MAIN
END MAIN
```

定义位置： Module 中，但不被任何函数包围。
生命周期：当前 Module 中的公共变量。

### 局部变量

> Local Variables

```plsql
FUNCTION ins_employee()
 DEFINE flag CHAR(1),
 change SMALLINT
END FUNCTION
```

定义位置： 定义在 Module 中，并且在函数中。
生命周期：只属于当前定义函数中使用。
