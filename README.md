
```
name :A.Anbuselvam
reg  :212222240009
```
# EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD

## Aim: 
To Interface a 4X4 matrix keypad and show the output on 16X2 LCD display to ARM controller , and simulate it in Proteus
## Components required: 
STM32 CUBE IDE, Proteus 8 simulator .
## Theory:

<img src="https://github.com/vasanthkumarch/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/36288975/2a4a795e-1674-4329-ae07-3f5e8d5073e2" width=450 height=450>

4×4 Keypad Module Pin Diagram
 
4x4 Keypad module Pin Diagram
4×4 Keypad module Pin Diagram
Pin Number	Pin Name	Description
1	R1	Taken out from 1st  ROW
2	R2	Taken out from 2nd  ROW
3	R3	Taken out from 3rd  ROW
4	R4	Taken out from 4th  ROW
5	C1	Taken out from 1st  COLUMN
6	C2	Taken out from 2nd COLUMN
7	C3	Taken out from 3rd  COLUMN
8	C4	Taken out from 4th  COLUMN
4×4 Matrix Keypad Module Hardware Overview
These Keypad modules are made of thin, flexible membrane material. The 4 x4 keypad module consists of 16 keys, these Keys are organized in a matrix of rows and columns. All these switches are connected to each other with a conductive trace. Normally there is no connection between rows and columns. When we will press a key, then a row and a column make contact.

## Procedure : 
 ## LCD 16X2 
   16×2 LCD is named so because; it has 16 Columns and 2 Rows. There are a lot of combinations available like,
   8×1, 8×2, 10×2, 16×1, etc. But the most used one is the 16*2 LCD, hence we are using it here.

All the above mentioned LCD display will have 16 Pins and the programming approach is also the same and hence the choice is left to you. 
Below is the Pinout and Pin Description of 16x2 LCD Module:




4-bit and 8-bit Mode of LCD:

The LCD can work in two different modes, namely the 4-bit mode and the 8-bit mode. In 4 bit mode we send the data nibble by nibble, first upper nibble and then lower nibble. For those of you who don’t know what a nibble is: a nibble is a group of four bits, so the lower four bits (D0-D3) of a byte form the lower nibble while the upper four bits (D4-D7) of a byte form the higher nibble. This enables us to send 8 bit data.

Whereas in 8 bit mode we can send the 8-bit data directly in one stroke since we use all the 8 data lines.

 8-bit mode is faster and flawless than 4-bit mode. But the major drawback is that it needs 8 data lines connected to the microcontroller. This will make us run out of I/O pins on our MCU, so 4-bit mode is widely used. No control pins are used to set these modes. 
 LCD Commands:

There are some preset commands instructions in LCD, which we need to send to LCD through some microcontroller. Some important command instructions are given below:

Hex Code

Command to LCD Instruction Register

0F

LCD ON, cursor ON

01

Clear display screen

02

Return home

04

Decrement cursor (shift cursor to left)

06

Increment cursor (shift cursor to right)

05

Shift display right

07

Shift display left

0E

Display ON, cursor blinking

80

Force cursor to beginning of first line

C0

Force cursor to beginning of second line

38

2 lines and 5×7 matrix

83

Cursor line 1 position 3

3C

Activate second line

08

Display OFF, cursor OFF

C1

Jump to second line, position 1

OC

Display ON, cursor OFF

C1

Jump to second line, position 1

C2

Jump to second line, position 2
 
## Procedure:
 1. click on STM 32 CUBE IDE, the following screen will appear 



 2. click on FILE, click on new stm 32 project 


3. select the target to be programmed  as shown below and click on next 



4.select the program name 



5. corresponding ioc file will be generated automatically 

6.select the appropriate pins as gipo, in or out, USART or required options and configure 



7.click on cntrl+S , automaticall C program will be generated 

8. edit the program and as per required 

9. Add necessary library files of LCD 16x2 , write the program and use project and build  


10. once the project is bulild 

11. click on debug option 
=450>


12.  Creating Proteus project and running the simulation
We are now at the last part of step by step guide on how to simulate STM32 project in Proteus.

13. Create a new Proteus project and place STM32F40xx i.e. the same MCU for which the project was created in STM32Cube IDE. 
14. After creation of the circuit as per requirement as shown below 


14. Double click on the the MCU part to open settings. Next to the Program File option, give full path to the Hex file generated using STM32Cube IDE. Then set the external crystal frequency to 8M (i.e. 8 MHz). Click OK to save the changes.
https://engineeringxpert.com/wp-content/uploads/2022/04/26.png

15. click on debug and simulate using simulation as shown below 



## CIRCUIT DIAGRAM 
 

## STM 32 CUBE PROGRAM :
```
#include "main.h"
#include"stdbool.h"
#include"lcd.h"
bool col1,col2,col3,col4;
void key();
  while (1)
  {
	  key();
	  HAL_Delay(1000);
  }
void key(){
	  Lcd_PortType ports[]={GPIOA,GPIOA,GPIOA,GPIOA};
	  	  Lcd_PinType pins[]={GPIO_PIN_3,GPIO_PIN_2,GPIO_PIN_1,GPIO_PIN_0};
	  	  Lcd_HandleTypeDef lcd;
 lcd=Lcd_create(ports,pins,GPIOB,GPIO_PIN_0,GPIOB,GPIO_PIN_1,LCD_4_BIT_MODE);
HAL_GPIO_WritePin(GPIOC,GPIO_PIN_0,GPIO_PIN_RESET);
HAL_GPIO_WritePin(GPIOC,GPIO_PIN_1,GPIO_PIN_SET);
HAL_GPIO_WritePin(GPIOC,GPIO_PIN_2,GPIO_PIN_SET);
HAL_GPIO_WritePin(GPIOC,GPIO_PIN_3,GPIO_PIN_SET);
col1=HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_4);
col2=HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_5);
col3=HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_6);
col4=HAL_GPIO_ReadPin(GPIOC,GPIO_PIN_7);

if(!col1){
Lcd_cursor(&lcd,0,1);
Lcd_string(&lcd,"key 7 pressed");
col1=1;
}
else if(!col2){
Lcd_cursor(&lcd,0,1);
Lcd_string(&lcd,"key 8 pressed");
col2=1;
 }
 else if(!col3){
Lcd_cursor(&lcd,0,1);
Lcd_string(&lcd,"key 9 pressed");
col3=1;
}
else if(!col4){
Lcd_cursor(&lcd,0,1);
Lcd_string(&lcd,"key / pressed");
col4=1;
 }

HAL_Delay(500);
HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0, GPIO_PIN_SET);
HAL_GPIO_WritePin(GPIOC, GPIO_PIN_1, GPIO_PIN_RESET);
HAL_GPIO_WritePin(GPIOC, GPIO_PIN_2, GPIO_PIN_SET);
HAL_GPIO_WritePin(GPIOC, GPIO_PIN_3, GPIO_PIN_SET);

col1 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_4);
col2 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_5);
col3 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_6);
col4 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_7);
 if(!col1)
{
Lcd_cursor(&lcd, 0,1);
Lcd_string(&lcd, "key 4 pressed");
col1=1;
}
else if(!col2)
{
  Lcd_cursor(&lcd, 0,1);
  Lcd_string(&lcd, "key 5 pressed");
  col2=1;
 }
 else if(!col3)
 {
  Lcd_cursor(&lcd, 0,1);
  Lcd_string(&lcd, "key 6 pressed");
 col3=1;
 }
  else if(!col4)
 {
  Lcd_cursor(&lcd, 0,1);
  Lcd_string(&lcd, "key X pressed");
  col4=1;
 }
  HAL_Delay(500);
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0, GPIO_PIN_SET);
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_1, GPIO_PIN_SET);
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_2, GPIO_PIN_RESET);
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_3, GPIO_PIN_SET);
  col1 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_4);
  col2 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_5);
  col3 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_6);
  col4 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_7);
  if(!col1)
  {
  Lcd_cursor(&lcd, 0,1);
  Lcd_string(&lcd, "key 1 pressed");
  col1=1;
  }
  else if(!col2)
  {
  Lcd_cursor(&lcd, 0,1);
  Lcd_string(&lcd, "key 2 pressed");
  col2=1;
  }
 else if(!col3)
 {
  Lcd_cursor(&lcd, 0,1);
  Lcd_string(&lcd, "key 3 pressed");
  col3=1;
  }
  else if(!col4)
  {
  Lcd_cursor(&lcd, 0,1);
  Lcd_string(&lcd, "key - pressed");
  col4=1;
  }
  HAL_Delay(500);
 HAL_GPIO_WritePin(GPIOC, GPIO_PIN_0, GPIO_PIN_SET);
 HAL_GPIO_WritePin(GPIOC, GPIO_PIN_1, GPIO_PIN_SET);
 HAL_GPIO_WritePin(GPIOC, GPIO_PIN_2, GPIO_PIN_SET);
 HAL_GPIO_WritePin(GPIOC, GPIO_PIN_3, GPIO_PIN_RESET);
 col1 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_4);
 col2 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_5);
 col3 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_6);
  col4 = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_7);
 if(!col1)
 {
  Lcd_cursor(&lcd, 0,1);
  Lcd_string(&lcd, "key ON pressed");
  col1=1;
  }
 else if(!col2)
 {
  Lcd_cursor(&lcd, 0,1);
  Lcd_string(&lcd, "key 0 pressed");
  col2=1;
 }
 else if(!col3)
 {
 Lcd_cursor(&lcd, 0,1);
 Lcd_string(&lcd, "key = pressed");
  col3=1;
  }
  else if(!col4)
  {
  Lcd_cursor(&lcd, 0,1);
  Lcd_string(&lcd, "key + pressed");
  col4=1;
  }
  else{
            Lcd_cursor(&lcd,0,1);
           Lcd_string(&lcd,"no key pressed");
	  HAL_Delay(500);
  }
}
```



## Output screen shots of proteus  :
 
<img src="https://github.com/anbuselvamA/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/119559871/46f7d440-208d-413b-adc1-074c98aeffbd" width=450 height=450>
<img src="https://github.com/anbuselvamA/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/119559871/239c1c51-0478-4211-9e57-5f9a0b9aa61d" width=450 height=450>

 ## CIRCUIT DIAGRAM (EXPORT THE GRAPHICS TO PDF AND ADD THE SCREEN SHOT HERE): 
 <img src="https://github.com/anbuselvamA/EXPERIMENT--05-INTERFACING-A-4X4-MATRIX-KEYPAD-AND-DISPLAY-THE-OUTPUT-ON-LCD/assets/119559871/8bfda57a-5b1e-4ab0-8526-4a3f62739789" width=450 height=450>

 
## Result :
Interfacing a 4x4 keypad with ARM microcontroller are simulated in proteus and the results are verified.
