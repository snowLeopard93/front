## HighCharts

### 一、前置

#### 1、HighCharts引入

> **通过script标签引入**

```html
<script src="http://cdn.highcharts.com.cn/highcharts/highcharts.js"></script>
```

> **通过require引入**

```javascript
// 先将 highcharts.js 放入到 lib 目录下，然后再通过require引入
require([
    "lib/highcharts"
], function (highcharts) {
    // ......
})
```
#### 2、HighCharts常用方法

> 初始化

```javascript
var chartOption = {
    //......
};
$("#demoDiv").highcharts(chartOption);
var demoChart = $("#demoDiv").highcharts();
```

> 自适应

```javascript
var demoChart = $("#demoDiv").highcharts();

function resizeChart(){
    demoChart && demoChart.reflow();
}

$(window).bind("resize", resizeChart);
```

### 二、常用图表简单示例

#### 1、通用属性设置

> **图例（legend）**

```javascript
"legend": {
    "itemStyle": {
        "fontWeight": "normal"
    }
}
```


> **坐标轴（xAxis、yAxis）**

**（1）xAxis**

```javascript
"xAxis": {
    "categories": ["10:00", "11:00", "12:00", "13:00"],
    "labels": {
        "rotation": 0,
        "step": 0 //横坐标间隔显示
    }
}
```

**（2）yAxis**

> **悬浮提示信息（tooltip）**

```javascript
"tooltip": {
    "pointFormat": "{series.name}: <b>{point.percentage:.1f}%</b>"
}
```

#### 2、chart设置

**（1）chart.type** 

|  **类型**    | **值**     |
| ------------- |-------------|
| **折线图** | line |
| **曲线图** | spline |
| **面积图** | area |
| **柱状图** | column |
| **条形图** | bar |
| **饼图** | pie |
| **仪表图** | gauge |

```javascript
"chart": {
    "type": "spline",
    "zoomType": "x",
    "panning": true,
    "panKey": "ctrl"
},
```

#### 2、plotOptions设置


**（1）column.stacking** 

```javascript
// 展开
plotOptions.column.stacking = "";

// 堆叠
plotOptions.column.stacking = "normal";
```

> **折线图（曲线图）**

```javascript
"plotOptions": {
    "column": {
        "stacking": ""
    },
    "area": {
        "fillColor": {
            "linearGradient": {
                "x1": 0,
                "y1": 0,
                "x2": 0,
                "y2": 1
            }
        },
        "marker": {
            "radius": 2
        },
        "lineWidth": 1,
        "states": {
            "hover": {
                "lineWidth": 1
            }
        },
        "threshold": null
    },
    "series": {
        "pointPadding": 0.1,
        "connectNulls": false,
        "dataLabels": {
            "enabled": true,
            "color": "#000",
            "style": {
                "fontWeight": "normal"
            },
            "formatter": function () {
                if (this.y == 0) {
                    return '';
                } else {
                    return this.y;
                }
            }
        }
    }
}
```

> **柱状图（堆叠、展开）**

```javascript
"plotOptions": {
    "column": {
        "stacking": ""
    },
    "area": {
        "fillColor": {
            "linearGradient": {
                "x1": 0,
                "y1": 0,
                "x2": 0,
                "y2": 1
            }
        },
        "marker": {
            "radius": 2
        },
        "lineWidth": 1,
        "states": {
            "hover": {
                "lineWidth": 1
            }
        },
        "threshold": null
    },
    "series": {
        "pointPadding": 0.1,
        "connectNulls": false,
        "dataLabels": {
            "enabled": true,
            "color": "#000",
            "style": {
                "fontWeight": "normal"
            },
            "formatter": function () {
                if (this.y == 0) {
                    return '';
                } else {
                    return this.y;
                }
            }
        }
    }
}
```

> **饼图（环形图）**

```javascript
"plotOptions": {
    "pie": {
        "allowPointSelect": true,
        "cursor": "pointer",
        "dataLabels": {
            "enabled": true,
            "format": "<b>{point.name}</b>: {point.percentage:.1f} %",
            "style": {
                    "color": (Highcharts.theme && Highcharts.theme.contrastTextColor) || "black"
            }
        }
    }
}
```

#### 3. series设置

> **折线图（曲线图）**

```javascript
"series": [
    {"name": "折线1","data": [
        10, 20, 30, 40, 50, 60
    ]},
    {"name": "折线2","data": [
        60, 50, 40, 30, 20, 10
    ]}
]
```

> **柱状图（堆叠、展开）**

```javascript
"series": [
    {"name": "柱状图1","data": [
        1000
    ]},
    {"name": "柱状图2","data": [
        2000
    ]}
]
```
> **饼图（环形图）**

```javascript
// 饼图
series[0].innerSize = "";

// 环形图
series[0].innerSize = "80%";
```

```javascript
"series": [{
    "type": "pie",
    "innerSize": '',
    "name": "市场份额",
    "data": [{
        "name": "Chrome",
        "y": 61.41,
        "sliced": true,
        "selected": true
    }, {
        "name": "Internet Explorer",
        "y": 11.84
    }, {
        "name": "Firefox",
        "y": 10.85
    }, {
        "name": "Edge",
        "y": 4.67
    }, {
        "name": "Safari",
        "y": 4.18
    }, {
        "name": "Sogou Explorer",
        "y": 1.64
    }, {
        "name": "Opera",
        "y": 1.6
    }, {
        "name": "QQ",
        "y": 1.2
    }, {
        "name": "Other",
         "y": 2.61
    }]
}]
```

**参考**

[HighCharts官网](https://www.highcharts.com.cn/)

[HighCharts API](https://api.highcharts.com.cn/highcharts)

[1 分钟上手 Highcharts](https://www.highcharts.com.cn/docs/start-helloworld)