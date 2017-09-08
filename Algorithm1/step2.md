# 步骤2-初始布局
> 首先，配置多少车，就有多少个路牌，接着确定第一个班次到底是上行还是下行（算法下面说道），以早高峰开始时间为最后一个路牌的第一个班次的发车时间，然后反向计算上面路牌的第一个开始班次的时间（减去低谷发车间隙），这个过程叫纵向展开。每个路牌以第一个班次的发车时间去补全后面的班次，每个班次都是一个对象（包含发车时间，当前方向时间段内的行驶时间，从而计算出到达时间，当前方向时间段内的停站时间），下一个班次的发车时间＝上一个班次的到达时间＋上一个班次的停战时间，然后每计算一个班次需要切换一个方向，一直计算到结束时间（结束时间为最大的末班车时间，要么上行，要么下行），这个过程叫横向展开。
![](/assets/step2_1.png)

## 一，计算路牌第一个班次的方向

```
// 下行班车时间+下行行驶时间+下行停站时间 > 上行头班时间
// 如果true=上行，如果false=下行
getdefaultDir01 : function(list , xxsj , tzsj) {
	var sxtbcsj =  baseF.getDateTime(list[0].kssj);
	var xxtbcsj =  baseF.getDateTime(list[1].kssj);
	xxtbcsj.setMinutes(xxtbcsj.getMinutes() + xxsj + tzsj);
	if(xxtbcsj>sxtbcsj) {
		return 0;
	}else {
		return 1;
	}
}
```

## 二，结束时间确定

```
// 比较上下行哪个末班车晚
function getStartAndEndDate(map) {
		return {'s': map.linePlayType == '1' ? map.startStationFirstTime : getMinDate(map.startStationFirstTime,map.endStationFirstTime),
				'e': map.linePlayType == '1' ? map.startStationEndTime : getMaxDate(map.startStationEndTime,map.endStationEndTime)}
	}
```





