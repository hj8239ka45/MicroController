# MicroController
課堂上的微控實驗及介面實驗  
使用 ATMega128 晶片進行實驗，運用 C 語言針對晶片記憶體位置進行設定  
OM4實驗：  
透過 PWM 不同模式進行實驗操作，其中 WGM 用來設定 使用哪一種 PWM (CTC、Fast PWM、phase correct)。  
![image](https://user-images.githubusercontent.com/39979565/229761109-6461e44a-68b3-4b94-a03f-d2998b529466.png)
透過 COM 設定是否反相計數。
![image](https://user-images.githubusercontent.com/39979565/229770003-02f6185d-a2ca-4fd3-8850-e9aa6b0915d3.png)

CTC是計數至設定之 OCR(WGMn3:0 = 4) 或 ICR(WGMn3:0 = 12) 與計數器值(TCNT) 相同時觸發，且此時計數器將重新開始計數。中斷頻率為 F_ocna = F_clk_io / ( 2 * N * (1+OCRn) )
![image](https://user-images.githubusercontent.com/39979565/229768548-614d8fd9-243e-472d-97d2-84411d8921ef.png)

 OM5實驗：
 
