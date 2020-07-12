# STD_BUTTON_CONTROL
This project is button control and used Std Library with Stm32f407-Discovery Kit.
//CODES

#include "stm32f4xx.h"
#include "stm32f4_discovery.h"

GPIO_InitTypeDef GPIO_InitStruct;

void GPIO_Config(){
	RCC_AHB1PeriphClockCmd(RCC_AHB1Periph_GPIOA, ENABLE); //BUTTON HATTI AKTIF EDILDI
	RCC_AHB1PeriphClockCmd(RCC_AHB1Periph_GPIOD, ENABLE); //LED HATTI AKTIF EDILDI


	//BUTTON AYARI
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_IN; //GPIO_InitStruct.GPIO_Mode = 0x00
	GPIO_InitStruct.GPIO_OType = GPIO_OType_PP;
	GPIO_InitStruct.GPIO_Pin = GPIO_Pin_0;
	GPIO_InitStruct.GPIO_PuPd = GPIO_PuPd_DOWN; //Board uzerindeki mavi button pull Down baglidir
	GPIO_InitStruct.GPIO_Speed = GPIO_Speed_100MHz;

	GPIO_Init(GPIOA, &GPIO_InitStruct);

	//LED AYARI
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_OUT;
	GPIO_InitStruct.GPIO_Pin = GPIO_Pin_12 | GPIO_Pin_13 | GPIO_Pin_14 | GPIO_Pin_15;
	GPIO_InitStruct.GPIO_OType = GPIO_OType_PP; //Open Drain yapılırsa calısmaz. sadece veri almak icindir
	GPIO_InitStruct.GPIO_PuPd = GPIO_PuPd_DOWN;
	GPIO_InitStruct.GPIO_Speed = GPIO_Speed_100MHz;

	GPIO_Init(GPIOD, &GPIO_InitStruct);

}

void delay(int time){
	while(time--);
}

int main(void)
{

	GPIO_Config();

  while (1)
  {
	  if(GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_0))//Butona basılma durumu kontrol ediliyor  GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_0)==1
	  {
		  while(GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_0)); //buton hala basılıysa bekle     GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_0)==1
		  delay(336000); //butona basıldigi an da bir olusur. arki onlmek icin bir gecikme yaplidi. Ark süresi ort 100ms dir. Bu komutta ark ların kesin bitme süresi beklenip 200 ms beklenilmistir
		  GPIO_ToggleBits(GPIOD,GPIO_Pin_12 | GPIO_Pin_13 | GPIO_Pin_14 | GPIO_Pin_15);
	  }
  }
}
