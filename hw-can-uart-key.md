---
title: "CAN数据转发"
description: "转串口/CAN"
date: 2020-05-15T23:08:08+08:00
---
完结篇
===
《[CAN转COM或CAN-1](https://blog.csdn.net/xiaobin_HLJ80/article/details/44890423)》介绍了CAN，本篇其余部分。

- 开发板：英蓓特MCB2300（LPC2368）

- 开发工具：MDK3.8a

- 开发主机：Windows XP SP3 中文专业版

# UART
因为我们使用UART主要是进行发送的，所以关闭接收可以更加快速的进行发送处理。

控制接收缓存寄存（Receiver Buffer Register）即可达到此目的。

## 禁止RBR
```c
void preUARTInitRBR( unsigned char i )
{
    switch (i) {
		case 0:
			U0IER = IER_THRE | IER_RLS;			/* Disable RBR */
	  		break;

		case 1:
			U1IER = IER_THRE | IER_RLS;			/* Disable RBR */
			break;
	}   
}
```

## 允许RBR
```c
void postUARTInitRBR( unsigned char i )
{
    switch (i) {
	  	case 0:
			U0IER = IER_THRE | IER_RLS | IER_RBR;	/* Re-enable RBR */
	  	  	break;

		case 1:
			U1IER = IER_THRE | IER_RLS | IER_RBR;	/* Re-enable RBR */
	  		break;
	}  
}
```

## 完成后的UART
> NXP的官方例程UART加入RBR后的code

uart.c
```c
/*****************************************************************************
 *   uart.c:  UART API file for NXP LPC23xx/24xx Family Microprocessors
 *
 *   Copyright(C) 2006, NXP Semiconductor
 *   All rights reserved.
 *
 *   History
 *   2006.07.12  ver 1.00    Prelimnary version, first Release
 *
******************************************************************************/
#include "LPC23xx.h"                        /* LPC23xx/24xx definitions */
#include "../common/type.h"
#include "../common/target.h"
#include "../common/irq.h"
#include "uart.h"

volatile DWORD UART0Status, UART1Status;
volatile BYTE UART0TxEmpty = 1, UART1TxEmpty = 1;
volatile BYTE UART0Buffer[BUFSIZE], UART1Buffer[BUFSIZE];
volatile DWORD UART0Count = 0, UART1Count = 0x40;

/*****************************************************************************
** Function name:		UART0Handler
**
** Descriptions:		UART0 interrupt handler
**
** parameters:			None
** Returned value:		None
**
*****************************************************************************/
void UART0Handler (void) __irq
{
  BYTE IIRValue, LSRValue;
  BYTE Dummy = Dummy;

  IENABLE;				/* handles nested interrupt */
  IIRValue = U0IIR;

  IIRValue >>= 1;			/* skip pending bit in IIR */
  IIRValue &= 0x07;			/* check bit 1~3, interrupt identification */
  if ( IIRValue == IIR_RLS )		/* Receive Line Status */
  {
	LSRValue = U0LSR;
	/* Receive Line Status */
	if ( LSRValue & (LSR_OE|LSR_PE|LSR_FE|LSR_RXFE|LSR_BI) )
	{
	  /* There are errors or break interrupt */
	  /* Read LSR will clear the interrupt */
	  UART0Status = LSRValue;
	  Dummy = U0RBR;		/* Dummy read on RX to clear
							interrupt, then bail out */
	  IDISABLE;
	  VICVectAddr = 0;		/* Acknowledge Interrupt */
	  return;
	}
	if ( LSRValue & LSR_RDR )	/* Receive Data Ready */			
	{
	  /* If no error on RLS, normal ready, save into the data buffer. */
	  /* Note: read RBR will clear the interrupt */
	  UART0Buffer[UART0Count] = U0RBR;
	  UART0Count++;
	  if ( UART0Count == BUFSIZE )
	  {
		UART0Count = 0;		/* buffer overflow */
	  }
	}
  }
  else if ( IIRValue == IIR_RDA )	/* Receive Data Available */
  {
	/* Receive Data Available */
	UART0Buffer[UART0Count] = U0RBR;
	UART0Count++;
	if ( UART0Count == BUFSIZE )
	{
	  UART0Count = 0;		/* buffer overflow */
	}
  }
  else if ( IIRValue == IIR_CTI )	/* Character timeout indicator */
  {
	/* Character Time-out indicator */
	UART0Status |= 0x100;		/* Bit 9 as the CTI error */
  }
  else if ( IIRValue == IIR_THRE )	/* THRE, transmit holding register empty */
  {
	/* THRE interrupt */
	LSRValue = U0LSR;		/* Check status in the LSR to see if
							valid data in U0THR or not */
	if ( LSRValue & LSR_THRE )
	{
	  UART0TxEmpty = 1;
	}
	else
	{
	  UART0TxEmpty = 0;
	}
  }

  IDISABLE;
  VICVectAddr = 0;		/* Acknowledge Interrupt */
}

/*****************************************************************************
** Function name:		UART1Handler
**
** Descriptions:		UART1 interrupt handler
**
** parameters:			None
** Returned value:		None
**
*****************************************************************************/
void UART1Handler (void) __irq
{
  BYTE IIRValue, LSRValue;
  BYTE Dummy = Dummy;

  IENABLE;				/* handles nested interrupt */
  IIRValue = U1IIR;

  IIRValue >>= 1;			/* skip pending bit in IIR */
  IIRValue &= 0x07;			/* check bit 1~3, interrupt identification */
  if ( IIRValue == IIR_RLS )		/* Receive Line Status */
  {
	LSRValue = U1LSR;
	/* Receive Line Status */
	if ( LSRValue & (LSR_OE|LSR_PE|LSR_FE|LSR_RXFE|LSR_BI) )
	{
	  /* There are errors or break interrupt */
	  /* Read LSR will clear the interrupt */
	  UART1Status = LSRValue;
	  Dummy = U1RBR;		/* Dummy read on RX to clear
							interrupt, then bail out */
	  IDISABLE;
	  VICVectAddr = 0;		/* Acknowledge Interrupt */
	  return;
	}
	if ( LSRValue & LSR_RDR )	/* Receive Data Ready */			
	{
	  /* If no error on RLS, normal ready, save into the data buffer. */
	  /* Note: read RBR will clear the interrupt */
	  UART1Buffer[UART1Count] = U1RBR;
	  UART1Count++;
	  if ( UART1Count == BUFSIZE )
	  {
		UART1Count = 0;		/* buffer overflow */
	  }
	}
  }
  else if ( IIRValue == IIR_RDA )	/* Receive Data Available */
  {
	/* Receive Data Available */
	UART1Buffer[UART1Count] = U1RBR;
	UART1Count++;
	if ( UART1Count == BUFSIZE )
	{
	  UART1Count = 0;		/* buffer overflow */
	}
  }
  else if ( IIRValue == IIR_CTI )	/* Character timeout indicator */
  {
	/* Character Time-out indicator */
	UART1Status |= 0x100;		/* Bit 9 as the CTI error */
  }
  else if ( IIRValue == IIR_THRE )	/* THRE, transmit holding register empty */
  {
	/* THRE interrupt */
	LSRValue = U1LSR;		/* Check status in the LSR to see if
								valid data in U0THR or not */
	if ( LSRValue & LSR_THRE )
	{
	  UART1TxEmpty = 1;
	}
	else
	{
	  UART1TxEmpty = 0;
	}
  }

  IDISABLE;
  VICVectAddr = 0;		/* Acknowledge Interrupt */
}

/*****************************************************************************
** Function name:		UARTInit
**
** Descriptions:		Initialize UART0 port, setup pin select,
**						clock, parity, stop bits, FIFO, etc.
**
** parameters:			portNum(0 or 1) and UART baudrate
** Returned value:		true or false, return false only if the
**						interrupt handler can't be installed to the
**						VIC table
**
*****************************************************************************/
DWORD UARTInit( DWORD PortNum, DWORD baudrate )
{
  DWORD Fdiv;

  if ( PortNum == 0 )
  {
	PINSEL0 = 0x00000050;       /* RxD0 and TxD0 */

    U0LCR = 0x83;		/* 8 bits, no Parity, 1 Stop bit */
    Fdiv = ( Fpclk / 16 ) / baudrate ;	/*baud rate */
    U0DLM = Fdiv / 256;							
    U0DLL = Fdiv % 256;
	U0LCR = 0x03;		/* DLAB = 0 */
    U0FCR = 0x07;		/* Enable and reset TX and RX FIFO. */

    if ( install_irq( UART0_INT, (void *)UART0Handler, HIGHEST_PRIORITY ) == FALSE )
    {
	  return (FALSE);
    }

    U0IER = IER_RBR | IER_THRE | IER_RLS;	/* Enable UART0 interrupt */
    return (TRUE);
  }
  else if ( PortNum == 1 )
  {
#if EA_BOARD_LPC24XX
	PINSEL7 |= 0x0000000F;	/* P3.16 TXD1, P3.17 RXD1 */
#else						/* Default is Keil MCB2300 board */							
	PINSEL0 |= 0x40000000;	/* Enable TxD1 P0.15 */
	PINSEL1 |= 0x00000001;	/* Enable RxD1 P0.16 */
#endif
    U1LCR = 0x83;		/* 8 bits, no Parity, 1 Stop bit */
    Fdiv = ( Fpclk / 16 ) / baudrate ;	/*baud rate */
    U1DLM = Fdiv / 256;							
    U1DLL = Fdiv % 256;
	U1LCR = 0x03;		/* DLAB = 0 */
    U1FCR = 0x07;		/* Enable and reset TX and RX FIFO. */

    if ( install_irq( UART1_INT, (void *)UART1Handler, HIGHEST_PRIORITY ) == FALSE )
    {
	  return (FALSE);
    }

    U1IER = IER_RBR | IER_THRE | IER_RLS;	/* Enable UART0 interrupt */

    return (TRUE);
  }
  return( FALSE );
}

/*****************************************************************************
** Function name:		UARTSend
**
** Descriptions:		Send a block of data to the UART 0 port based
**						on the data length
**
** parameters:			portNum, buffer pointer, and data length
** Returned value:		None
**
*****************************************************************************/
void UARTSend( DWORD portNum, BYTE *BufferPtr, DWORD Length )
{
  if ( portNum == 0 )
  {
    while ( Length != 0 )
    {
	  /* THRE status, contain valid data */
	  while ( !(UART0TxEmpty & 0x01) );
	  U0THR = *BufferPtr;
	  UART0TxEmpty = 0;	/* not empty in the THR until it shifts out */
	  BufferPtr++;
	  Length--;
	}
  }
  else
  {
	while ( Length != 0 )
    {
	  /* THRE status, contain valid data */
	  while ( !(UART1TxEmpty & 0x01) );
	  U1THR = *BufferPtr;
	  UART1TxEmpty = 1;	/* not empty in the THR until it shifts out */
	  BufferPtr++;
	  Length--;
    }
  }
  return;
}

void myDelay(unsigned long ulTime)
{
    unsigned long i;

    i = 0;
    while (ulTime--) {
      for (i = 0; i < 5000; i++);
    }
}

void preUARTInitRBR( unsigned char i )
{
    switch (i) {
		case 0:
			U0IER = IER_THRE | IER_RLS;			/* Disable RBR */
	  		break;

		case 1:
			U1IER = IER_THRE | IER_RLS;			/* Disable RBR */
			break;
	}   
}

void postUARTInitRBR( unsigned char i )
{
    switch (i) {
	  	case 0:
			U0IER = IER_THRE | IER_RLS | IER_RBR;	/* Re-enable RBR */
	  	  	break;

		case 1:
			U1IER = IER_THRE | IER_RLS | IER_RBR;	/* Re-enable RBR */
	  		break;
	}  
}
/******************************************************************************
**                            End Of File
******************************************************************************/
```

uart.h
```c
/*****************************************************************************
 *   uart.h:  Header file for NXP LPC23xx Family Microprocessors
 *
 *   Copyright(C) 2006, NXP Semiconductor
 *   All rights reserved.
 *
 *   History
 *   2006.09.01  ver 1.00    Prelimnary version, first Release
 *
******************************************************************************/
#ifndef __UART_H
#define __UART_H

#define IER_RBR		0x01
#define IER_THRE	0x02
#define IER_RLS		0x04

#define IIR_PEND	0x01
#define IIR_RLS		0x03
#define IIR_RDA		0x02
#define IIR_CTI		0x06
#define IIR_THRE	0x01

#define LSR_RDR		0x01
#define LSR_OE		0x02
#define LSR_PE		0x04
#define LSR_FE		0x08
#define LSR_BI		0x10
#define LSR_THRE	0x20
#define LSR_TEMT	0x40
#define LSR_RXFE	0x80

/* #define BUFSIZE		0x40 */
#define BUFSIZE     0x08

DWORD UARTInit( DWORD portNum, DWORD Baudrate );
void preUARTInitRBR( unsigned char i );
void postUARTInitRBR( unsigned char i );
void UART0Handler( void ) __irq;
void UART1Handler( void ) __irq;
void UARTSend( DWORD portNum, BYTE *BufferPtr, DWORD Length );

/* */
void myDelay(unsigned long ulTime);

#endif /* end __UART_H */
/*****************************************************************************
**                            End Of File
******************************************************************************/
```

# Key
我们知道一般的开发板都会带4个按键。

在MCB2300中分别是：P1.24、P1.25、P1.26、P1.27

在用户手册中，我们可以看到PINSEL3控制着以上4个管脚。
![nxp man1](https://gitee.com/xiaobin80/csdn/raw/master/images/20150325120436044.png)


同时我们要禁止ETM
![nxp man2](https://gitee.com/xiaobin80/csdn/raw/master/images/20150325120822589.png)

## 源代码

choiceKey.c
```c
#include <LPC23XX.h>
#include "choiceKey.h"

void KeyInit(void)
{
	PINSEL3  = (PINSEL3&0xF0FFFFFF)|0x00000000;	    
	PINSEL10 = 0x00000000;		                            //禁止ETM
	IODIR1   = (IODIR1&0xF0FFFFFF)|0x00000000;	    
}
```

choiceKey.h
```c
#ifndef _CHOICEKEY_H_
#define _CHOICEKEY_H_

extern void KeyInit(void);

#endif
```

---
# 主函数

## 1. 硬件功能
### 1) CAN口
- CAN0：接收数据
- CAN1：发送数据

### 2) UART口
  在下面两个串口中任选一项。

- UART0：发送数据
- UART1：发送数据

### 3) KEY
- KEY1（P1.24）：使用CAN1发送数据
- KEY2（P1.25）：使用UART0或UART1发送数据
- KEY3（P1.26）：保留
- KEY4（P1.27）：保留

## 2. 功能块
### 1）检查CAN信息
  此功能点分为2部分：接收信息和错误处理

#### 接收信息
```c
if (CAN1GSR & 0x01)							// CAN0 接收到信息
{
CANRCVANDSEND(0,3);             // 参数分别为：（ CAN通道号，选择缓冲区（1、2、3））

recFlag1 = TRUE;
while (CAN1GSR & 0x01)					// RBS为1 接收缓冲区满
{
  mes  = CAN1CMR;
  mes |= (1 << 2);					   // 释放接收缓冲区
  CAN1CMR = mes;
}
  CAN1CMR &= ~(1 << 2);
}
```

#### 错误处理
```c
if (CAN1GSR & (0x01 << 6))					// CAN0错误计数器是否超过错误报警限制
{
  Enter_SWRst(1);
  regaddr = (unsigned long)(&CAN1GSR);		
  mes     = RGE(regaddr);
  mes    &= 0x00ff;												
  RGE(regaddr) = mes;	    				// 总线错误清除处理
  Quit_SWRst(1);
}
```

### 2) 检测按键并执行
> 定义KEY宏
```c
#define KEY(i)  ((IOPIN1&1<<i)>>i)
```

主体代码:
```c
if( KEY(24) == 0) {
	while(KEY(24) == 0 ) {
		// 执行CAN发送
	} // key24 while end
} else if ( KEY(25) == 0 ) {
	while (KEY(25) == 0) {
		// 执行UART发送
	} // key25 while end

} else if (KEY(26) == 0) {  
	while(KEY(26) == 0 );

} else if (KEY(27) == 0) {
	while(KEY(27) == 0);
}
```

## 3. 程序流程
### 1) 初始化
#### 按键初始化
```c
KeyInit();
```

#### CAN初始化
```c
CAN_Init(0,0x140002);						        //channel:0 Baud speed:1M
```

#### UART初始化
```c
UARTInit(1, 9600);	                                /* baud rate setting */
```

### 2) 监测按键
```c
while (1)
{
  /* key in - begin */
  if( KEY(24) == 0) {								 	
        while(KEY(24) == 0 ) {
      if (checkCANmessage()) {
            for (j1 = 0; j1 < 8; j1++) {
            data11[j1] = dataAB[j1];
          }

                writedetail(8, 1, dataRID, data11);

                  CANSend(0, 2);
        } // send end
      } // key24 while end

      } else if ( KEY(25) == 0 ) {

          while (KEY(25) == 0) {
      if (checkCANmessage()) {
            /* This is new append code 20110222 */
          for (j2 = 0; j2 < 8; j2++) {
              UART1Buffer[j2] = dataAB[j2];
          }

              preUARTInitRBR(1);
              UARTSend( 1, (BYTE *)UART1Buffer, 8 );       // channel:1
              postUARTInitRBR(1); 		
        } // send end

    } // key25 while end

      } else if (KEY(26) == 0) {  	

        while(KEY(26) == 0 );

      } else if (KEY(27) == 0) {
        UART1Count = 0x0F;
        while(KEY(27) == 0);
    }
    /* key in - end */

}  /* while end	*/
```

## 附录
### 1. 主函数
main.c
```c
#include "LPC23xx.h"
#include "../common/type.h"
#include "../common/irq.h"
#include "../uart/uart.h"
#include "../common/target.h"
#include "../can/LPC2300CAN.h"
#include "../common/key/choiceKey.h"

#define KEY(i)  ((IOPIN1&1<<i)>>i)
#define key1    ((IOPIN1&1<<24)>>24)
#define key2    ((IOPIN1&1<<25)>>25)
#define key3    ((IOPIN1&1<<26)>>26)
#define key4    ((IOPIN1&1<<27)>>27)

extern unsigned char dataAB[8];
extern unsigned long dataRID;

extern volatile DWORD UART0Count;
extern volatile BYTE UART0Buffer[BUFSIZE];
extern volatile DWORD UART1Count;
extern volatile BYTE UART1Buffer[BUFSIZE];

BOOL checkCANmessage(void);

int main(void)
{
	unsigned char j, j1, j2;

	unsigned char data11[] = {0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00};
	j1 = j2 =0;

	KeyInit();

	CAN_Init(0,0x140002);						        //channel:0 Baud speed:1M
    // CAN_Init(1,0x140002);						    //channel:1 Baud speed:1M

	// UARTInit(0, 9600);	                            /* baud rate setting */
    UARTInit(1, 9600);	                                /* baud rate setting */
	while (1)
	{
		/* key in - begin */
		if( KEY(24) == 0) {								 	
	        while(KEY(24) == 0 ) {
				if (checkCANmessage()) {
			        for (j1 = 0; j1 < 8; j1++) {
					    data11[j1] = dataAB[j1];
				    }

	                writedetail(8, 1, dataRID, data11);

                    CANSend(0, 2);
			    } // send end
		    } // key24 while end

        } else if ( KEY(25) == 0 ) {

            while (KEY(25) == 0) {
				if (checkCANmessage()) {
			        /* This is new append code 20110222 */
				    for (j2 = 0; j2 < 8; j2++) {
				        UART1Buffer[j2] = dataAB[j2];
				    }

		            preUARTInitRBR(1);
		            UARTSend( 1, (BYTE *)UART1Buffer, 8 );       // channel:1
		            postUARTInitRBR(1); 		
			    } // send end

			} // key25 while end

        } else if (KEY(26) == 0) {  	

	        while(KEY(26) == 0 );

        } else if (KEY(27) == 0) {
    	    UART1Count = 0x0F;
	        while(KEY(27) == 0);
	    }
	    /* key in - end */

	}  /* while end	*/

    return (0);
}

BOOL checkCANmessage(void) {
    unsigned int regaddr, mes;
    BOOL recFlag1 = FALSE;
    if (CAN1GSR & 0x01)							// CAN0 接收到信息
    {
		CANRCVANDSEND(0,3);                     // 参数分别为：（ CAN通道号，选择缓冲区（1、2、3））

		recFlag1 = TRUE;
		while (CAN1GSR & 0x01)					// RBS为1 接收缓冲区满
		{
			mes  = CAN1CMR;
			mes |= (1 << 2);					// 释放接收缓冲区
			CAN1CMR = mes;
		}
			CAN1CMR &= ~(1 << 2);
	}		

	if (CAN1GSR & (0x01 << 6))					// CAN0错误计数器是否超过错误报警限制
	{
		Enter_SWRst(1);
		regaddr = (unsigned long)(&CAN1GSR);		
		mes     = RGE(regaddr);
		mes    &= 0x00ff;												
		RGE(regaddr) = mes;	    				// 总线错误清除处理
		Quit_SWRst(1);
	}

	return (recFlag1);		
}

/*********************************************************************************************************
**                            End Of File
********************************************************************************************************/
```

# 项目打包文件
[selUtCan23.7z](http://pan.baidu.com/s/1mgHuFfq)

# reference
- [用户手册LPC23XX User manual Rev. 03 — 25 August 2009 User manual](http://pan.baidu.com/s/1hqAboA4)
