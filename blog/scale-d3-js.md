title: D3学习笔记之scale对象
date: 2015-07-16 20:21:18
tags: [D3.js]
---
scale是D3.js中提供的一个对象，通过该对象可以将一个输入域映射到合适的输
出范围，输入域和输出范围都是可以自己设置的，默认的输入域和输出范围都是
[0, 1];举个例子：若输入域是[100, 500]输出范围是[10, 350]，则输入值100映
射到比例尺上的值就是10，300映射的则是180,500对应的就是比例尺的最大值350.

当我们想要表现非常大的数值时，比如每个月的销售额，第一个月是200，第二个
月是500，我们可以将每个数值对应相应的条形图的高度，然而如果后面的销售额
越来越大时，还是用原来的方法，将数值对应相应的高度，显然是不可行的。我们
需要找到一种更灵活的转换方法，能将输入的值转化为合适的输出值。而scale对象
正好提供了这些方法。

D3提供了好几个比例尺方法，这里主要介绍下线性（linear）比例尺；

#### 生成一个比例尺
``` js
var scale = d3.scale.linear();
```
这样就生成了一个比例尺，此时的设置的是默认的输入域和输出范围[0, 1],可以测
试一下，
``` js
scale(0.5); // 0.5
```

#### 设置输入域
``` js
scale.domain([100, 500]);
```
参考d3.js的API，其中有描述domain()接受一个数值数组参数，数值的个数须是两个或
两个以上（输入域和输出范围的数值个数须保持一致），如果数组中的元素不是数值类
型时，将会在scale对象被调用时转为数值类型(Number);

对于输入域中包含三个数值的情况，API中提供了一个参考示例，代码如下：

``` js
var color = d3.scale.linear()
	    .domain([-1, 0, 1])
	    .range(["red", "white", "green"]);
```
这里我测试了下三个输入值对应的输出值，分别是红、白、绿对应的十六进制颜色值；
``` js
console.log(color(-1));	//#ff0000
console.log(color(0));  //#ffffff
console.log(color(1));  //#008000
```
<!-- more -->
也就是说[-1, 0]映射到的是红色(#ff0000)到白色(#ffffff)的颜色范围，而[0, 1]映射到
的是白色(#ffffff)到绿色(#008000)范围内的颜色。这样也就相当于定义了两个量化尺度，
因此输入域有多个值情况，适用于有多个量化尺度的情况；

#### 设置输出范围
``` js
scale.range([10, 350]);
```
range()接受一个数组参数，参数中的元素不一定是数值类型，但数组中的元素个数必须
和输入域中参数数组中的数值个数匹配；

经过上面三个步骤就完成了一个完整的比例尺的生成，当然上面三个步骤可写的更简洁些，
如下：
``` js
var scale = d3.scale
	  .linear()
	  .domain([100, 500])
	  .range([10, 350]);
```

#### 其他方法
d3.scale.linear()还有几个非常方便的方法
###### nice()
调用该方法可以取得range()范围内最接近的整数值，比如值域为[0.200015, 0.99552245]
会被优化为[0.2, 1];

##### rangeRound()
用rangeRound()代替range()后，比例尺所有的输出值都会舍入到最接近的整数值；


##### clamp()
默认情况下，比例尺可以返回指定范围外的值；也就是说当输入一个输入域外的值，比例尺
也会返会一个值域外的值，而调用clamp(true)后，则可强制所有的输出值都在指定范围内；
当有超出范围的值时，会取到指定范围上最接近的值；


可以这样调用以上方法
``` js
var scale = d3.scale
	  .linear()
	  .domain([100, 500])
	  .range([10, 350])
	  .nice();
```

