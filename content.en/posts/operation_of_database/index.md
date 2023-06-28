---
title: "数据库操作"
date: 2022-04-14T10:03:17+08:00
tags: ["数据库操作"]
categories: ["ERP", "TIPTOP"]
# bookComments: false
# bookSearchExclude: false
---


> 在 Genero FGL 语言中，当下达‘DATABASE dstabase_id’指令后，即与资料库执行连结，也就是可以开始进行资料的查询（SELECT）、或更改（UPDATE）、新增（INSERT）等动作。

## 原生 SQL 语句

> Genero FGL 支持原生的一部分 SQL 语句

### INSERT

```plsql
INSERT INTO table-specification [ ( column [,...] ) ]
{
 VALUES ( { variable | sql-expression } [,...] )
|
 select-statement
}
```

```plsql
INSERT INTO table-specification VALUES ( record.* )
```

- table-specification [dbname[@dbserver]:][owner.]table

### DELETE

```plsql
DELETE FROM table-specification
   [ WHERE { condition | CURRENT OF cursor } ] #cursor 指的open语句的cursor
```

### UPDATE

```plsql
UPDATE table-specification
   SET
       column = { variable | sql-expression }
       [,...]
   [  sql-condition ]
```

```plsql
UPDATE table-specification
   SET ( column [,...] )
     = ( { variable | sql-expression } [,...] )
   [  sql-condition ]
```

```plsql
UPDATE table-specification
   SET [table.]*
     = ( { variable | sql-expression } [,...] )
   [  sql-condition ]
```

```plsql
UPDATE table-specification
   SET { [table.]* | ( column [,...] ) }
     = record.*
   [  sql-condition ]
```

### SELECT

```plsql
SELECT [subset-clause] [duplicates-option] { * | select-list }
  [ INTO variable [,...] ]
  FROM table-list [,...]
  [ WHERE condition ]
  [ GROUP BY column-list [ HAVING condition ]]
  [ ORDER BY column [{ASC|DESC}] [,...] ]
```

- subset-clause

```plsql
[ SKIP { integer | variable }] ??
[ {FIRST|MIDDLE|LIMIT} { integer | variable ] ??
```

- duplicates-option

```plsql
{ ALL
| DISTINCT
| UNIQUE
}
```

- select-list

```plsql
{ [@]table-specification.*
| [table-specification.]column
| literal
} [ [AS] column-alias ]
[,...]
```

- table-list

```plsql
{ table-name
| OUTER table-name
| OUTER ( table-name [,...] )
}
[,...]
```

```plsql
MAIN
   DEFINE myrec RECORD
            key INTEGER,
            name CHAR(10),
            cdate DATE,
            comment VARCHAR(50)
         END RECORD
   DATABASE stock
   LET myrec.key = 123
   SELECT name, cdate
     INTO myrec.name, myrec.cdate
     FROM items
     WHERE key=myrec.key
END MAIN
```

## CURSOR

> 直接运行 SQL，SELECT 只支持一笔资料，且不支持嵌套等复杂 SQL，这时候需要利用 CURSOR 等方式操作数据库。

### PREPARE

> PREPARE 将一个 SQL 字符串变为可以执行资料。

```plsql
PREPARE sid FROM sqltext
```

- sqltext 为组成的 SQL 字符串，可以用？用作占位符。
- sid 不可重复

### FREE

释放 PREPARE 记录

```plsql
LET l_str = "employee='1000' AND salary>'30000’ "
LET l_sql = " SELECT * FROM employee_file WHERE ",l_str
PREPARE emp_pre FROM l_sql
…
FREE emp_pre

```

### EXECUTE

```plsql
FUNCTION deleteOrder(n)
  DEFINE n INTEGER
  PREPARE s1 FROM "DELETE FROM order WHERE key=?"
  EXECUTE s1 USING n
  FREE s1
END FUNCTION
```

### SCROLLING CURSOR

> 一次抓取一笔，读取时，可以抓取前一笔，后一笔。

```plsql
DECLARE cursor_id SCROLL CURSOR [WITH HOLD] FOR sql statement
OPEN cursor_id [USING value]
FETCH [first|last|previous|next| cursor_id INTO variable
CLOSE cursor_id
```

> WITH HOLD 能保证循环中，commit 提交事务，游标 cursor 不会关闭。

### OPEN

```plsql
OPEN cid
   [ USING pvar {IN|OUT|INOUT} [,...] ]
```

> OPEN 打开游标，如果 SQL 语句中含有 FOR UPDATE [NOWAIT]，这时候就开始锁表。

> IN|OUT|INOUT 用于执行计划函数参数和输出

### Non-SCROLLING CURSOR

> 符合条件资料都被抓取出来，直到结束

```plsql
DECLARE cursor_id CURSOR [WITH HOLD] FOR sql statement
 FOREACH cursor_id
 [USING value]
 [INTO variable ]
 …
END FOREACH

```

> 在 DECLARE 前可用 PREPARE 指令执行 SQL 字串的转换及检查

### FETCH

```plsql
FETCH [first|last|previous|next| cursor_id INTO variable
```

![](/post/mk_img/2022-04-14-10-15-17.png)

### FOREACH

```plsql
FOREACH cid
   [ USING pvar {IN|OUT|INOUT} [,...] ]
   [ INTO fvar [,...] ]
   [ WITH REOPTIMIZATION ]  #执行计划
       {
         statement
       | CONTINUE FOREACH
       | EXIT FOREACH
       }
       [...]
END FOREACH
```

### CLOSE

> 关闭并释放指标（CURSOR）的储存空间。

```plsql
CLOSE cursor_id
```

## TRANSACTION 事务

```plsql
MAIN
  DATABASE stock
  BEGIN WORK #开启事务
  INSERT INTO items VALUES ( ... )
  IF SQLCA.SQLCODE < 0 THEN
    ROLLBACK WORK   #回滚事务
  ELSE
  UPDATE items SET ...
  COMMIT WORK #提交事务
END MAIN
```

## PUT..FLUSH

> 大量执行新增命令，PUT FLUSH 比 INSERT 和 EXECUTE 速度都要快

```plsql
PREPARE prepared_id FROM INSERT_sql_statement
DECLARE cursor_id [WITH HOLD] FOR prepared_id
OPEN cursor_id
PUT cursor_id FROM variable_list
FLUSH cursor_id
CLOSE cursor_id
FREE cursor_id
FREE prepared_id
```

```plsql
MAIN
 	DEFINE i INTEGER
 	DEFINE rec RECORD
 					key INTEGER,
 					name CHAR(30)
 				END RECORD
 DATABASE stock
 PREPARE is FROM "INSERT INTO item VALUES (?,?)"
 DECLARE ic CURSOR FOR is
 BEGIN WORK
 	OPEN ic
 	FOR i=1 TO 100
   	LET rec.key = i
 		LET rec.name = "Item #" || i
 		PUT ic FROM rec.*
 		IF i MOD 50 = 0 THEN
 			FLUSH ic
 		END IF
 	END FOR
 	CLOSE ic
 	COMMIT WORK
 	FREE ic
 	FREE is
END MAIN

```
