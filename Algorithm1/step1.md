# 步骤1-参数计算

> 在生成时刻表之前，本算法必须设置一定量的参数，包括必填参数和可选参数，模型参数设置后，系统会计算关联的参数（会给出相关代码），此处以浦东38路举例。

## 一，上下行首末班车的行驶时间（默认从线路标准中获取）

![](/assets/step1.png)

* 上行首班车的行驶时间（startStationFirstTime，必填）
* 上行末班车的行驶时间（startStationEndTime，必填）
* 下行首班车的行驶时间（endStationFirstTime，必填）
* 下行末班车的行驶时间（endStationEndTime，必填）

```
// 相关计算代码：

formatksjssj : function(gp) {
   return [{'kssj':gp.startStationFirstTime,'jssj':gp.startStationEndTime},
   {'kssj':gp.endStationFirstTime,'jssj':gp.endStationEndTime}];
}

// 起终点站首末班车时间.[下标0代表起始站的首末班车时间；下标1代表终点站的首末班车时间]
'smbcsjArr' : BaseFun.formatksjssj(gatps)
```

## 二，早晚高峰开始结束时间（默认从线路标准中获取）

![](/assets/step2.png)

* 早高峰开始时间（earlyStartTime，必填）
* 早高峰结束时间（earlyEndTime，必填）
* 晚高峰开始时间（lateStartTime，必填）
* 晚高峰结束时间（lateEndTime，必填）

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

## 三，行驶时间（默认从线路标准中获取）

![](/assets/step3.png)  
![](/assets/step4.png)

* 上行行驶时间（upTravelTime，必填）
* 下行行驶时间（downTravelTime，必填）
* 早高峰上行时间（earlyUpTime）
* 早高峰下行时间（earlyDownTime）
* 晚高峰上行时间（lateUpTime）
* 晚高峰下行时间（lateDownTime）
* 低谷上行时间（troughUpTime）
* 低谷下行时间（troughDownTime）

```
//在计算各个时间段的行驶时间时，默认设置为行驶时间，如果指定了特定行驶时间，如早高峰的时间，则使用

// 平常行驶时间。[下标0代表上；下标1代表下]
'pcxssjArr' : BaseFun.formatPairing(
    gatps.upTravelTime,
    gatps.downTravelTime),
// 高峰行驶时间。[下标0代表上；下标1代表下]
'gfxxsjArr' : BaseFun.formatPairing(
    gatps.lateUpTime=='' ? gatps.upTravelTime : gatps.lateUpTime,    
    gatps.lateDownTime=='' ? gatps.downTravelTime : gatps.lateDownTime),
// 低谷行驶时间。[下标0代表上；下标1代表下]
'dgxxsjArr' : BaseFun.formatPairing(
    gatps.troughUpTime=='' ? gatps.upTravelTime : gatps.troughUpTime,
    gatps.troughDownTime=='' ? gatps.downTravelTime : gatps.troughDownTime)
```

## 四、停站时间（没有默认值，人工输入）

* 高峰上行停站时间（gfupStopTime，必填）
* 高峰下行停站时间（gfdownStopTime，必填）
* 低谷上行停站时间（dgupStopTime，必填）
* 低谷下行停站时间（dgdownStopTime，必填）
* 低谷最大停站时间（dgmaxtzsj，必填）

```
// 相关代码
// 高峰停站时间.
'gftzsj':BaseFun.formatPairing(
    gatps.gfupStopTime,
    gatps.gfdownStopTime),
// 低谷停站时间.
'dgtzsj':BaseFun.formatPairing(
    gatps.dgupStopTime,
    gatps.dgdownStopTime),
// 低谷最大停站时间.
'dgmaxtzsj' :  parseInt(gatps.dgmaxtzsj),
```

## 五，周转时间计算（根据之前的参数设置计算出来）

```
// 里面计算高峰周转时间用的是早高峰的数据
function getzzsj(map) {
    // 一圈所用时间 (上行+下行+停歇时间)
    return {
        // 高峰周转时间
        'gfzzsj':  parseInt(map.earlyUpTime =='' ? map.upTravelTime : map.earlyUpTime) + parseInt(map.gfupStopTime) + parseInt(map.gfdownStopTime) + parseInt(map.earlyDownTime =='' ? map.downTravelTime : map.earlyDownTime ), 
        // 低谷周转时间
    'dgzzsj':  parseInt(map.troughUpTime==''? map.upTravelTime : map.troughUpTime) + parseInt(map.dgupStopTime) + parseInt(map.dgdownStopTime) + parseInt(map.troughDownTime==''? map.downTravelTime: map.troughDownTime),
    // 低谷最大的周转时间
    'dgmaxzzsj' : parseInt(map.troughUpTime==''? map.upTravelTime : map.troughUpTime) + parseInt(map.dgmaxtzsj) + parseInt(map.dgmaxtzsj) + parseInt(map.troughDownTime==''? map.downTravelTime: map.troughDownTime),        };
    }
```

## 六，发车间隙（没有默认值，人工输入）

* 低谷最大发车间隙（dgmaxfcjx，必填）

```
// 低谷最大发车间隙.
'dgmaxfcjx' :  parseInt(gatps.dgmaxfcjx),

function getMaxCarAndStopSpace1(map) {
    // ...
    // 里面的车辆总数是后面设定的参数
    // round按照四舍五入计算

    'fcjx': {
        'gffcjx': Math.round(map.zzsj.gfzzsj/map.clzs) , 
    'dgfcjx': Math.round(map.zzsj.dgzzsj/map.clzs),
    'dgmaxfcjx' : parseInt(map.dgmaxfcjx)
    },
    // ...
}
```

## 七，班型配置和车辆数（没有默认值，人工输入）

* 班型人次车辆（bxrc，必选）
* 车辆总数（clzs，必填）
  ```
    //班型可以设置多种，如常用的配置一休一班型加五休二版型
    //每个班型还需填写人次和车辆数和平均工时
    // 这里假设每个班型有几辆车就有几个路牌，一般一个班型都有一个标准工时，这里设定了一个平均工时是允许每个路牌的工时都可以浮动，只要总的平均工时不超
    // 支持分班，就是一个路牌2个人做，此时设定必须是人次是2倍的路牌
    // TODO：暂时不支持，一个路牌多个班型
  ```

## TODO：



