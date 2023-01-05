---
tags: LSA
---

# (Listen), Watch, Run
[TOC]



## 實作念想 
- 動機 : 
    - 我們是看到之前有賽車比賽為了讓選手幫車手加油，所以他們改裝車子能讓聲音變成驅動汽車，那我們根據這樣的方式想說我們也做車子用聲音去取代引擎去驅動，比賽方式為一人進行一次，紀錄成績，再換下一人
- 功能 :
    - 地圖顏色偵測, 採到顏色車子會發出不同音效
        -  紅 : 音頻(988)
        -  綠 : 音頻(523)
        -  藍 : 音頻(3000)
    - 車在地圖上走動


## 硬體設備
| 設備名稱 | 圖片網址 | 來源 |
|---------|---------|-------|
| Raspberry Pi 4 |![](https://i.imgur.com/BEZPftv.jpg =50%x)| 友情贊助(石安通學長提供) |
| mbot車 |![](https://i.imgur.com/KK7WuRG.jpg =50%x)| 友情贊助(林宜蔓學姊提供) |
| 無緣蜂鳴器 |![](https://i.imgur.com/flbA6mY.jpg =50%x)| moli提供|
|杜邦線n |![](https://i.imgur.com/6GoIwi3.jpg =50%x)| 友情贊助(朋友n)|
|Arduino |![](https://i.imgur.com/xTBTYco.jpg =50%x)| 友情提供(張中漢同學)|
|顏色感測器|![](https://i.imgur.com/QJ597Ex.jpg =50%x)|蝦皮|

    


## 安裝設定過程
#### GPIO & 設備
![](https://i.imgur.com/cEbRA4x.png)

#### 顏色感測器
|顏色感測器接口|arduino接口 |
|------|------|
|VIN|5V|
|GND|GND|
|SCL|A5|
|SDA|A4|

#### 蜂鳴器
|蜂鳴器接口|arduino接口 |
|------|------|
|S1|D9|
|V|GND|
|G|5V|

## car car photo & 成果影片

## Job Assignment
|  組員 | 工作分配 |
|------|---------|
| 張中漢| 設備場地食物提供, 車車, 地圖, 焊接|
| 簡語萱| ppt, 程式, |
| 陳思妤| 程式, github, web server |
| 陳煒姍| 程式, github, web page|
| 吳常恩| 程式|
| 全部人|硬體組裝, 程式, 地圖 |

## References
> - 顏色感測器
> [1. TCS3200顏色感測器模組(TCS230升級版)](https://www.icshop.com.tw/product-page.php?9486)
> - 樹梅派連接arduino
> https://s761111.gitbook.io/raspi-sensor/pai-arduino
## Future
嘗試將計時功能(web)與寫在樹莓派中的程式連動
![](https://i.imgur.com/zUXDjP1.png)

## thank you kind good
- 安通學長 : 器材提供
- 惠霖學姊 : 溫柔大姊姊
- 宜蔓學姊 : 提供mbot

## 簡報
https://www.canva.com/design/DAFWsbh7i_I/sOb-uMQBgqB8cr_T3EKsDw/edit?utm_content=DAFWsbh7i_I&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton


