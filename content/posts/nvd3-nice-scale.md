+++
title = "nvd3中计算坐标轴的nice domain"
date =  2014-12-24T21:39:06+08:00
draft = true
tags = ["javascript"]
categories = ["d3","nvd3","chart","angularjs"]
image="/images/nvd3_axis_1.png"
+++
最近用[nvd3](https://github.com/novus/nvd3)以及[angularjs-nvd3-directives](https://github.com/angularjs-nvd3-directives/angularjs-nvd3-directives)做chart遇到一个问题，如下图，y轴中的最大值是3.8。nvd3只会在坐标轴显示到数据最大值，但是有些情况下最大值并不是很好看，我们需要在这种情况下显示4.0。

![](/images/nvd3_axis_1.png)

最开始查了下d3的文档，发现有个[nice()](https://github.com/mbostock/d3/wiki/Quantitative-Scales)方法可能会做到，但是尝试了下，不生效。查了下，可能是nvd3的[bug](https://github.com/novus/nvd3/issues/594)。要控制坐标轴的显示还可以通过xDomain(),yDomain()来实现，关键问题是如何计算domain的nice值。

### 计算数据中的最小值，最大值
```javascript
var yAxisValues = [1,2.4,3.6,7.8,9.2];
var yAxisMax = d3.max(yAxisValues)
var yAxisMin= d3.min(yAxisValues)
```
我们可以通过d3中提供的max方法得到数据中的最小值，最大值

### 根据给定的最小值和最大值计算出nice值
```javascript
app.factory('niceScale', [function () {
        var factory = {};

        /// <summary>
        ///  Returns a "nice" number approximately equal to range Rounds
        ///  the number if round = true Takes the ceiling if round = false.
        /// </summary>
        /// <param name="range">the minimum data point on the axis</param>
        /// <param name="round">the maximum data point on the axis</param>
        function niceNum(range, round) {
            // exponent of range
            var exponent;
            // fractional part of range
            var fraction;
            // nice, rounded fraction
            var niceFraction;
            exponent = Math.floor(Math.log10(range));
            fraction = range / Math.pow(10, exponent);

            if (round) {
                if (fraction < 1.5) niceFraction = 1;
                else if (fraction < 3) niceFraction = 2;
                else if (fraction < 7) niceFraction = 5;
                else niceFraction = 10;
            } else {
                if (fraction <= 1) niceFraction = 1;
                else if (fraction <= 2) niceFraction = 2;
                else if (fraction <= 5) niceFraction = 5;
                else niceFraction = 10;
            }
            return niceFraction * Math.pow(10, exponent);
        }

        factory.calculate = function (min, max, maxTicks) {
            maxTicks = maxTicks ? maxTicks : 10;
            var range = niceNum(max - min, false);
            var tickSpacing = niceNum(range / (maxTicks - 1), true);
            var niceMin = Math.floor(min / tickSpacing) * tickSpacing;
            var niceMax = Math.ceil(max / tickSpacing) * tickSpacing;
            return [niceMin, niceMax];
        }

        return factory;
    }]);
```
上面定义了一个angular中的service用来处理算法

### 设置chart的domain
```javascript
    chart.yDomain([niceMin,niceMax]);
```