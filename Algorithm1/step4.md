# 步骤4，删除多余班次，确定首末班
> 删除上行，下行班次链的多余班次，并将班次链的第一个班次和最后一个班次，修正为首班车时间和末班车时间。

* 删除多余班次，不在首班车，末班车时间范围内的班次
* 然后计算第一个班次和最后一个班次的时间，调整对应的班次对象的发车时间，班次历时，到达时间
* 第一个班次，发车时间=首班车时间，班次历时＝高峰行驶时间
* 最后一个班次，发车时间=末班车时间，班次历时＝低谷行驶时间
![](/assets/step3_5.png)