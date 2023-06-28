---
title: "TIPTOP运行时效能分析"
date: 2021-12-20T16:36:14+08:00
tags: ["TIPTOP","GP","T100"]
categories: ["4GL"]
# bookComments: false
# bookSearchExclude: false
contentCopyright: '<a rel="license noopener" href="https://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" target="_blank">Creative Commons Attribution-ShareAlike License</a>'
---

## DEBUG过程中记录日志，分析效能办法


> TOGP GP与T100 自带效能工具`r.r2d+`/`r.h`，只能直接运行作业，如果有特殊情况只能debug运行作业。可用先产生日志，再对日志进行分析的办法。
>

可参考下面步骤

### 1. 开启日志
运行下面两个命令，开启日志开启后，运行命令会将每个连接数据库语句执行状况在后台显示出来。

`FGLSQLDEBUG=9`

`export FGLSQLDEBUG`
export FGLSQLDEBUG=9 
 


### 2. debug 将日志保存下来

以aapt110为例，运行命令`r.d/r.d2+ aapt110 >> ./aapt110.lixwz.log 2>&1`，即可将运行作业中的日志保存到文件中，文件名和目录都可自定义。**记得事后删除**，如果是批处理或者报表日志可能占用空间很大。



### 3. 对产生的日志进行效能分析

#### T100
T100 使用命令

`$FGLRUN $UTL/fbin/42m/T100SQLDebug.42r ./aapt110.lixwz.log`

GP 使用命令

`$FGLRUN $DS4GL/bin/fgldebug.42r  ./aapt110.lixwz.log `
> GP 目录下若没有 `fgldebug`相关文件，需要先将效能工具相关文件上传到此目录，并运行`chmod 777 fgldebug*` 给与运行权限。


运行上述命令后即可打开效能工具


### 4. 关闭日志
运行命令`unset FGLSQLDEBUG` 关闭日志

