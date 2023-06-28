---
title: "tiptop控制流"
date: 2022-04-14T10:02:39+08:00
lastmod: 2022-04-14T10:02:39+08:00
tags: ["tiptop控制流"]
categories: ["ERP", "TIPTOP"]
author: "darcy"
# bookComments: false
# bookSearchExclude: false
---

## IF

```plsql
IF condition THEN
   statement
   [...]
[ ELSE
   statement
   [...]
]
END IF
```

```plsql
MAIN
  DEFINE name CHAR(20)
  LET name = "John Smith"
  IF name MATCHES "John*" THEN
    DISPLAY "The name starts with [John]!"
  ELSE
    DISPLAY "The name is " || name || "."
  END IF
END MAIN
```

## CASE

```plsql
CASE expression-1
  WHEN expression-2
    { statement | EXIT CASE }
    [...]
  [ OTHERWISE
    { statement | EXIT CASE }
    [...]
  ]
END CASE
```

```plsql
CASE
  WHEN boolean-expression
    { statement | EXIT CASE }
    [...]
  [ OTHERWISE
    { statement | EXIT CASE }
    [...]
  ]
END CASE
```

```plsql
MAIN
   DEFINE v CHAR(10)
   LET v = "C1"
   -- CASE Syntax 1
   CASE v
     WHEN "C1"
       DISPLAY "Value is C1"
     WHEN "C2"
       DISPLAY "Value is C2"
     WHEN "C3"
       DISPLAY "Value is C3"
     OTHERWISE
       DISPLAY "Unexpected value"
   END CASE
   -- CASE Syntax 2
   CASE
     WHEN ( v="C1" OR v="C2" )
       DISPLAY "Value is either C1 or C2"
     WHEN ( v="C3" OR v="C4" )
       DISPLAY "Value is either C3 or C4"
     OTHERWISE
       DISPLAY "Unexpected value"
   END CASE
END MAIN
```

## FOR

```plsql
FOR counter = start TO finish [ STEP value ]
   { statement
   | EXIT FOR
   | CONTINUE FOR }
   [...]
END FOR
```

```plsql
MAIN
  DEFINE i, i_min, i_max INTEGER
  LET i_min = 1
  LET i_max = 10
  DISPLAY "Count from " || i_min || " to " || i_max
  DISPLAY "Counting forwards..."
  FOR i = i_min TO i_max
      DISPLAY i
  END FOR
  DISPLAY "... and backwards."
  FOR i = i_max TO i_min STEP -1
      DISPLAY i
  END FOR
END MAIN
```

### WHILE

```plsql
WHILE condition
    { statement | EXIT WHILE | CONTINUE WHILE }
   [...]
END WHILE
```

```plsql
MAIN
  DEFINE cnt INTEGER
  LET cnt = 1
  WHILE cnt <= 100
    DISPLAY "Iter: " || cnt
    LET cnt = cnt + 1
    IF int_flag THEN
      EXIT WHILE
    END IF
  END WHILE
END MAIN
```

## CONTINUE

```plsql
CONTINUE
 { FOR
 | FOREACH
 | WHILE
 | MENU
 | CONSTRUCT
 | INPUT
 | DIALOG
 }
```

## EXIT

```plsql
EXIT
 { CASE
 | FOR
 | FOREACH
 | WHILE
 | MENU
 | CONSTRUCT
 | REPORT
 | DISPLAY
 | INPUT
 | DIALOG }
```

## TRY CATCH

```plsql
TRY
   instruction
   [...]
CATCH
   instruction
   [...]
END TRY
```

```plsql
TRY
    TRY
        SELECT COUNT(*) INTO num_cust FROM customers
    CATCH
        ERROR "Try block 2: ", SQLCA.SQLCODE
    END TRY
CATCH
    ERROR "Try block 1: ", SQLCA.SQLCODE
END TRY
```

> 利用 WHENEVER 的报错也可以

```plsql
MAIN
    TRY
        CALL cust_report()
    CATCH
        ERROR "An error occurred during report execution: ", STATUS
    END TRY
END MAIN

FUNCTION cust_report()
    WHENEVER ERROR RAISE
    START REPORT cust_rep ...
    ...
END FUNCTION
```

## SLEEP

```plsql
SLEEP seconds
```

```plsql
MAIN
  DISPLAY "Please wait 5 seconds..."
  SLEEP 5
  DISPLAY "Thank you."
END MAIN
```

## LABEL / GOTO

```plsql
GOTO [:] label-id
LABEL label-id:
```

```plsql
MAIN
  DEFINE exit_code INTEGER
  DEFINE l_status INTEGER

  WHENEVER ANY ERROR GOTO _error
  DISPLAY 1/0
  GOTO _noerror

LABEL _error:
  LET l_status = STATUS
  DISPLAY "The error number ", l_status, " has occurred."
  DISPLAY "Description: ", err_get(l_status)
  LET exit_code = -1
  GOTO _exit

LABEL _noerror:
  LET exit_code = 0
  GOTO _exit

LABEL _exit:
  EXIT PROGRAM exit_code

END MAIN
```
