	for(i=0;i<sizeof(USART_RX_BUF);i++)
			{
				if(USART_RX_BUF[i]==0x2B||USART_RX_BUF[i]==0x2D||USART_RX_BUF[i]==0x2A||USART_RX_BUF[i]==0x2F)
				{
					int t;
				  location=i;
				  mark_bit = USART_RX_BUF[i];
					for(t=0;t<location;t++)
					{
						buf[t]=USART_RX_BUF[t];
					}
				    behind_len=sizeof(USART_RX_BUF)-i;
					for(behind=location;behind<sizeof(USART_RX_BUF);behind++)
			    {
				    buf1[behind-location-1]=USART_RX_BUF[behind];
			    }
				}
      }
			   

			switch(mark_bit)
			 {case 43:
			   int1=atoi(buf);
		     int2=atoi(buf1);
			   int3=int2+int1;
				 break;
			 case 45:
				 int1=atoi(buf);
		     int2=atoi(buf1);
			   int3=int1-int2;
			   break;
			 case 42:
				 int1=atoi(buf);
		     int2=atoi(buf1);
			   int3=int1*int2;
			   break;
			 case 47:
				 int1=atoi(buf);
		     int2=atoi(buf1);
			   int3=int1/int2;
			   break;
			 }
串口计算器程序，每次串口输入	计算的第一个数运算符和第二个数，利用USART_RX_STA的高两位判断将每次发送回车和换行视为一次串口输入结束，依据串口接收函数USART_ReceiveData()，将接受到的数据	放入 USART_RX_BUF[]中，利用以上函数对接收的数据进行切片和计算，用FOR循环对接受的数据进行遍历，如果遍历过程中遇到加减乘除符号（十六进制ASCII值）则记录此时的遍历到此的数组下标值并利用此数值将运算符位之前的数据放入一个字符数组（此数组为第一个计算数），此外利用总长度和运算符位将剩余长度USART_RX_BUF[]的数据拿出并赋值到另一个字符数组（此为第二个计算数），最后利用switch函数判断是具体哪个运算符，在case里用atoi()函数将存到数组里的两个转换数据类型并赋值给整型变量并运算输出
short Get_Temprate(void)	
{
	u32 adcx;
	short result;
 	double temperate;
	adcx=T_Get_Adc_Average(ADC_Channel_16,20);	
	temperate=(float)adcx*(3.3/4096);		
	temperate=(1.43-temperate)/0.0043+25;		 
	result=temperate*=100;					
	return result;
利用此函数获取内部温度传感器温度平均值，读取ADC通道16的数据，20次取平均值获得电压值并计算将电压值转换为温度值，输出。

