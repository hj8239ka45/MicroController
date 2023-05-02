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
I2C(Inter-Integrated Circuit), 是一種常用的串列通訊協定，用於在元件之間——特別是兩個或兩個以上不同電路之間建立通訊。  
I2C 有利於設計人員在系統的眾多節點之間建立簡單、雙向、彈性的通訊。I2C僅使用兩條雙向線來發送和接收資訊，從而降低了複雜性。它還允許設計人員配置多個主節點系統IC之間的通訊。 I2C 對管理系統和電源的開發人員也很有利，讓他們能夠在盡可能短的時間內創建高品質的產品。  

每個I2C件有兩條線路：  
* SDA是供主元件和節點發送和接收數據的線路。  
* SCL是承載時脈訊號的線路。SCL總是由 I2C 主元件產生。規範對時脈訊號的低相位和高相位有最短週期要求。
![image](https://user-images.githubusercontent.com/39979565/235631568-9aae8126-acc3-4c55-8c6d-4b15e4ee4665.png)

I2C 匯流排僅使用兩條雙向線路：每個元件的SDA和SCL用於簡單的IC間通訊。  
I2C 訊息為：  
* 起始條件: 喚醒匯流排上的元件。SDA線從高位準切換到低位準，然後SCL線從高位準切換到低位準。
* 位址幀包含7位元或10位元序列，具體取決於可用性（參見產品手冊）。主元件將其想要與之通訊的節點位址發送到其所連接的每個節點。然後，每個節點將主元件所發送的位址與其自己的位址進行比較。如果位址匹配，它便向主元件發送一個低電壓ACK位元。如果地址不匹配，則節點什麼也不做，SDA線保持高位準。
* 位址幀的最後一位告知節點，主元件是想要將數據寫入其中還是從中接收數據。如果主元件希望將數據發送到節點，則讀⁄寫位處於低位準。如果主元件請求從節點得到數據，則該位元處於高位準。
* 消息中的每一幀後面都跟隨一個應答⁄不應答位元。如果成功接收到一個位址幀或數據幀，則接收元件會向發件者返回一個ACK位元。
![image](https://user-images.githubusercontent.com/39979565/235634796-adf107b4-905e-47ae-a324-9d04a6871aee.png)  

以本實驗為例，使用的晶片為 TMP175:
1. 首先會先進行 start 告訴通訊系統開始傳輸 I2C 通訊
2. 初始設定 Address 會根據元件接的電路以及 datasheet 來決定設定之 slave address 為何  
![image](https://user-images.githubusercontent.com/39979565/235640682-9e686e78-ecf9-4156-9915-3c6a7b73b1c8.png)
![image](https://user-images.githubusercontent.com/39979565/235641259-c8f621a5-51f2-4658-bd81-2b05d852474f.png)  
3. 傳完 address 後會選擇讀/寫模式  
![image](https://user-images.githubusercontent.com/39979565/235640124-0c4fe185-9cac-4791-8bf3-5d77dcdb2710.png)  
4. 在寫入模式下，可以設定元件的configuration，可根據資料選擇三種不同模式，分別為 one shot, compare mode, interrupt mode  
![image](https://user-images.githubusercontent.com/39979565/235640288-a4fd8d7f-8a46-4079-adda-5553c686ec1c.png)
![image](https://user-images.githubusercontent.com/39979565/235639891-602cbaa1-5a6a-4f76-88d0-f4f69d6c7245.png)  
5. 設定完成後，需傳輸 end 告訴通訊系統結束傳輸 I2C 通訊 (切換讀寫模式也需要先 end)
6. 在讀取模式下，設定完讀取模式後直接 end，根據 slave address 進行讀取，由於資料由 2 Bytes 組成:
![image](https://user-images.githubusercontent.com/39979565/235640193-e23a2f60-9fe0-432f-b9a7-91d9c7773e9f.png)


