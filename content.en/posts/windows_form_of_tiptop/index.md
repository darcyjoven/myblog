---
title: TIPTOP表单窗口FORM
date: 2022-04-14T10:04:02+08:00
lastmod: 2022-04-14T10:04:02+08:00
draft: false
tags: ["Windows_Form"]
categories: ["ERP", "TIPTOP"]
# bookComments: false
# bookSearchExclude: false
---



> windows 和 form 本身无法直接运行，需要借助程序开启。
> window 由 4gl 源码创建，使用 open window 指令，form 时指 42f 档案描述的画面结构。若需要读取以及创元的 form 作为 window，可以利用 open window with form 指令。开启的 window 和 form 可以使用 ui.window 和 ui.form 内部物件进行操作。

## OPEN WINDOW

```plsql
OPEN WINDOW window-id [AT line, column ]
WITH [ FORM form-file | height ROWS, width COLUMNS ][ ATTRIBUTES ( window-attributes ) ]
```

- window-id：定义这个 window name 名称。
- AT line, column：表示让画面开启的起始座标，仅限主控台﹝Console﹞生效。
- form-file：经过编译后的画面档档案名称(不含附档名)，之前可以指定放置路径。
- height ROWS, width COLUMNS：指定画面佔用行数及栏数。
- ATTRIBUTES ( window-attributes )：可以加上属性设定。
  | Attribute | 系统 Default 说明 |
  | --- | --- |
  | TEXT = string | NULL 将 string 显示在视窗标题列 |
  | STYLE = string | NULL 读取 string 的画面设定属性【注】 |
  | BORDER | No border 加边框 (仅限于主控台模式) |
  | REVERSE | No reverse 反白模式 (仅限于主控台模式) |

> 【注】Genero 画面设定档，附档名为'.4st'，T100 置于'$ERP/cfg/4st'下。

### ui.Window 物件

- class 方法
  | 名称 | 说明 |
  | --- | --- |
  | ui.Window.forName(name STRING ) RETURNING result ui.Window | 抓取指定名称的 WINDOW 到物件内 |
  | ui.Window.getCurrent() RETURNING result ui.Window | 指定现行 WIDNOW 到物件内 |

- object 方法
  | 名称 | 说明 |
  | --- | --- |
  | getForm() RETURNING result ui.Form | 抓取现行的 FORM 物件 |
  | getNode() RETURNING result om.DomNode | 抓取现行 WINDOW 节点 |
  | findNode( type STRING, name STRING ) RETURNING result om.DomNode | 搜寻指定 WINDOW 节点 |
  | createForm( name STRING ) RETURNING result ui.Form | 建立新画面 |
  | setText(text STRING ) | 设定画面上方标题 |
  | getText() RETURNING result STRING | 取回画面上方标题 |
  | setImage(image STRING ) | 设定画面图标（icon） |
  | getImage() RETURNING result STRING | 取回画面图标（icon） |

```plsql
DEFINE gwin_curr ui.Window
LET gwin_curr = ui.Window.getCurrent() #取得现行画面
CALL g_win_curr.setText("建立画面名称")

```

## CLOSE WINDOW

```plsql
MAIN
 DEFINE ls_flow_pic STRING
 OPEN WINDOW act_w WITH FORM "/u1/users/top/topmenu"
ATTRIBUTE(BORDER，CYAN)
 MENU ""
 ON ACTION add
 CALL act_a()
   :
 ON ACTION quit
 EXIT Program
 END MENU
 CLOSE WINDOW act_w
END MAIN
```

## CURRENT WINDOW

> 程式部份需要开很多视窗，且需切换不同视窗，则可以利用 CURRENT WINDOW。

```plsql
MAIN
 OPEN WINDOW w1 WITH FORM "edit"
 OPEN WINDOW w2 WITH FORM "topmenu"
 MENU "Change Windows"
 ON ACTION edit
 CURRENT WINDOW IS w1
 CALL act1_a()
 ON ACTION topmenu
 CURRENT WINDOW IS w2
 ON ACTION exit
 EXIT MENU
 END MENU
 CLOSE WINDOW w1
 CLOSE WINDOW w2
END MAIN
```

## CLEAR WINDOW

> 此指令是清除 FORM 内所有变数内容。但它不影响其他画面栏位显示状态。

```plsql
MAIN
 OPEN WINDOW w1 WITH FORM "employee"
 CLEAR WINDOW SCREEN #清除背景的 SCREEN 畫面，但不影響 w1 資料
 CLOSE WINDOW w1
END MAIN

```

## OPEN FORM

```plsql
OPEN FORM form-id FROM "file-name"
```

- form-id：定义画面的代码，为一全域变数。
- file-name：画面档经过编译后的档案名称（不含附档名），可以指定放置的相对或绝对路径。

### ui.Form 物件

- object 方法  
  | 名称 | 说明 |
  | --- | --- |
  | getNode() RETURNING result om.DomNode | 抓取 FORM 的节点 |
  | loadActionDefaults(filename STRING ) | 由指定路径读取 4ad 档 |
  | loadToolBar(filename STRING ) | 由指定路径读取 4tb 档 |
  | loadTopMenu(filename STRING ) | 由指定路径读取 4tm 档 |
  | findNode(type STRING,name STRING ) RETURNING result om.DomNode | 搜寻指定的 FORM 节点 |
  | setElementText(name STRING,text STRING ) | 指定元件的文字 |
  | setElementImage(name STRING,text STRING ) | 指定元件的图片 |
  | setElementStyle(name STRING,style STRING ) | 指定元件的风格 |
  | setElementHidden(name STRING,hide INTEGER ) | 指定元件隐藏 |
  | setFieldHidden(name STRING,hide INTEGER ) | 指定栏位隐藏 |
  | setFieldStyle(name STRING,style INTEGER ) | 指定栏位风格 |
  | ensureFieldVisible(name STRING ) | 确认栏位可否被看见 |
  | ensureElementVisible(name STRING ) | 确认元件可否被看见 |

## DISPLAY FORM

> form-name 与 OPEN FORM 的 form-name 要一致。
> 事先要做 OPEN FORM 将画面做开启的动作，透过 DISPLAY FORM 将画面显示在
> 萤幕上。

## CLEAR FORM

> 此指令是清除 FORM 内所有变数内容。但它不影响其他画面栏位显示状态。

## CLOSE FORM

> FORM 使用后，透过 CLOSE FORM 指令可将 form 关闭。

```plsql
MAIN
 OPEN WINDOW w1 WITH 11 ROWS， 63 COLUMNS #程式执行时先开启 edit form 执行
 OPEN FORM f1 FROM "edit" #开 form f1
 DISPLAY FORM f1
 CALL act1_a()
 CLOSE FORM f1
 OPEN FORM f2 FROM "combobox" #开 form f2
 DISPLAY FORM f2
 CALL act1_a()
 CLOSE FORM f2
 CLOSE WINDOW w1
END MAIN
FUNCTION act1_a()
 DEFINE a VARCHAR(50)
 INPUT a from formonly.a
END FUNCTION
```

## CLEAR

> 在目前所显示的画面上，清除指定的栏位变数内容

## DISPLAY / DISPLAY TO

```plsql
DISPLAY expression [,...]
 [ TO field-spec [,...] [ ATTRIBUTE ( display-attribute [,...] ) ] ]
```

> 显示指定的资料。若未跟上 TO，则资料显示在系统标准输出装置(背景 stdout)，
> 若加上 TO 则显示在指定的画面栏位上。

## DISPLAY BY NAME

> 变数与栏位名称一致时，可以简化为 DISPLAY BY NAME。与 DISPLAY TO 功能一致

### ATTRIBUTES

- BLACK、BLUE、CYAN、GREEN、MAGENTA、RED、WHITE、YELLOW 显示资料的颜色型态
- BOLD、DIM、NORMAL 显示资料的字型型态
- REVERSE、BLINK、UNDERLINE 显示资料的影像型态

## MESSAGE

> 显示程式运行中需要提示给操作者的讯息，显示在预设的 MESSAGE 区块

> 预设的区块在 windows style (4st) 档内设定，可以参阅 status bar types 说明进
> 行设定。

## ERROR

> 显示程式运行中的错误讯息在预设的 ERROR 区块

> 程式执行中发生的系统错误，也会呈现在此区块内，相关错误讯息可以查阅
> 手册的 error messages 章节或直接搜寻错误讯息编号。
