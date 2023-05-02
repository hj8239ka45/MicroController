MicroController  
====
課堂上的微控實驗及介面實驗，使用 ATMega128 晶片進行實驗，運用 C 語言針對晶片記憶體位置進行設定 
  
OM4實驗：
------- 
透過 PWM 不同模式進行實驗操作，其中 WGM 用來設定 使用哪一種 PWM (CTC、Fast PWM、phase correct)。  
![image](https://user-images.githubusercontent.com/39979565/229761109-6461e44a-68b3-4b94-a03f-d2998b529466.png)
透過 COM 設定是否反相計數。  
![image](https://user-images.githubusercontent.com/39979565/229770003-02f6185d-a2ca-4fd3-8850-e9aa6b0915d3.png)

CTC：  
計數至設定之 OCR(WGMn3:0 = 4) 或 ICR(WGMn3:0 = 12) 與計數器值(TCNT) 相同時觸發，且此時計數器將重新開始計數。中斷頻率為 F_ocna = F_clk_io / ( 2 * N * (1+OCRn) )  
![image](https://user-images.githubusercontent.com/39979565/229768548-614d8fd9-243e-472d-97d2-84411d8921ef.png)  
  
OM5實驗：
------- 
SPI(Serial Peripheral Interface) 通訊，用以連接 ADC, DAC, EEPROM, 通訊傳輸 IC...等週邊晶片。由於具備有低接腳數, 結構單純, 傳輸速度快, 簡單易用...等特性, 目前已經成為業界慣用標準: 不只是單晶片微控制器上有, 許多新的 SoC 晶片直接就支援多組 SPI 介面, 甚至普及到連模組化的產品 (如: 手機用的 LCD 模組 (SDI 介面), 相機模組) 及 3C 產品 (如: 數位相機用的記憶卡) 也都是使用 SPI 介面。  
SPI 為一主從式架構, 通常有一個 Master (主設備) 和一個 (或多個) Slave (從設備). 介接方法及內部硬體結構很簡單, 如下面的示意圖:  
![image](https://user-images.githubusercontent.com/39979565/235602906-a99668fe-c94f-4cad-86ac-3d9e202261fd.png)  
介面有 4 個接腳:  
![image](https://user-images.githubusercontent.com/39979565/235604320-119baa3c-86ba-4522-95e0-787e6ecffb02.png)  
在介接方面, 除了簡單的一主一從架構之外, SPI 也可以一個 master 連接多個 slave。透過給不同 ss 訊號來決定對哪一個晶片進行輸出。  
SPI 工作模式及時序圖:  
使用 SPI, 最麻煩一件事是確認周邊晶片的工作模式. SPI 一共有 4 種工作模式  
![image](https://user-images.githubusercontent.com/39979565/235606499-bf83d1bd-9191-4992-9b2f-2e8fdc08fad4.png)  
實驗五先設定好 SPI 進行傳輸的各個腳位設定，使用Port F於傳送資料時進行 /ss 腳位觸發，透過對 Max7219 點矩陣輪循進行傳送矩陣資料。  
傳送資料流程為: 1.致能晶片 2.傳送希望改變的暫存器位置編號 3.希望設定的資料 4.禁能晶片  
![image](https://user-images.githubusercontent.com/39979565/235618388-ce3d5c13-bc1c-4c96-8eeb-598b80d3bcaa.png)  

OM6實驗：
------- 

