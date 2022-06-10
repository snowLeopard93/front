### 一、前置

#### 1、Charts引入

> **通过script标签引入**

```html
<script src="http://lib.jiaminghi.com/charts/charts.min.js"></script>
```

### 二、常用图表简单示例

`Charts`的配置项与[Echarts](https://echarts.apache.org/zh/option.html)基本一致，以下主要介绍不同的常用配置项。

#### 1、通用属性设置

> **color**

> **legend**

> **xAxis**

> **yAxis**

> **grid**

#### 2、series设置

> **折线图**

```javascript
"series": [
    {
        "data": [2339, 1899, 2118, 1790, 3265, 4465, 3996],
        "type": "line",
        // 是否光滑
        "smooth": true,
        "lineStyle": {
            // 虚线
            "lineDash": [5, 5]
        },
        // 区域
        "lineArea": {
            "show": true,
            // 渐变填充
            "gradient": ["rgba(55, 162, 218, 0.6)", "rgba(55, 162, 218, 0)"]
        },
        // 显示数值
        "label": {
            "show": true,
            "formatter": "{value} 元"
        },
        // 球点半径
        "linePoint": {
            "radius": 4
        }
    }
]
```

> **柱状图**

**（1）梯形柱状图**

```javascript
"series": [
    {
       "name": "系列A",
       "data": [1200, 2230, 1900, 2100, 3500, 4200, 3985],
       "type": "bar",
       "shapeType": "leftEchelon"          
    },
    {
        "name": "系列B",
        "data": [1200, 2230, 1900, 2100, 3500, 4200, 3985],
        "type": "bar",
        "shapeType": "rightEchelon"
    }
]
```

**（2）背景柱状图**

```javascript
"series": [
    {
        "data": [1200, 2230, 1900, 2100, 3500, 4200, 3985],
        "type": "bar",
        "backgroundBar": {
            "show": true
        }
    }
]
```

**（3）描边柱状图（渐变）**

```javascript
"series": [
    {
        "data": [1200, 2230, 1900, 2100, 3500, 4200, 3985],
        "type": "bar",
        "gradient": {
            "color": ["rgba(251, 114, 147, .6)", "rgba(251, 114, 147, .1)"]
        },
        "barStyle": {
            "stroke": "#fb7293"
        }
    }
]
```

> **柱状图+折线图、折线图+柱状图**

**（1）折线图+柱状图**

```javascript
"series": [
    {
        "name": "人流量",
        "data": [1000, 1200, 900, 1500, 900, 1200, 1000],
        "type": "line",
        "smooth": true,
        "lineArea": {
            "show": true,
            "gradient": ["rgba(251, 114, 147, 1)", "rgba(251, 114, 147, 0)"]
        },
        "lineStyle": {
            "stroke": "rgba(251, 114, 147, 1)",
            "lineDash": [3, 3]
        },
        "linePoint": {
            "style": {
                "stroke": "rgba(251, 114, 147, 1)"
            }
        },
        "yAxisIndex": 1
    },
    {
        "name": "销售额",
        "data": [1500, 1700, 1400, 2000, 1400, 1700, 1500],
        "type": "bar",
        "gradient": {
            "color": ["rgba(103, 224, 227, .6)", "rgba(103, 224, 227, .1)"]
        },
        "barStyle": {
            "stroke": "rgba(103, 224, 227, 1)"
        }
    }
]
```

**（2）柱状图+折线图**

```javascript
"series": [
    {
        "name": "人流量",
        "data": [1000, 1200, 900, 1500, 900, 1200, 1000],
        "type": "line",
        "smooth": true,
        "lineArea": {
            "show": true,
            "gradient": ["rgba(251, 114, 147, 1)", "rgba(251, 114, 147, 0)"]
        },
        "lineStyle": {
            "stroke": "rgba(251, 114, 147, 1)",
            "lineDash": [3, 3]
        },
        "linePoint": {
            "style": {
                "stroke": "rgba(251, 114, 147, 1)"
            }
        },
        "yAxisIndex": 1
    },
    {
        "name": "销售额",
        "data": [1500, 1700, 1400, 2000, 1400, 1700, 1500],
        "type": "bar",
        "gradient": {
            "color": ["rgba(103, 224, 227, .6)", "rgba(103, 224, 227, .1)"]
        },
        "barStyle": {
            "stroke": "rgba(103, 224, 227, 1)"
        }
    }
]
```

> **仪表盘**

**（1）百分比环**

```javascript
"series": [
    {
        "type": "gauge",
        "startAngle": -Math.PI / 2,
        "endAngle": Math.PI * 1.5,
        "arcLineWidth": 25,
        "data": [
            { 
                "name": "itemA", 
                "value": 65, 
                "gradient": ["#03c2fd", "#1ed3e5", "#2fded6"] 
            }
        ],
        "axisLabel": {
            "show": false
        },
        "axisTick": {
            "show": false
        },
        "pointer": {
            "show": false
        },
        "dataItemStyle": {
            "lineCap": 'round'
        },
        "details": {
            "show": true,
            "formatter": "{value}%",
            "style": {
                "fill": "#1ed3e5",
                "fontSize": 35
            }
        }
    }
]
```

**（2）多同心百分比环**

```javascript
"series": [
    {
        "type": "gauge",
        "startAngle": -Math.PI / 2,
        "endAngle": Math.PI * 1.5,
        "arcLineWidth": 10,
        "data": [
            { "name": "A", "value": 25, "gradient": ["#03c2fd", "#1ed3e5", "#2fded6"] },
            { "name": "B", "value": 45, "gradient": ["#03c2fd", "#1ed3e5", "#2fded6"], "radius": "53%" },
            { "name": "C", "value": 65, "gradient": ["#03c2fd", "#1ed3e5", "#2fded6"], "radius": "46%" },
            { "name": "D", "value": 35, "gradient": ["#03c2fd", "#1ed3e5", "#2fded6"], "radius": "39%" },
            { "name": "E", "value": 25, "gradient": ["#03c2fd", "#1ed3e5", "#2fded6"], "radius": "32%" }
        ],
        "axisLabel": {
            "show": false
        },
        "axisTick": {
            "show": false
        },
        "pointer": {
            "show": false
        },
        "dataItemStyle": {
            "lineCap": "round"
        },
        "backgroundArc": {
            "show": false
        },
        "details": {
            "show": true,
            "formatter": "{name}占比{value}%",
            "position": "start",
            "offset": [-10, 0],
            "style": {
                "fill": "#1ed3e5",
                "fontSize": 13,
                "textAlign": "right",
            }
        }
    }
]
```

**参考：**

[Charts](http://charts.jiaminghi.com/)

[Charts Demo](http://charts.jiaminghi.com/example/)

[Charts API](http://charts.jiaminghi.com/config/)