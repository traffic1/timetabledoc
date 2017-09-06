# 步骤1-参数设置
##### 模型的参数设置说明，与后续的计算直接相关，此处以浦东38路举例，带*的为必填项
* *上下行首末班车的行驶时间（从线路标准中获取）

```
formatksjssj : function(gp) {
   return [{'kssj':gp.startStationFirstTime,'jssj':gp.startStationEndTime},
   {'kssj':gp.endStationFirstTime,'jssj':gp.endStationEndTime}];
}

// 起终点站首末班车时间.[下标0代表起始站的首末班车时间；下标1代表终点站的首末班车时间]
'smbcsjArr' : BaseFun.formatksjssj(gatps)

```

![](/assets/step1.png)
* *早晚高峰开始结束时间（从线路标准中获取）
![](/assets/step2.png)
* *上行行驶时间，早高峰上行行驶时间，晚高峰上行行驶时间，低谷上行时间
* *下行行驶时间，早高峰下行行驶时间，早高峰下行行驶时间，低谷下行时间

```
    //在计算各个时间段的行驶时间时，默认设置为行驶时间，如果指定了特定行驶时间，如早高峰的时间，则使用
    'pcxssjArr' : BaseFun.formatPairing(gatps.upTravelTime,gatps.downTravelTime),// 平常行驶时间。[下标0代表上；下标1代表下]
    'gfxxsjArr' : BaseFun.formatPairing(gatps.lateUpTime=='' ? gatps.upTravelTime : gatps.lateUpTime,
													gatps.lateDownTime=='' ? gatps.downTravelTime : gatps.lateDownTime),// 高峰行驶时间。[下标0代表上；下标1代表下]
    'dgxxsjArr' : BaseFun.formatPairing(gatps.troughUpTime=='' ? gatps.upTravelTime : gatps.troughUpTime,
										gatps.troughDownTime=='' ? gatps.downTravelTime : gatps.troughDownTime),// 低谷行驶时间。[下标0代表上；下标1代表下]
    
```
![](/assets/step3.png)
![](/assets/step4.png)