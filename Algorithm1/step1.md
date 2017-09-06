# 步骤1-参数设置
> 在生成时刻表之前，本算法必须设置一定量的参数，包括必填参数和可选参数，模型参数设置后，系统会计算关联的参数（会给出相关代码），此处以浦东38路举例。

## 一，上下行首末班车的行驶时间（从线路标准中获取）
> 从form表单中获取参数，并封装成相应对象

### 相关参数，代码，图示

* 上下行首末班车的行驶时间（从线路标准中获取）
* #### ddd

```
// 相关计算代码：

formatksjssj : function(gp) {
   return [{'kssj':gp.startStationFirstTime,'jssj':gp.startStationEndTime},
   {'kssj':gp.endStationFirstTime,'jssj':gp.endStationEndTime}];
}

// 起终点站首末班车时间.[下标0代表起始站的首末班车时间；下标1代表终点站的首末班车时间]
'smbcsjArr' : BaseFun.formatksjssj(gatps)

```
![](/assets/step1.png)
* *早晚高峰开始结束时间（从线路标准中获取）

```
// 相关时间段计算代码
'zgfsjd' : BaseFun.getsd(
    BaseFun.getDateTime(gatps.earlyStartTime),
    BaseFun.getDateTime(gatps.earlyEndTime)), // 早高峰时间段
'wgfsjd' : BaseFun.getsd(
    BaseFun.getDateTime(gatps.lateStartTime),
    BaseFun.getDateTime(gatps.lateEndTime)),// 晚高峰时间段
'gfzjsjd' : BaseFun.getsd(
    BaseFun.getDateTime(gatps.earlyEndTime),
    BaseFun.getDateTime(gatps.lateStartTime)),//高峰之间时间段
'wgfzhsjd' : 
    BaseFun.getsd(BaseFun.getDateTime(gatps.lateEndTime),
    BaseFun.getDateTime(seMap.e)),// 晚高峰之后时间段
'zgfzqsjd': 
    BaseFun.getsd(BaseFun.getDateTime(seMap.s),
    BaseFun.getDateTime(gatps.earlyStartTime)),//早高峰之前时间段
    
```
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