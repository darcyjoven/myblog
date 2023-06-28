---
title: "u1磁盘空间不足导致复制账套失败 解决办法"
date: 2021-12-20T17:05:09+08:00
tags: ["TIPTOP","GP","T100"]
categories: ["linux"]
# bookComments: false
# bookSearchExclude: false
contentCopyright: '<a rel="license noopener" href="https://en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License" target="_blank">Creative Commons Attribution-ShareAlike License</a>'
---

## u1磁盘空间不足导致复制账套失败



情况适用于以下情况：



  复制账套时，报错

  EXP-00002: error in writing to export file

  最后导出失败



## 如何判断是否是磁盘空间不足的2种方法



1.重新运行一遍aooi931建立sch。

若提示修改FGLPROFILE,直接vi $FGLPROFILE 删除掉复制的新账套信息。一般是最后几行。

再次生成sch。

运行过程中观察磁盘空间，df -h，如果/u1磁盘空间一直增长到100%，即可判断是磁盘空间不足。



2.查看复制的来源账套的最近一次备份，解压之后的dmp文件如果大于u1剩余可用空间，就可判断磁盘空间不足



## 修改createsch脚本



createsch 默认目录 

/u1/topprod//tiptop/ora/bin/createsch



将exp和imp语句中的文件位置修改为磁盘空间足够的目录

复制账套只用到第四条命令，可参考文件createsch 备注为lixwz

```shell
exp $source/$ans5@$ORACLE_SID owner=${source} file=/u2/topprod/tiptop/tmp/${source}.dmp log=$TOP/tmp/exp_${source}.log

imp system/$ans2@$ORACLE_SID fromuser=$source touser=$1 file=/u2/topprod/tiptop/tmp/${source}.dmp log=$TOP/tmp/imp_$1.log  

rm -f /u2/topprod/tiptop/tmp/${source}.dmp
```



上传完文件，注意文件的权限，用chmod修改