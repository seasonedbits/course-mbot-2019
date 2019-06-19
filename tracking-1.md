[//]: # "slide Markdown for remark"

class: img-100

# 循跡模塊

.row[
.col-6[
![](./images/tracking/sensor.png)
]
.col-6[
![](./images/tracking/sensor1.jpg)  
![](./images/tracking/sensor2.jpg)
]
]

---

# 循跡模塊

.row[
.col-6[

- 紅外感應器模塊是由一對紅外管組成的，一個發出紅外線一個負責接收
- 根據不同顏色的物體對光線的反射能力不同，接收端會輸出不同電平

]
.col-6.img-100[
![](./images/tracking/sensor_illustrated.png)
]
]

- 反射光線較少 (檢測到黑線)：  
  循跡模塊相應的指示燈**亮**，端口電平為**LOW (0)**
- 反射光線較多 (未檢測到黑線)：  
  循跡模塊相應的指示燈**滅**，端口電平為**HIGH (1)**

---

class: img-50

| 左感應器 | 右感應器 |                                     | 動作  |
| :------: | :------: | :---------------------------------: | :---: |
|    黑    |    黑    | ![](./images/tracking/status_0.png) |       |
|    黑    |    白    | ![](./images/tracking/status_1.png) |       |
|    白    |    黑    | ![](./images/tracking/status_2.png) |       |
|    白    |    白    | ![](./images/tracking/status_3.png) |       |

---

class: center, middle

# mBlock 編程

---

# 循跡模塊回傳

| 左感應器 | 右感應器 | 十進制 | 二進制 |
| :------: | :------: | :----: | :----: |
|    黑    |    黑    |  `0`   | `0b00` |
|    黑    |    白    |  `1`   | `0b01` |
|    白    |    黑    |  `2`   | `0b10` |
|    白    |    白    |  `3`   | `0b11` |

.center[
![](./images/tracking/say_tracking.png)
]

.footnote[
.row[
.col-5[
`0b`: 二進制 (Binary) 前綴  
`0o`: 八進制 (Octal) 前綴
]
.col-6[
`0x`: 十六進制 (Hexadecimal) 前綴
]
]
]

---

class: img-100

# 在 mBlock 使用二進制

.row[
.col-7[
![](./images/tracking/program_binary.png)
]
.col-5[
.blockquote.danger[

.title[離線模式限定]

`0b`前綴只在離線模式生效

]

- 用二進制可以直觀的看到循跡狀態
- 不使用馬達的離線程序不用拔 USB 線
  ]
  ]

.footnote[
[binary.sb2](./programs/tracking/binary.sb2)  
]

---

class: img-100

# 邏輯運算積木

.row[
.col-6[
![](./images/basics/if_vs_ifelse.png)
]
.col-6[
左邊三個 if 裏面的積木（區塊）都可能被執行  
右邊三個區塊只可以有一個被執行
]
]
.row[
.col-6[
![](./images/basics/if_and.png)
]
.col-6[
右邊把兩個 if 的條件用"且"積木合併起來
]
]

---

# 基本循跡

- 前進到黑線停止
- 走曲線
- 簡單地圖

.blockquote.warning[

.title[小貼士]

我們所有用馬達的程序一開始都要等待板載按鈕按下，避免 mBot 一開機就發狂亂跑。

.center[
![](./images/basics/wait_button.png)
]

]

---

# 轉向

.center.img-100[
![](./images/basics/differential_drive.png)
]

.row[
.col-6.center[
![](./images/basics/differential_left.png)
差速左轉
]
.col-6.center[
![](./images/basics/spin_left.png)
原地左轉
]
]

---

.blockquote.warning[

.title[小貼士：循跡程序]

- 做判別時要把循跡狀態先存起來，mBot 一直在動，每次感應器回傳可能會不一樣的
- 前進和轉向速度不要相差太遠，否則 mBot 會走得搖搖晃晃
- 應該用嵌套的"如果 否則 "，因為每個區塊處理一種循跡狀態
- 寫了第一個區塊後可以用右鍵，複製貼到第二個區塊
- 直接用`binary.sb2` 來開始吧

.title[小貼士：可調整的項目]

- 前行速度
- 轉向速度
- 差速左轉 vs 原地左轉
- 轉向後加"等待"_（結果是？）_
- "不停重複"最尾加"等待"_（跟上一項一樣嗎？）_
- 一直轉向直到找到黑線

]

---

# 記錄狀態

- 前進到第三條黑線停止，並*停止程序*
- 我們需要新增變數來記錄經過的黑線數目

.blockquote.success[

.title[你來試試吧]

限時十分鐘

]

---

# 停止程序

.center[
![](./images/basics/do_while.png)

如果程序要按某些條件停止，我們會用  
"直到　前都不斷重複"積木
]

---

# 重跑程序

.center[
![](./images/basics/loop_main.png)

如果我們要重跑程序，可以在外層再加一個"不停重覆"積木  
注意每次循環開始還是會先等待板載按鈕按下
]

---

class: center, middle

# 角度大怎麽辦？

---

# 大角度處理（左轉）

- 循跡模塊會碰到白色（`0b11`）
- 往左轉直至重新找到黑線

.blockquote.warning[

.title[小貼士]

- 碰到白色可以稍為繼續前行一點再轉向*（怎樣做？）*

]

---

class: center, middle

# 虛線怎麽辦？

---

# 虛線處理

- 在有虛線的地圖，不能一碰到白色就觸發大角度處理
- 碰到白色時記下當前的計時器，不作操作  
  計時器計的是千份之一秒
- 碰到黑線就把記錄歸零
- 如記錄不是零（一直在白色上）  
  而且當前計時器比記錄超過指定值（例如`x`秒）  
  那我們真的跑出賽道了，這樣才觸發大角度處理

- 超時值是怎樣定的？  
  mBot 要在超時值內走過虛線，讓記錄歸零，  
  否則會誤觸發大角度處理  
  但亦*不能太大*，否則在大角度時會跑得太遠

---

class: img-100

# 指令積木

- 我們可以自定義指令積木（函數），把大角度處理區塊放到裡面，方便重用

.row[
.col-6.center[
![](./images/basics/function1.png)
]
.col-6.center[
![](./images/basics/function2.png)
]
]

---

# 大角度處理（通用）

- 記下當前的計時器
- 往**左轉**直至：
  - 重新找到黑線 **或**
  - 當前計時器比記錄超過指定值（例如`x`秒）
- 如果超時都沒有在左邊找到黑線，就往**右轉**直至：
  - 重新找到黑線 **或**
  - 當前計時器比記錄超過指定值（例如`2x`秒）  
    `2x`是先用`x`秒回中，再往右轉`x`秒
--
- 如果超時都沒有在右邊找到黑線．．．_我們迷路了_


- 超時值是怎樣定的？  
  超時值太小會找不到黑線，大則要找右邊黑線會很慢  
  最大是令 mBot 轉＜ 180° 就夠了

---

# 參考資料

- [Binary numbers on Scratch](https://scratch.mit.edu/projects/29447030/)
- [Decimal-hexadecimal-binary conversion table](https://kb.iu.edu/d/afdl)
- [How binary digits work - CS Unplugged](https://csunplugged.org/en/topics/binary-numbers/unit-plan/how-binary-digits-work/)

* [mBot 與 STEM 的教學: mBot 入門教學：mBot 循跡車](https://mbotandstem.blogspot.com/2017/04/mbot-line-follow-car.html)
* [mBot 教學(8) - 巡線控制應用](https://www.slideshare.net/sshiouwu/mbot8-87982223)
