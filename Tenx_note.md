## 笔记

输出电压乘占空比

```
	heatVol = (uint32_t)GetADC.VoutVDD * GetADC.LoadRes / 4096;
							AD_Temp = 348000UL / ((u32)heatVol); 

							if (AD_Temp >= BuckPwmCycle)
								RunPWM.BuckPwmDutyBuff = BuckPwmCycle;
							else if (AD_Temp < BuckPwmMinCycle)
								RunPWM.BuckPwmDutyBuff = BuckPwmMinCycle;
							else
								RunPWM.BuckPwmDutyBuff = AD_Temp;
```



## 模版代码

#### 空载检测

PowerProgress（）函数

  if (F_EnSmoke) // 如果启用烟雾检测

  {

​    ClrEnSmoke; // 清除烟雾标志

​    if (!F_Firing && (Display_Mode < LOW_LEVEL)) // 如果未点火且显示模式低

​    {  

```
  			GPIO_OD; // 设置GPIO为开路状态
            GPIO_OD_CH2; // 设置第二通道GPIO为开路状态

            // 检查负载状态
            if (GPIO_UnLoad_EN == 1 || GPIO_UnLoad_CH2_EN == 1)
            {
                SetWarnEvent(&RunEventLED, HighRes, 25, 10); // 设置高电流警告事件
            }
            else if (GetADC.BatteryVoltage <= 3300 || (u8OldPrecent == 0))
            {
                GetADC.BatteryPercent = 0; // 电池百分比为0
                u8OldPrecent = 0; // 更新旧百分比
                SetWarnEvent(&RunEventLED, LOW_LEVEL, 15, 40); // 设置低电量警告事件
            }
            else
            {
                GPIO_ADC; // 设置GPIO为ADC状态
                GPIO_ADC_CH2; // 设置第二通道GPIO为ADC状态

                SetSmokeFlag; // 设置烟雾标志
```







