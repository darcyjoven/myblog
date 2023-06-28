---
title: "表单控制流"
date: 2022-04-14T10:04:30+08:00
lastmod: 2022-04-14T10:04:30+08:00
tags: ["表单控制流"]
categories: ["ERP", "TIPTOP"]
# bookComments: false
# bookSearchExclude: false
---


## MENU

> MENU 指令定义了一组选项，用户可以选择这些选项来触发程序中的操作。

```plsql
MENU [title]
   [ ATTRIBUTES ( menu-attribute [,...] ) ]
   [ BEFORE MENU
       menu-statement
     [...]
   ]
   menu-option
   [...]
END MENU
```

### menu-option

```plsql
{ COMMAND option-name
      [option-comment] [ HELP help-number ]
    menu-statement
    [...]
| COMMAND KEY ( key-name ) option-name
      [option-comment] [ HELP help-number ]
    menu-statement
    [...]
| COMMAND KEY ( key-name )
    menu-statement
    [...]
| ON ACTION action-name
    menu-statement
    [...]
| ON IDLE idle-seconds  #闲置时间
    menu-statement
    [...]
}
```

### menu-statement

```plsql
{ statement
|  CONTINUE MENU
|  EXIT MENU
|  NEXT OPTION option
|  SHOW OPTION { ALL | option [,...] }
|  HIDE OPTION { ALL | option [,...] }
}
```

### menu-attribute

```plsql
{ STYLE = { "default" | "popup" | "dialog" }
| COMMENT = "string"
| IMAGE = "string"
}
```

- COMMAND

```plsql
MAIN
 :
MENU ""
 COMMAND "A.add" #menu 功能名称"A.新增"，a 为快速键
 CALL bdl1_a()
 COMMAND "U.modi"
 CALL bdl1_u()
 COMMAND "Q.qry"
 CALL bdl1_q()
 COMMAND KEY (CONTROL-A) # CONTROL-A 为快速键且
 CALL bdl1_a() #此功能不 SHOW MENU 上
 COMMAND "exit"
 EXIT PROGRAM
 END MENU
END MAIN
```

![](/post/mk_img/2022-04-14-10-08-41.png)

> ON ACTION 和 COMMAND 功能相似，ON ACTION 不允许重复

- title 是定义菜单标题的字符串表达式。
- menu-attribute 是定义菜单行为和显示的属性。
- key-name 名称是一个热键标识符（如 F11 或 Control-z）。
- option-name 是一个字符串表达式，用于定义菜单选项的标签，并标识用户可以执行的操作。
- option-comment 是一个字符串表达式，包含菜单选项的描述，当选项名称为当前名称时显示。
- help-number 是一个整数，允许您将帮助消息编号与菜单选项相关联。
- action-name 标识用户可以执行的操作。
- idle-seconds 是定义秒数的整数文字或变量。
- action-name 标识用户可以执行的操作。

## INPUT

> 在使用 INPUT 指令前必须先开启画面格式档，将画面显示在萤幕上。

```plsql
INPUT BY NAME { variable | record.* } [,...]
  [ WITHOUT DEFAULTS ]  #保留原变量名
  [ ATTRIBUTES (
          { display-attribute
          | control-attribute
          } [,...] ) ]
  [ HELP help-number ]
  [ dialog-control-block
      [...]
END INPUT ]
```

```plsql
INPUT { variable | record.* } [,...]
  [ WITHOUT DEFAULTS ]
  FROM
  field-list
  [ ATTRIBUTES (
          { display-attribute
          | control-attribute
          } [,...] ) ]
  [ HELP help-number  ]
  [ dialog-control-block
      [...]
END INPUT ]
```

### dialog-control-block

```plsql
{ BEFORE INPUT
| AFTER INPUT
| BEFORE FIELD field-spec [,...]
| AFTER FIELD field-spec [,...]
| ON CHANGE field-spec [,...]
| ON IDLE idle-seconds
| ON ACTION action-name [INFIELD field-spec]
| ON KEY ( key-name [,...] )
}
    dialog-statement
    [...]
```

### dialog-statement

```plsql
{ statement
| ACCEPT INPUT
| CONTINUE INPUT
| EXIT INPUT
| NEXT FIELD
   { CURRENT
   | NEXT
   | PREVIOUS
   | field-spec
   }
}
```

### field-list

```plsql
{ field-name
| table-name.*
| table-name.field-name
| screen-array[line].*
| screen-array[line].field-name
| screen-record.*
| screen-record.field-name
} [,...]
```

### field-spec

```plsql
{ field-name
| table-name.field-name
| screen-array.field-name
| screen-record.field-name
}
```

### display-attribute

```plsql
{ BLACK | BLUE | CYAN | GREEN
| MAGENTA | RED | WHITE | YELLOW
| BOLD | DIM | INVISIBLE | NORMAL
| REVERSE | BLINK | UNDERLINE
}
```

### control-attribute

```plsql
{ ACCEPT [ = boolean ]
| CANCEL [ = boolean ]
| FIELD ORDER FORM
| HELP = help-number
| NAME = "dialog-name"
| UNBUFFERED [ = boolean ]  #变量改变是否影响画面
| WITHOUT DEFAULTS [ = boolean ]
}
```

## PROMPT

> 若只有简单的问题要询问使用者，可考虑使用 PROMPT 指令，此指令只需宣告并告知所需要询问的问题，就可开出一个小视窗，对使用者询问并取回答案。

```plsql
PROMPT question
    [ ATTRIBUTES ( display-attribute [,...] ) ]
    FOR [CHAR[ACTER]] variable
    [ HELP number ]
    [ ATTRIBUTES ( control-attribute [,...] ) ]
[ dialog-control-block
   [...]
   END PROMPT ]
```

### control-attribute

```plsql
{ ACCEPT [ = boolean ]
| CANCEL [ = boolean ]
| CENTURY = "century-spec"
| FORMAT = "format-spec"
| PICTURE = "picture-spec"
| SHIFT = { "up" | "down" }
| HELP = help-number
| COUNT = row-count
| UNBUFFERED [ = boolean ]
| WITHOUT DEFAULTS [ = boolean ]
}
```

#### CENTURY

> 日期取得的算法

```plsql
CENTURY = { "R" | "C" |"F" | "P" }
```

- C 使用最接近当前日期的过去、未来或当前年份。
- F 使用最近的未来年份扩展输入值。
- P 使用过去最接近的年份来扩展输入的值。
- R 用当前年份的前两位数字作为输入值的前缀。

#### FORMAT

> 数字格式化

#### PICTURE

> PICTURE 属性为文本字段中的数据输入指定字符模式，并防止输入与指定模式冲突的值。类似简单的正则表达式。

- "A" 表示一个字母字符
- "#" 表示一个任何数字字符
- "X" 表示一个任何字符
- 其它字符匹配自身

```plsql
PICTURE "1##-####-####"  #匹配手机号码
```

#### SHIFT

> 强制大写或者小写

## DISPLAY

> 将程式数组的内容透过 TABLE、TREE 等元件显示。

```plsql
DISPLAY ARRAY array TO screen-array.*
  [ HELP help-number ]
  [ ATTRIBUTES ( { display-attribute
                   | control-attribute }
                       [,...]) ]
  [  dialog-control-block
  [...]
END DISPLAY ]
```

### dialog-control-block

```plsql
{ BEFORE DISPLAY
| AFTER DISPLAY
| BEFORE ROW
| AFTER ROW
| ON IDLE idle-seconds
| ON ACTION action-name
| ON FILL BUFFER
| ON APPEND
| ON INSERT
| ON UPDATE
| ON DELETE
| ON EXPAND ( row-index )
| ON COLLAPSE ( row-index )
| ON DRAG_START ( dnd-object )
| ON DRAG_FINISH ( dnd-object )
| ON DRAG_ENTER ( dnd-object )
| ON DRAG_OVER ( dnd-object )
| ON DROP ( dnd-object )
| ON KEY ( key-name [,...] )
}
  dialog-statement
  [...]
```

### dialog-statement

```plsql
{ statement
| EXIT DISPLAY
| CONTINUE DISPLAY
| ACCEPT DISPLAY
}
```

### display-attribute

```plsql
{ BLACK | BLUE | CYAN | GREEN
| MAGENTA | RED | WHITE | YELLOW
| BOLD | DIM | INVISIBLE | NORMAL
| REVERSE | BLINK | UNDERLINE
}
```

### control-attribute

```plsql
{ ACCEPT [ = boolean ]
| CANCEL [ = boolean ]
| KEEP CURRENT ROW [ = boolean ] ??
| HELP = help-number
| COUNT = row-count
| UNBUFFERED [ = boolean ]
}
```

per 文件内容

```plsql
SCHEMA shop

LAYOUT
TABLE
{
 Id       Name         LastName
[f001    |f002        |f003        ]
[f001    |f002        |f003        ]
[f001    |f002        |f003        ]
[f001    |f002        |f003        ]
[f001    |f002        |f003        ]
[f001    |f002        |f003        ]
}
END
END

TABLES
  customer
END

ATTRIBUTES
  f001 = customer.id;
  f002 = customer.fname;
  f003 = customer.lname;
END

INSTRUCTIONS
  SCREEN RECORD srec[6] (customer.*);
END
```

```plsql
SCHEMA shop

MAIN

  DEFINE cnt INTEGER
  DEFINE arr DYNAMIC ARRAY OF RECORD LIKE customer.*

  DATABASE shop

  OPEN FORM f1 FROM "custlist"
  DISPLAY FORM f1

  DECLARE c1 CURSOR FOR
    SELECT id, fname, lname FROM customer
  LET cnt = 1
  FOREACH c1 INTO arr[cnt].*
    LET cnt = cnt + 1
  END FOREACH
  CALL arr.deleteElement(cnt)

  DISPLAY ARRAY arr TO srec.*
    ON ACTION print
       DISPLAY "Print a report"
  END DISPLAY

END MAIN
```

## CONSTRUCT

> 此指令可让使用者在画面上输入查询条件（通称 Query By Example；QBE），以取得
> 使用者的查询范围资料。使用者的查询资料会组成一串 WHERE 指令（参下页注解），
> 并置入设定好的变数中。若使用者未输入任何条件，即按下‘确定’离开 CONSTRUCT，
> 系统也会自动于此变数中补入'1=1'。

```plsql
CONSTRUCT BY NAME variable ON column-list
    [ ATTRIBUTES ( { display-attribute
                     | control-attribute }
                     [,...] ) ]
    [ HELP help-number ]
[ dialog-control-block
  [...]
END CONSTRUCT ]
```

```plsql
CONSTRUCT variable ON column-list FROM field-list
    [ ATTRIBUTES ( { display-attribute
                     | control-attribute
                     } [,...] ) ]
    [ HELP help-number ]
[ dialog-control-block
   [...]
END CONSTRUCT ]
```

### column-list

```plsql
{ column-name
| table-name.*
|  table-name. column-name
} [,...]
```

### dialog-control-block

```plsql
{ field-name
| table-name.*
| table-name.field-name
| screen-array[line].*
| screen-array[line].field-name
| screen-record.*
| screen-record.field-name
} [,...]
```

```plsql
{ BEFORE CONSTRUCT
| AFTER CONSTRUCT
| BEFORE FIELD field-spec [,...]
| AFTER FIELD field-spec [,...]
| ON IDLE idle-seconds
| ON ACTION action-name [INFIELD field-spec]
| ON KEY ( key-name [,...] )
}
    dialog-statement
    [...]
```

### dialog-statement

```plsql
{ statement
| NEXT FIELD { NEXT | PREVIOUS | field-spec}
| CONTINUE CONSTRUCT
| EXIT CONSTRUCT
}
```

### field-spec

```plsql
{ field-name
| table-name.field-name
| screen-array.field-name
| screen-record.field-name
}
```

### display-attribute

```plsql
{ BLACK | BLUE | CYAN | GREEN
| MAGENTA | RED | WHITE | YELLOW
| BOLD | DIM | INVISIBLE | NORMAL
| REVERSE | BLINK | UNDERLINE  #颠倒|闪烁|下划线
}
```

### control-attribute

```plsql
{ ACCEPT [ = boolean ]
| CANCEL [ = boolean ]
| FIELD ORDER FORM
| HELP = help-number
| NAME = "dialog-name"
}
```

## DIALOG

> INPUT/CONSTRUCT 等指令使用时，必须等待上一个指令结束才能开始下一个。
> 使用 DIALOG 将 INPUT/CONSTRUCT 指令包裹起来输入的指令栏位可以自由移动。

```plsql
DIALOG [ ATTRIBUTE ( { display-attribute | control-attribute } [,...] ) ]
  BEFORE DIALOG
  AFTER DIALOG
  ON IDLE idle-seconds
  ON ACTION action-name
END DIALOG

```

- DIALOG 不支援 INT_FLAG 侦测，因此若需中断程式处理必需靠 ON ACTION 达成
- 离开 DIALOG 只能使用 EXIT DIALOG 或 ACCEPT DIALOG 指令
- 当 DIALOG 内只存在一组内层指令时，建议勿使用 DIALOG

```plsql
SCHEMA stores
DEFINE p_customer RECORD LIKE customer.*
DEFINE p_orders DYNAMIC ARRAY OF RECORD LIKE order.*
FUNCTION customer_dialog()
 DIALOG ATTRIBUTES(UNBUFFERED, FIELD ORDER FORM)
 INPUT BY NAME p_customer.* # sub-DIALOG
 AFTER FIELD cust_name
 CALL setup_dialog(DIALOG)
 END INPUT
 DISPLAY ARRAY p_orders TO s_orders.* # sub-DIALOG
 BEFORE ROW
 CALL setup_dialog(DIALOG)
 END DISPLAY
 ON ACTION close # Action for DIALOG
 EXIT DIALOG
 END DIALOG
END FUNCTION
```

## SUBDIALOG

> 将画面控制流放在 DIALOG 中，可以当作函数一样调用。

orders.4gl

```plsql
SCHEMA stock
DEFINE arr DYNAMIC ARRAY OF RECORD LIKE orders.*
DIALOG orders_dlg() #DIALOG 指令若后方有 id，则视为和 FUNCTION 相同
 DEFINE x INT
 DISPLAY ARRAY arr TO sr_orders.*
 ...
 END DISPLAY
END DIALOG
```

调用时

```plsql
IMPORT FGL orders #使用 IMPORT 指令将 42m 于编译时读入
SCHEMA stock
DEFINE rec RECORD LIKE customers.*
FUNCTION co_list()
 DIALOG ...
 INPUT BY NAME rec.*
 ...
 END INPUT
 ...
 SUBDIALOG orders.orders_dlg #使用 SUBDIALOG 指令引入
 ...
 END DIALOG
END FUNCTION
```
