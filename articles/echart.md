# 那些年，为echarts留下的眼泪

## 图表选型原因
业界内比较流行的图表插件主要有**D3** 🆚 **Highcharts** 🆚 **ECharts**
- D3。主要是形成svg，适合交互类的图表，上手成本高，不利于快速开发需要，未选择；
- Highcharts。也是形成svg，适合交互类的图表，上手成本一般，有相应的文档与开发生态，最初考虑到不兼容IE8会有点问题，所以没选择，后面才发现是自己姿势错了。
- ECharts。底层是Canvas，上手成本一般，文档与开发生态良好，主要是知道兼容IE8，所以选择了它。

## ECharts相关
### 在线定制图表
ECharts可以自行配置js包的内容，访问[链接](https://echarts.apache.org/zh/builder.html)即可根据自身需求打包需要的图表。（PS：最下方有“兼容IE8”的选项）

我定制的的兼容IE8的图表链接 👉 [https://shineasyr.github.io/FESource/echarts3_8_0.mini.js](https://shineasyr.github.io/FESource/echarts3_8_0.mini.js)

关于ECharts的更多属性介绍可以点击[链接](https://echarts.apache.org/zh/option.html)自行搜索

### 兼容与视觉还原的大PK

视觉稿大致风格如下图所示，有双折线，面积需要为渐变色，XY轴的也为自定义颜色。
![图表视觉稿](imgs/echart-ui.png)

**1. 快速上手**
（若为ECharts熟练工，可跳到下一趴）
官方快速上手[文档](https://echarts.apache.org/zh/tutorial.html#5%20%E5%88%86%E9%92%9F%E4%B8%8A%E6%89%8B%20ECharts)，下面贴一下视觉还原的页面（PS：IE8兼容性，建议本地拷贝后打开测试）。

界面链接 👉 [codePen](https://codepen.io/shineasyr/full/xxqgeZx) 、<a href="https://shineasyr.github.io/Produce101/echart-fe.html" target="_blank">Github Page</a>
```html
<div class="shinea-chart" id="shineaChart" ></div>
.shinea-chart {min-height: 400px;}

```
还原界面如下所示
![图表还原界面](imgs/echart-fe.png)

**2. 各类特殊设置**

参数设置文档 👉 [https://echarts.apache.org/v4/zh/option.html](https://echarts.apache.org/v4/zh/option.html)
- 面积渐变色 `areaStyle` 
```javascript
// series里设置
areaStyle: {
    normal: {
        color: {
        type: 'linear',
        x0: 0,
        y0: 0,
        x2: 0,
        y2: 1,
        colorStops: [{
            offset: 0,
            color: 'rgba(0, 103, 229, 0.2)',
        }, {
            offset: 0.5,
            color: 'rgba(0, 103, 229, 0.08)',
        }, {
            offset: 1,
            color: 'rgba(0, 103, 229, 0)',
        }],
        globalCoord: false
        }
    }
}
```
- XY轴线设置
```javascript
// xAxis/yAxis里设置
axisTick: {show: false},  //  坐标轴刻度-短线
axisLabel: {              // 刻度标签
    textStyle: {
        color: '#525252',
        fontSize : 12
    }
},
axisLine: {             // 坐标轴轴线
    lineStyle: {
        color: "rgba(21, 26, 48, 0.12)",
        type: "dashed"
    },
},
// 分割线，数值轴特有(yAxis)
splitLine: {
    lineStyle: {
        color: "rgba(21, 26, 48, 0.12)",
        type: "dashed"
    }
}
```
- 数值轴设置(yAxis)
```javascript
// 单位
yAxis: [{
    axisLabel: {  
      show: true,
      formatter: '{value}%'   // 单位
    },
}]

// 最小刻度 ，可以设置 0.01 或 1 或其他
yAxis: [{
    axisLabel: {  
      show: true,  
      minInterval: 1,     // 最小刻度
    },
}]

// 刻度标签间距 避免数字标签文本被切割，稍微下移
yAxis: [{
    axisLabel: {  
      show: true,
      padding: [5, 0, 0, 0],    // 间距
    },
}]
```
交互问题：
a: 数据为0是，数值轴会出现小数——调整最小刻度即可;
b: 数值轴需要单位，`formatter`一下即可

![图表还原界面](imgs/echart-question.png)
- 类目轴设置(xAxis)
```javascript
// 间隔 0一个挨一个显示，1显示一个跳过一个再显示
xAxis: {
    axisLabel: {
        interval: 1,
    },
}
```
- 样式设置
```javascript
// 图表绘制网格
grid: {
    top: '3%',
    left: '2%',
    right: '3%',
    bottom: '12%',
    containLabel: true  // grid 区域是否包含坐标轴的刻度标签
},
```

## 小结
- EChart支持图表定制&IE8兼容
- [入门](https://echarts.apache.org/zh/tutorial.html)很简单
- [各类设置](https://echarts.apache.org/v4/zh/option.html)需要多尝试

以上，结束啦～

如果后续还有其他的图表尝试，会继续更新。

如有错误，欢迎指出，也欢迎小伙伴们的热情交流