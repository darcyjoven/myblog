---
title: "环境变量"
date: 2022-04-14T09:49:56+08:00
lastmod: 2022-04-14T09:49:56+08:00
draft: false
tags: ["环境变量"]
categories: ["ERP", "TIPTOP"]
# bookComments: false
# bookSearchExclude: false
---



| 变量名称        | 变量功能                                                 |
| --------------- | -------------------------------------------------------- |
| ORACLE_HOME     | 【ORACLE】指向存放 ORACLE 系統的目錄                     |
| ORACLE_SID      | 【ORACLE】ORACLE 資料庫的名稱                            |
| NLS_LANG        | 【ORACLE】正常存取資料的語系設定                         |
| DBPATH          | 【均有】程式執行所使用的畫面搜尋根目錄路徑               |
| DBDATE          | 【均有】指定日期顯示的格式                               |
| DBDELIMITER     | 【均有】指定資料在上／下載時的欄位分隔碼                 |
| Genero          | 需用環境變數                                             |
| FGLDIR          | 設定編譯器存放的路徑                                     |
| FGLSERVER       | 本次對應客戶端 IP 位置(GDC 執行路徑，可接上『埠編號』)   |
| FGLDBPATH       | 存放資料庫資料表結構的路徑                               |
| FGLRESOURCEPATH | 執行過程中需要的 42s 多語言檔案存放路徑                  |
| FGLIMAGEPATH    | 執行過程中系統預設抓取圖片檔案路徑                       |
| FGLLDPATH       | 程式鏈結 (link) 以及程式執行時，42m 檔案的搜尋路徑。     |
| FGLPROFILE      | 存放畫面檔相關的設定檔路徑                               |
| LANG            | UNIX 系統與 Genero 系統運行語系                          |
| LC_CTYPE        | 系統提示錯誤時，應顯示的錯誤訊息語系(可和 LANG 設定不同) |
| DBPRINT         | 要使用 Local Printer List 功能必須設定此環境變數。       |
| TOP             | 設定安裝 T100 的最上層路徑                               |
| ERP/COM/FILE…   | 第一層目錄均設置對應的環境變數，以方便操作               |
| AXX/LIB/SUB…    | 各模組均設置對應環境變數，以方便操作                     |
| AXXi            | 各 ERP 模組的 42r 路徑均設置對應環境變數，以方便操作     |
| FGLASDIR        | 設定 Web Server 路徑                                     |
| FGLASIP         | 設定要帶出 Web Server 資料時需叫用的主機資訊             |
| PATH4RP         | 報表主機上的 GR 樣板檔路(設置於 AP 主機上)               |
| MNT4RP          | AP 主機連線至報表主機上的 GR 樣板檔資料夾(PATH4RP)路徑   |
| XTRAGRIDIP      | 查詢報表的站台網址                                       |
| WINGREDIR       | 報表主機安裝 GRE 的目錄                                  |
| WINGREPORT      | GRE 使用的 port 號                                       |
