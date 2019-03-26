# **HW04** 廖柏翔

===

## 1. 實驗題目 

1.練習使用usermannual
2.按下藍色按鈕使藍色LED恆亮
3.使用macro function READ_BIT(addr, bit) 讀取使用者按下按鈕的狀態

## 2. 實驗步驟: 


1. 先建立`btn_init`讓按鍵其初始化
```
void btn_init(unsigned int pin)
{
	SET_BIT(RCC_BASE + RCC_AHB1ENR_OFFSET, GPIO_EN_BIT(GPIO_PORTA));
	//MODER btn pin = 00 => General purpose output mode
	CLEAR_BIT(GPIO_BASE(GPIO_PORTA) + GPIOx_MODER_OFFSET, MODERy_1_BIT(pin));
	CLEAR_BIT(GPIO_BASE(GPIO_PORTA) + GPIOx_MODER_OFFSET, MODERy_0_BIT(pin));

	//PUPDR btn pin = 10 => No pull-up, pull-down
	SET_BIT(GPIO_BASE(GPIO_PORTA) + GPIOx_PUPDR_OFFSET, PUPDRy_1_BIT(pin));
	CLEAR_BIT(GPIO_BASE(GPIO_PORTA) + GPIOx_PUPDR_OFFSET, PUPDRy_0_BIT(pin));

	//output GPIO setting
	SET_BIT(GPIO_BASE(GPIO_PORTA) + GPIOx_IDR_OFFSET,GPIOx_IDR_BIT(pin));

}


```
2. 建立`readbit`方便待會讀值
```
int readbit(unsigned int addr,unsigned int bit)
{
	return READ_BIT(addr,bit);
}
```


3. 在`blink`函式中進行初始化,等讀值。若是`readbit`返回值為1表示按下按鈕，則進行開燈動作。

```
void blink(unsigned int led)

{
	led_init(led);
	btn_init(BLUE_Btn_PA0);
	while (1)
	{	
            
            if(readbit(GPIO_BASE(GPIO_PORTA) + GPIOx_IDR_OFFSET,BLUE_Btn_PA0)==1 )
		{	
                while (1)
               {	
                    SET_BIT(GPIO_BASE(GPIO_PORTD) + GPIOx_BSRR_OFFSET, BSy_BIT(led));
		 }
	}}
}
```


reg.h:

```

//Btn
#define BLUE_Btn_PA0 0
#define READ_BIT(addr, bit) (REG(addr) & (UINT32_1 << (bit)))
#define GPIOx_IDR_OFFSET 0x10
#define GPIOx_IDR_BIT(y) (y)

```



4. 最後使用查看`make`和`make flash`進行燒入便觀察其運作 





## 3. 結果與討論

 1. 結果:按下按鈕後LED燈恆亮成功

 2. 討論:有試著寫程式，試圖讓其變成按一下亮，在按一下不亮的功能。不過由於它讀取速度很快，按下時它可能讀到好幾次。因此還沒找到方法進行除頻。或許可以在往後終做更進一步的思考
 3. 另外，覺得這次的作業學到很重要的就是看usermanual，以往沒人帶著，總覺得它是天書，感謝這次的課程與講師和助教群們。




參考資料
[Lecture 04]: http://www.nc.es.ncku.edu.tw/course/embedded/04/







--------------------




