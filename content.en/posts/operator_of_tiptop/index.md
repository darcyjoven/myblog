---
title: "TIPTOP运算符"
date: 2021-12-24T14:07:00+08:00
# bookComments: false
# bookSearchExclude: false
tags: ["TIPTOP"]
categories: ["TIPTOP"]
---
 
## 比较运算
| 运算符 | 说明 | 示例 |
| --- | --- | --- |
| IS NULL  | 空白 |  IF a IS NULL THEN…   |
| LIKE | 符合指定语法 |  IF "abcdef" LIKE "a%e_" THEN   |
| MATCHES | 符合其中一项 |  IF a MATCHES “[1234]” THEN   |
| ==、= | 等于 |  IF a == 10 THEN  ... |
|  !=、<> | 不等于 |  IF a != 10 THEN…   |
| <、<= | 小于，小于等于 |  IF a <= 10 THEN   |
| >、>= | 大于，大于等于 |  IF a >= 10 THEN   |
| NVL(a,b) | a为NULL，就把b值赋给a |  |
| IIF(a,b,c) | 三元表达式 |  |
| := | 指定值 |  LET a = b := 10   |
| NOT  | 非 |  IF NOT a = b THEN…   |
| AND | 与 |  IF a = b AND c = d THEN…   |
| OR | 或 |  IF a = b OR c = d THEN…   |

 
## 数值运算
 
## 字符串运算
| 运算符 | 说明 | 示例 |
| --- | --- | --- |
| , | 字串连接，可连接空字符串 |  LET a = a, b   |
| || | 字符串连接，只要有一个空结果就为空 |  LET a = a||b   |
| [start,end] | 截取字符串一部分 |  LET a = a[1,10]   |
| USING | 格式化字符串 |  LET a = a USING “###”   |
| CLIPPED | 消除尾部空白 |  LET a = a CLIPPED   |
| SPACES | 输出空白字符 |  LET a = 10 SPACES   |
| ASCII() | 已ASCII输入文字 |  LET a = ASCII 37   |
| ORD() | 取字符ASCII码 |  LET I = ORD( “A” )   |
| LSTR() | 取本地化字符串 |  DISPLAY LSTR ("str")   |
| SFMR(...) | 字符串拼接 |  SFMT("Order #%1 has been %2.",n,"deleted")   |

 
### USING
> USING 可以针对数值和日期设置字符串显示格式，使用要注意溢出问题。

 
#### 数值格式标识
| * | 空白的地方用*显示 |
| --- | --- |
| & | 空白的地方用0显示 |
| # | 无影响，但限制字符的输出最大长度 |
| < | 数字向左靠 |
| , | 指定逗号出现位置 |
| . | 指定小数点出现位置 |
| - | 当数字小于0加上一个-号 |
| + | 当数字小于0加上一个-号，当数字大于0加上一个+号 |
| $ | 数值出现一个签字符号 |
| ( | 数值小于0，加上左括号 |
| ) | 素质小于0，加上又括号 |

 
#### 日期格式标识
| dd | 两位数字表示日期 |
| --- | --- |
| ddd | 三位英文表示星期 |
| mm | 两位数字表示月份 |
| mmm | 三位英文表示月份 |
| yy | 2为数字表示年份 |
| yyyy | 4位数字表示年份 |

 
#### 示例
LET i = 12345<br />LET j = -12345

| 格式方式 | 显示内容 |
| --- | --- |
| DISPLAY i | -- Display: 12345 |
| DISPLAY j | -- Display: -12345 |
| DISPLAY i USING"***" | -- Display:**12345 |
| DISPLAY j USING"***" | -- Display:**12345 |
| DISPLAY i USING"&&&&&&&" | -- Display:0012345 |
| DISPLAY j USING"&&&&&&&" | -- Display:0012345 |
| DISPLAY i USING"#######" | -- Display: 12345 |
| DISPLAY j USING"#######" | -- Display: 12345 |
| DISPLAY i USING"<<<<<<<" | -- Display:12345 |
| DISPLAY j USING"<<<<<<<" | -- Display:12345 |
| DISPLAY i USING"-------" | -- Display: 12345 |
| DISPLAY j USING"-------" | -- Display: -12345 |
| DISPLAY i USING"+++++++" | -- Display: +12345 |
| DISPLAY j USING"+++++++" | -- Display: -12345 |
| DISPLAY i USING"$$$$$$$" | -- Display: $12345 |
| DISPLAY j USING"$$$$$$$" | -- Display: $12345 |
| DISPLAY i USING"(######)" | -- Display: 12345 |
| DISPLAY j USING"(######)" | -- Display:( 12345) |
| DISPLAY i USING"###,###.&&" | -- Display: 12,345.00 |
| DISPLAY j USING"###,###.&&" | -- Display: 12,345.00 |

| 格式方式 | 显示内容 |
| --- | --- |
| DISPLAY TODAY USING "yyyy-mm-dd" | -- Display 2004-06-15 |
| DISPLAY TODAY USING "yy-mm-dd" | -- Display 04-06-15 |
| DISPLAY TODAY USING "yy-mmm-ddd | -- Display 04-Jun-Tue |

 
## 日期运算
| 运算符 | 说明 | 示例 |
| --- | --- | --- |
|  TODAY  | 取出今天日期 |  LET a = TODAY( )   |
|  CURRENT   | 获取当前日期和时间 |  LET a = CURRENT |
|  DATE( )   | 转化为日期格式 |  LET a = DATE( "07/31/2005" )   |
|  MDY( )  | 构造日期 |  LET a= MDY( 07, 31, 2005 )   |
|  TIME( )   | 取出时间（hh:mm:ss） |  LET a = TIME ( CURRENT )   |
|  YEAR( )   | 取出年 |  LET a = YEAR( CURRENT )   |
|  MONTH( )   | 取出月 |  LET a = MONTH( CURRENT ) |
|  DAY( )   | 取出日   |  LET a = DAY( CURRENT )   |
|  WEEKDAY( )  | 获取今天是周几 |  LET a = WEEKDAY(CURRENT)   |
|  EXTEND(a, b TO c )   | 将a日期调整为(b TO c)的格式 |  DISPLAY EXTEND ( TODAY, YEAR TO FRACTION(4) )   |
|  UNITS   | 数值转化为 interval*   |  LET a = 10 + (20 UNITS MINUTES)   |

