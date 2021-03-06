# **電子看板項目說明**

![Alt text](https://img.shields.io/badge/Dept-Watch%20NPI%20SI-brightgreen)
![Alt text](https://img.shields.io/badge/Date-2022--02--19-brightgreen)



 

## 目錄

- Change List

- UI頁面說明

  - 總覽

  - 分頁

  - UPH

  - Downtime

  - error記錄

  - 拋料

  - 良率

- 數據上傳
  - 上傳方式
  - 數據格式
  - 上傳內容
  - 產品數據
  - 設備數據
  - 心跳檢測

- 經濟效益
  - 工作流程優化 



## Change List

- 2021/11/01
  - 上傳方式，數據上傳內容，修改為分別上傳產品數據和設備數據

- 2021/11/05 
  - 總覽，總覽頁，增加良率及拋料TOP項

- 2021/11/10 
  - error記錄， error查詢可查詢整個工站及單個治具的記錄

- 2021/11/12 

  - 總覽，總覽增加黃色Alarm狀態，刪除重測率表格;

  - 分頁，分頁數據列表內容為查詢數據庫所有字段內容;

  - 數據格式，產品數據根據有sn與無sn建立兩個數據表;

  - 數據格式，設備數據根據station_status狀態變化記錄停線或報警開始與結束時間

- 2021/11/17

  - 總覽，總覽頁刪除Idle狀態，修改標題Testing Automation Dashboard，修改紅色提示標籤為stationDown

  - 上傳內容，修改數據上傳方式，product_status和station_status用兩個接口分別傳送數據

- 2021/11/19

  - 數據格式，數據上傳關鍵字改為小寫并需要轉義 "

  - 總覽，新增7.計算細節說明，說明總覽頁數據計算邏輯

- 2021/11/20

  - 總覽，總覽頁圖例2與3數據量小於10時不顯示，數據同時顯示數據和百分比

  - 總覽，總覽頁新增鼠標停時留，不自動切換頁面的需求

  - 總覽，總覽頁圖例1增加設備灰色Offline顯示，5分鐘未接收數據后掉線

  - 總覽，總覽頁圖例1工站拆分為多模組，模組需顯示工站數量

  - 數據格式，數據上傳修改station_status狀態Downtime為stationdown，增加參數模組station_module與工站數station_num，增加代碼示例

- 2021/11/25 
  - 數據格式，數據上傳中純數字參數的內容不需要增加引號，關聯參數station_module， station_num， station_no，ct

- 2021/11/26
  - 數據格式，數據上傳增加字段，模組總數moudule_num

- 2021/11/29

  - 數據格式，刪除product_status=timeout，僅留下Pass&Fail，修改sn參數空值為NOSN，修改Page.5中yield和toss計算公式

  - 心跳檢測，數據上傳增加心跳檢測數據，減少數據量，每30秒發送數據0&1即可

  - 上傳內容，station_status狀態會在station_status狀態變化時上傳，平時不再上傳

- 2021/12/08

  - 總覽，定義Yield圖表顏色為綠色，定義圖表線條顯示百分比，確定數據篩選條件

  - UPH分頁，圖表內容均可以翻頁展示不同線體數據（不同line_no），增加篩選條件描述，增加圖表顯示描述

  - Downtime分頁，圖表可選擇顯示當天或當月停線

- 2021/12/09

  - 總覽，加入顏色優先級排序

  - 分頁，定義所有分頁左側篩選條件默認值，完善分頁數據圖形數據選擇及排序方式

- 2021/12/10

  - 增加經濟效益的流程優化描述

  - 總覽，增加對線體顯示的描述

  - 增加1.2分頁，圖表縱座標與橫座標具體數據描述

- 2021/12/13
  - 總覽，增加手動翻頁

- 2021/12/16
  - 分頁，更新左側篩選框（改site為line），line/station篩選設置默認值

- 2022/01/14
  - 心跳檢測，增加計算邏輯及數據上傳內容

- 2022/01/15
  - 分頁，左側篩選框中下拉菜單增加多級關聯

- 2022/02/17

  - 總覽，增加line_type顯示，由2條線體變為3條線體（新增BCM）

  - 數據格式修正line_type由2種增加至3種（新增BCM）

  - 分頁下拉菜單內容改變，增加CAT WIFI，合併S-OTACM&RM為SOTA Combo 

- 2022/02/19
  - 測試

- 





## UI頁面說明



### 總覽

***

![Alt text](https://github.com/Ray-gitlab/Dashboard/blob/main/img/overview.png)                           

- 總覽頁面每30秒更換線體(圖例1.1 line_no)，鼠標停留在圖例1/2/3不切換，鼠標離開后重新計時

- 具備手動翻頁功能（圖例1.1切換）

- 圖例1中4個顏色狀態為設備信息，`Offline`灰色，服務器計算結果或增加心跳檢測，30秒未收到默認為模組`Offline`，另`station_status = "stationdown"`為紅色，`alarm`為黃色，都不等則為綠色，顏色優先級紅色>黃色>綠色

- 圖例2為治具拋料TOP項，顯示1小時內拋料的最高TOP 10，數據量小於10時不參與排行

- 圖例3為良率倒數TOP項，顯示1小時內良率最低TOP 10，數據量小於10時不參與排行

- 計算細節說明

  - 圖例1顯示同一個`line_no`下的線體，圖例1.1即為`line_no`，`line_no`值前端代碼更新為固定值，2022/06之前保留M03-3FA-03、M03-3FT-03，2022/06之後根據IE線體修改為4條

  - 圖例1.2圖形數量以`module_num`數值確定，圖形需篩選`line_no， station_type， station_module`并且以station_status的狀態來顯示顏色，分子分母數據為`count(station_status = Running&Idle)/station_num`，示例7/8

  - 圖例2縱座標單位為堆疊圖數值與線條百分比，堆疊圖採用兩種顏色且顯示數值，附加折線圖顯示拋料百分比并顯示百分比，排序使用降序篩選前10，計算公式採用`COUNT(sn = "NOSN")/COUNT(*)`，即產品無sn總數除以全部有記錄的總數

  - 圖例3縱座標單位為柱狀圖數值與線條百分比，柱狀圖採用綠色且顯示數值，附件折線圖顯示良率百分比并顯示百分比，排序使用升序篩選前10，計算公式採用`(product_status = "Pass")/(sn != "NOSN")`的總數，即產品Pass總數除以測試的總數.

  - 圖例2與3橫座標顯示為`station_type+"-"+station_module+"-"+station_no+"#"`



### 分頁

***

- 分頁內容手動刷新，重點展示細節數據



#### UPH

***

![Alt text](https://github.com/Ray-gitlab/Dashboard/blob/main/img/overview.png)

- 通過左側篩選條件，搜索單工站UPH圖表(圖例1)及可導出列表(圖例2)

- 篩選條件需求`line_no，station_type，date(YY-MM-DD)，fuxture(”station_module”-“station_no”#)`，篩選條件默認顯示為空白，空白時日期選擇當天，`line_no，station`設定默認值，Fixure選擇所有`(SELECT *)`

- 篩選框增加多級關聯

  - 篩選Line，固定值（臨時為M03-4FA-03，M03-4FT-03），通過`line_no`查詢`line_type`值

    - `line_type=pre-snap`時，Station篩選欄下拉顯示QT0，QT1，TILT1，QT2，CAT WIFI

    - `line_type=post-snap`時，Station篩選欄下拉顯示ALS-CAL，FACT21，C-TAP，Grape-Hybrid，SOTA Combo，Gatekeeper

  - 篩選Station，通過`line_no&station_name`查詢所有`station_module&station_no`
    - Fixture篩選欄下拉顯示2-8#（後臺字段為`"station_module"+"-"+"station_no"+"#"`）

- 圖例1展示工站當天24H(當日8:00至第二日8:00)的UPH圖表 

  - 縱座標單位小時，可上下拖動查看每小時的數據

  - 橫座標為每小時所有`COUNT(sn != "NOSN")`機台總和

  - 圖例1.1標題展示當前篩選的`line_no[station_type(site)]`

- 圖例2為細節列表，可查看工站各治具的UPH，查詢`time，site，line_type，line_no，station_type，station_module，station_no，sn，product_status，ct`



### Downtime

***

![Alt text](https://github.com/Ray-gitlab/Dashboard/blob/main/img/overview.png) 

- 通過左側篩選條件，搜索單工站Downtime圖表(圖例1)及可導出列表(圖例2)

- 篩選條件需求`line_no，station_type，date(YY-MM-DD)，fuxture("station_module"-"station_no"#)`，篩選條件默認顯示為空白，空白時日期選擇當天，`line_no，station`設定默認值，fixure選擇所有`(SELECT *)`

- 篩選框增加多級關聯

  - 篩選Line，固定值（臨時為M03-4FA-03，M03-4FT-03），通過`line_no`查詢`line_type`值

    - line_type=pre-snap時，Station篩選欄下拉顯示QT0，QT1，TILT1，QT2，CAT WIFI

    - line_type=post-snap時，Station篩選欄下拉顯示ALS-CAL，FACT21，C-TAP，Grape-Hybrid，SOTA COMBO，GateKeeper

  - 篩選Station，通過`line_no&station_name`查詢所有station_module&station_no
    - Fixture篩選欄下拉顯示2-8#（後臺字段為`"station_module"+"-"+"station_no"+"#"`）

- 圖例1展示工站downtime圖表，柱狀圖顯示停線時間加總（單位s），附件折線圖顯示百分比（分母暫定全天(86400s)，後期考慮加入班次確定當天工作小時數）

  - 左上角增加顯示選擇，可選擇Overall（展示當月每天數據）和Detail（展示當天24H每小時數據）

  - 橫座標為小時或者天，可左右拖動查看具體每小時或每天的數據

  - 縱座標為stationdown狀態的時間加總（單位s）

  - 圖例1.1標題展示當前篩選的`line_no[station_type(site)]`

- 圖例2為細節列表，可查看該工站各治具的Downtime，查詢`start_time，end_time，site，line_type，line_no，station_type，station_module，station_no，error`



###  error記錄

***

![Alt text](https://github.com/Ray-gitlab/Dashboard/blob/main/img/overview.png) 

- 通過左側篩選條件，搜索單工站error圖表(圖例1)及可導出列表(圖例2)

- 篩選條件需求`line_no，station_type，date(YY-MM-DD)，fuxture("station_module"-"station_no"#)`，篩選條件默認顯示為空白，空白時日期選擇當天，`line_no，station`設定默認值，fixure選擇所有`(SELECT *)` 

- 篩選框增加多級關聯

  - 篩選Line，固定值（臨時為M03-4FA-03，M03-4FT-03），通過`line_no`查詢`line_type`值

    - line_type=pre-snap時，Station篩選欄下拉顯示QT0，QT1，TILT1，QT2，CAT WIFI

    - line_type=post-snap時，Station篩選欄下拉顯示ALS-CAL，FACT21，C-TAP，Grape-Hybrid，SOTA COMBO，GateKeeper

  - 篩選Station，通過`line_no&station_name`查詢所有station_module&station_no
    - Fixture篩選欄下拉顯示2-8#（後臺字段為`"station_module"+"-"+"station_no"+"#"`）

- 圖例1展示工站error報警次數排行

  - 左上角增加顯示選擇，可選擇Month（展示當月總數據）和Day（展示當天總數據）

  - 縱座標按error內容的次數降序排列，可上下拖動顯示全部數據

  - 橫座標為次數

  - 圖例1.1標題展示當前篩選的`line_no[station_type(site)]`

- 圖例2為細節列表（暫時與Downtime查詢內容同步）,可查看該工站各治具的報警信息, 查詢`start_time，end_time，site，line_type，line_no，station_type，station_module，station_no，error`

  

### 拋料

***

![Alt text](https://github.com/Ray-gitlab/Dashboard/blob/main/img/overview.png) 

- 通過左側篩選條件，搜索單工站Toss圖表(圖例1)及可導出列表(圖例2)

- 篩選條件需求`line_no，station_type，date(YY-MM-DD)，fuxture("station_module"-"station_no"#)`，篩選條件默認顯示為空白，空白時日期選擇當天，`line_no，station`設定默認值，fixure選擇所有`(SELECT *)` 

- 篩選框增加多級關聯

  - 篩選Line，固定值（臨時為M03-4FA-03，M03-4FT-03），通過`line_no`查詢`line_type`值

    - `line_type=pre-snap`時，Station篩選欄下拉顯示QT0，QT1，TILT1，QT2，CAT WIFI

    - `line_type=post-snap`時，Station篩選欄下拉顯示ALS-CAL，FACT21，C-TAP，Grape-Hybrid，SOTA Combo，Gatekeeper

  - 篩選Station，通過`line_no&station_name`查詢所有`station_module&station_no`
    - Fixture篩選欄下拉顯示2-8#（後臺字段為`"station_module"+"-"+"station_no"+"#"`）

- 圖例1展示工站Tossing圖表，數據同首頁，堆疊圖採用兩種顏色且顯示每個station_no的數值，附加折線圖顯示拋料百分比并顯示百分比 

  - 左上角增加顯示選擇，展示當天24H每小時數據或當月每天

  - 橫座標為單個工站編號（station_no），可左右拖動查看所有station_module中每個station_no的數據，橫座標`module+"station_module"+"-"+"station_no"+"#"`
  - 縱座標的數值同首頁 

  - 圖例1.1標題展示當前篩選的`line_no[station_type(site)]`

- 圖例2為細節列表（暫時與UPH查詢內容同步）,可查看工站各治具的Tossing,查詢`time，site，line_type，line_no，station_type，station_module，station_no，sn，product_status，ct`

  

### 良率

***

![Alt text](https://github.com/Ray-gitlab/Dashboard/blob/main/img/overview.png) 

- 通過左側篩選條件，搜索單工站Yield圖表(圖例1)及可導出列表(圖例2)

- 篩選條件需求`line_no，station_type，date(YY-MM-DD)，fuxture("station_module"-"station_no"#)`，篩選條件默認顯示為空白，空白時日期選擇當天，`line_no，station`設定默認值，fixure選擇所有`(SELECT *)` 

- 篩選框增加多級關聯

  - 篩選Line，固定值（臨時為M03-4FA-03，M03-4FT-03），通過`line_no`查詢`line_type`值

    - `line_type=pre-snap`時，Station篩選欄下拉顯示QT0，QT1，TILT1，QT2，CAT WIFI

    - `line_type=post-snap`時，Station篩選欄下拉顯示ALS-CAL，FACT21，C-TAP，Grape-Hybrid，SOTA Combo，Gatekeeper

  - 篩選Station，通過`line_no&station_name`查詢所有`station_module&station_no`
    - Fixture篩選欄下拉顯示2-8#（後臺字段為`"station_module"+"-"+"station_no"+"#"`）

- 圖例1展示工站Yield圖表，柱狀圖採用綠色且顯示數值，附件折線圖顯示良率百分比并顯示百分比

  - 左上角增加顯示選擇，展示當天24H每小時數據或當月每天

  - 橫座標為單個工站編號（station_no），可左右拖動查看所有station_module中每個station_no的數據，橫座標`module+"station_module"+"-"+"station_no"+"#"`

  - 縱座標數值同首頁

  - 圖例1.1標題展示當前篩選的`line_no[station_type(site)]`

- 圖例2為細節列表（暫時與UPH查詢內容同步），可查看工站各治具的Yield，查詢`time，site，line_type，line_no，station_type，station_module，station_no，sn，product_status，ct`

 

## 數據上傳



### 上傳方式

***

- 上傳方式: 數據通過URL(http://172.18.241.61:5000/接口)上傳Json字符串

- 登錄URL: http://10.244.221.61:5000



### 數據格式

***

| **關鍵字** | **示例**       | **備註**                     |                                         |
| ---------- | -------------- | ---------------------------- | --------------------------------------- |
| 時間       | time           | "time":"2021-11-09 04:31:15" | now()                                   |
| 產線類別   | site           | "site":"SM"                  | SM&BIG                                  |
| 線體類型   | line_type      | "line_type":"Pre-snap"       | Pre-snap&Post-snap，BCM                 |
| 線體編號   | line_no        | "line_no":"M3-3F-01"         | 線體名稱                                |
| 工站類型   | station_type   | "station_type":"QT0"         | 工站名稱                                |
| 工站模組   | station_module | "station_module":1           | 1~3，模組編號                           |
| 模組總數   | module_num     | "module_num":3               | 2~3，模組總數                           |
| 治具編號   | station_no     | "station_no":1               | 1~20，治具編號                          |
| 治具總數   | station_num    | "station_num":8              | 1~20，治具總數                          |
| 治具狀態   | station_status | "station_status":"Running"   | Running&Inactive&Idle&stationdown&Alarm |
| 產品sn     | sn             | "sn":"H4HF1234Q123"          | 10位或12位字符或者NOsn                  |
| 產品狀態   | product_status | "product_status":"Pass"      | Pass&Fail                               |
| 產品節拍   | ct             | "ct":150                     | 單位秒                                  |
| 異常信息   | error          | "error":"7號氣缸異常"        | error為Text格式，默認值為NULL           |
| 廠商       | vendor         | "vendor":"SR"                |                                         |
| 心跳檢測   | heartbeat      | "heartbeat":0                | 發送0，服務器反饋1                      |

 

### 上傳內容

***

- 產品數據每次更新時發送，即product_status更新狀態即發送數據

- 設備數據在station_status狀態發生改變時上傳，設備缺料持續5分鐘后再變為Idel

- heartbeat心跳檢測每30秒交互一次，30秒內未收到數據默認為設備掉線offline

  

#### 產品數據

***

- 上傳URL: http://172.18.241.61:5000/product

- 使用數據表product，insert數據

- 正常數據:

`{`

`"time":"2021-11-09 04:31:15"，`

`"site":"SM"，`

`"line_type":"Pre-snap"，`

`"line_no":" M3-3F-01"，`

`"station_type":"QT0"，`

`"station_module":1，`

`"station_no":1，`

`"sn":"H4HF1234Q123"，`

`"product_status":"Pass"，`

`//"product_status":"Fail"，`

`"ct": 102`

`}`

- 異常數據:

`{`

`"time":"2021-11-09 04:31:15"，`

`"site":"SM"，`

`"line_type":"Pre-snap"，`

`"line_no":" M3-3F-01"，`

`"station_type":"QT0"，`

`"station_module":1，`

`"station_no":1，`

`"sn":"NOsn"，`

`"product_status":"Pass"，`

`"ct": 150`

`}`

- 服務器回覆

`Msg {`

​    `OK(error)`

`}`



#### 設備數據

***

- 上傳URL: http://172.18.241.61:5000/line

- 使用數據表line，insert數據，讀取最後一次station_status用來顯示到overview頁面

- 設備正常時:

`{`       

`"time":"2021-11-09 10:31:15"，`

`"site":"SM"，`

`"line_type":"Pre-snap"，`

`"line_no":"M3-3F-01"，`

`"station_type":"QT0"，`

`"station_module":1，`

`"module_num":3，`

`"station_no":1，`

`"station_num":8`

`"station_status":"Running"，`

`//"station_status":"Idle"，`

`"error":"NULL"，`

`"vendor":"SR"`

`}`

- 設備異常時:

`{`       

`"time":"2021-11-09 10:31:15"，`

`"site":"SM"，`

`"line_type":"Pre-snap"，`

`"line_no":"M3-3F-01"，`

`"station_type":"QT0"，`

`"station_module":1，`

`"module_num":3，`

`"station_no":1，`

`"station_num":8`

`"station_status":"Alarm"，`

`//"station_status":"stationdown"，`

`//"station_status":"Inactive"，`

`"error":"7號氣缸異常"，`

`"vendor":"SR"`

`}`

- 服務器回覆:

`Msg {`

​    `OK(error)`

`}`



#### 心跳檢測

***

- 上傳URL: http://172.18.241.61:5000/heartbeat

- 新建heartbeat數據表，表格數據不需要insert，只需要update數據即可。

- 設備會每分鐘上傳一次數據，將heartbeat字段置為0

- 服務器每30s讀取heartbeat，值為0時，則賦值為1

- 服務器每120s檢查一次heartbeat，若heartbeat=1，則認為設備offline，并將station_status賦值為offline

`{`

`"time":"2021-11-09 10:31:15"，`

`"site":"SM"，`

`"line_type":"Pre-snap"，`

`"line_no":"M3-3F-01"，`

`"station_type":"QT0"，`

`"station_module":1，`

`"module_num":3，`

`"station_no":1，`

`"station_num":8`

`"heartbeat":0`

`}`

- 服務器回覆

`Msg {`

​    `"heartbeat":1`

`}`