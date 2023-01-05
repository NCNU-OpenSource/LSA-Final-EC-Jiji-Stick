---
tags: LSA
---

# (Listen), Watch, Run
[TOC]



## 實作念想 
- 動機 : 
    - 我們這組有幾個對於教育學習程式有興趣的組員，
        想藉由樹梅派連結感測器做出與孩童學習相關的應用專題
- 功能 :
    - 地圖顏色偵測, 採到顏色車子會發出不同音效
        -  紅 : 音頻(988)
        -  綠 : 音頻(523)
        -  藍 : 音頻(3000)
    - 車在地圖上走動


## 硬體設備
| 設備名稱 | 圖片網址 | 來源 |
|---------|---------|-------|
| Raspberry Pi 4 | ![](https://i.imgur.com/BEZPftv.jpg =50%x) | 友情贊助(石安通學長提供) |
| mbot車 | ![](https://i.imgur.com/KK7WuRG.jpg =50%x) | 友情贊助(林宜蔓學姊提供) |
| 無緣蜂鳴器 | ![](https://i.imgur.com/flbA6mY.jpg =50%x) | moli提供|
|杜邦線n | ![](https://i.imgur.com/6GoIwi3.jpg =50%x) | 友情贊助(朋友n)|
|Arduino | ![](https://i.imgur.com/xTBTYco.jpg =50%x) | 友情提供(張中漢同學)|
|顏色感測器| ![](https://i.imgur.com/QJ597Ex.jpg =50%x) |蝦皮|

    


## 安裝設定過程
#### GPIO & 設備
![](https://i.imgur.com/cEbRA4x.png)

#### 顏色感測器 TCS34725

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

## car photo & 成果影片
影片一
https://youtube.com/shorts/ixodvznrxMY?feature=share
影片二
https://youtube.com/shorts/F6QM63x0BYs?feature=share


## Programing
### CAR (mbot用來行走)
* mbot 可以兼容 arduino
* 導入相關套件

```
// generated by mBlock5 for mBot
// codes make you happy

#include <MeMCore.h>
#include <Arduino.h>
#include <Wire.h>
#include <SoftwareSerial.h>
```

* 設定紅外線遙控與馬達

```
MeIR ir;
MeDCMotor motor_9(9);
MeDCMotor motor_10(10);
```

* 設定馬達在前後左右的方向
```
void move(int direction, int speed) {
  int leftSpeed = 0;
  int rightSpeed = 0;
  if(direction == 1) {
    leftSpeed = speed;
    rightSpeed = speed;
  } else if(direction == 2) {
    leftSpeed = -speed;
    rightSpeed = -speed;
  } else if(direction == 3) {
    leftSpeed = -speed;
    rightSpeed = speed;
  } else if(direction == 4) {
    leftSpeed = speed;
    rightSpeed = -speed;
  }
  motor_9.run((9) == M1 ? -(leftSpeed) : (leftSpeed));
  motor_10.run((10) == M1 ? -(rightSpeed) : (rightSpeed));
}

void _delay(float seconds) {
  long endTime = millis() + seconds * 1000;
  while(millis() < endTime) _loop();
}
```

* 設定遙控時，前進左右的馬達運動速度
```
void setup() {
  ir.begin();
  while(1) {
      if(ir.keyPressed(64)){

          move(2, 40 / 100.0 * 255);  //
          _delay(0.1);
          move(2, 0);

      }
      if(ir.keyPressed(25)){

          move(1, 40 / 100.0 * 255);
          _delay(0.1);
          move(1, 0);

      }
      if(ir.keyPressed(7)){

          move(3, 30 / 100.0 * 255);
          _delay(0.1);
          move(3, 0);

      }
      if(ir.keyPressed(9)){

          move(4, 30 / 100.0 * 255);
          _delay(0.1);
          move(4, 0);

      }

      _loop();
  }

}

```

* 需持續偵測紅外線遙控訊號
```
void _loop() {
  ir.loop();
}

void loop() {
  _loop();
}
```


### Color sensor & Buzzer(用於外接的arduino)
```

#include <Wire.h>                                                 //include the I2C library to communicate with the sensor
#include "Adafruit_TCS34725.h"                                    //include the sensor library


#define redpin 3                                                  //pwm output for RED anode use 1K resistor
#define greenpin 5                                                //pwm output for GREEN anode use 2K resistor
#define bluepin 6                                                 //pwm output for BLUE anode use 1K resistor
#define Buzzer 9


#define commonAnode false                                         // set to false if using a common cathode LED                     



byte gammatable[256];                                             // our RGB -> eye-recognized gamma color

                                                                  //Create an instance of the TCS34725 Sensor
Adafruit_TCS34725 tcs = Adafruit_TCS34725(TCS34725_INTEGRATIONTIME_50MS, TCS34725_GAIN_4X);

void setup() {
  Serial.begin(9600);                                             //Sart serial comms @ 9600 (you can change this)
  Serial.println("Color View Test");                              //Title info             

  if (tcs.begin()) {                                              //if the sensor starts correctly
    Serial.println("Found sensor");                               
  } else {                                                        //if the sensor starts incorrectly
    Serial.println("No TCS34725 found ... check your connections");
    while (1); // halt!
  }
  
  
  pinMode(redpin, OUTPUT);                                         //set redpin for output
  pinMode(greenpin, OUTPUT);                                       //set greenpin for output
  pinMode(bluepin, OUTPUT);                                        //set bluepin for output
  pinMode(Buzzer,OUTPUT);
                                                                  
                                                                   // it helps convert RGB colors to what humans see
  for (int i=0; i<256; i++) {
    float x = i;
    x /= 255;
    x = pow(x, 2.5);
    x *= 255;
      
    if (commonAnode) {
      gammatable[i] = 255 - x;
    } else {
      gammatable[i] = x;      
    }
                                                                   //Serial.println(gammatable[i]);
  }
  
 
}

void loop() {
  uint16_t clear, red, green, blue;                                 //declare variables for the colors

  delay(1000);                                                        // takes 50ms to read 
  
  tcs.getRawData(&red, &green, &blue, &clear);                      //read the sensor

  
  Serial.print("C:\t"); Serial.print(clear);                        //print color values
  Serial.print("\tR:\t"); Serial.print(red);
  Serial.print("\tG:\t"); Serial.print(green);
  Serial.print("\tB:\t"); Serial.print(blue);

                                                                    // Figure out some basic hex code for visualization
  uint32_t sum = clear;
  float r, g, b;
  r = red; r /= sum;
  g = green; g /= sum;
  b = blue; b /= sum;
  r *= 256; g *= 256; b *= 256;
  Serial.print("\t");
  Serial.print((int)r, HEX); Serial.print((int)g, HEX); Serial.print((int)b, HEX);
  Serial.println();

                                                                    //Serial.print((int)r ); Serial.print(" "); Serial.print((int)g);Serial.print(" ");  Serial.println((int)b );

  analogWrite(redpin, gammatable[(int)r]);                          //light red led as per calculation
  analogWrite(greenpin, gammatable[(int)g]);                        //light green led as per calculation
  analogWrite(bluepin, gammatable[(int)b]);                        //light blue led as per calculation
  
  if (red > 500 & red <= 590 ) { //red
    tone(Buzzer,988,100);
  }
  else if (green > 180 & green <= 290) { // green
    tone(Buzzer,523,300);
  }
  else if (blue > 60 & blue <= 130) { //blue
    tone(Buzzer,3000,300);
  }
  else if ((red > 900 & red <= 1100) & blue > 250 & b <=330) { //yellow
    tone(Buzzer,1047,300);
  }
}


```


## Job Assignment
|  組員 | 工作分配 |
|------|---------|
| **全部人**|**硬體組裝, 程式, 地圖** |
| 張中漢| 設備場地,車車程式, 地圖, 焊接,食物提供 |
| 簡語萱| ppt, 程式, |
| 陳思妤| 程式, github, web server |
| 陳煒姍| 程式, github, web page|
| 吳常恩| 程式|

## References
> - 顏色感測器
> [1. TCS3200顏色感測器模組(TCS230升級版)](https://www.icshop.com.tw/product-page.php?9486)
> - 樹梅派連接arduino
> https://s761111.gitbook.io/raspi-sensor/pai-arduino
## Future
嘗試將計時功能(web)與寫在樹莓派中的程式連動
![](https://i.imgur.com/zUXDjP1.png)

## thank you 
- 安通學長 : 器材提供
- 惠霖學姊 : 溫柔大姊姊
- 宜蔓學姊 : 提供mbot

## 簡報
https://www.canva.com/design/DAFWsbh7i_I/sOb-uMQBgqB8cr_T3EKsDw/edit?utm_content=DAFWsbh7i_I&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton


