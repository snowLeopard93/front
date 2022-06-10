### 一、前置

#### 1、Chartjs引入

> **通过script标签引入**

```html
<script type="text/javascript" src="https://cdnjs.com/libraries/Chart.js"></script>
```

> **通过require引入**

```javascript
// 先将 Chart.min.js 放入到 lib 目录下，然后再通过require引入
require([
    "lib/Chart.min"
], function (Chart) {
    // ......
})
```

#### 2、Chartjs基本语法

```javascript
var ctx = document.getElementById('canvas').getContext('2d');
new Chart(ctx, {
    "type": "line",
    "data": {
        "labels": [],
        "datasets": []
    },
    "options": {
    
    }   
});
```

### 二、常用图表简单示例

#### 1、通用属性设置

> **图例**

> **悬浮提示信息**

> **x轴（图例）**

```javascript
"data": {
    "labels": ["January", "February", "March", "April", "May", "June"]
}
```

#### 2. 数据集设置

> **折线图**

```javascript
"data": {
    "datasets": [{
        // 图例名称
        "label": "My First dataset",
        // y轴数据
        "data": [10, 30, 50, 20, 25, 44],
         // 是否填充阴影
         "fill": false,
        // 阴影区域颜色
        "backgroundColor": "rgb(255, 99, 132)",
        // 线条颜色
        "borderColor": "rgb(255, 99, 132)"
    }]
}
```

> **柱状图**

```javascript
"datasets": [{
    // 图例名称
    "label": "My First dataset",
    // y轴数据
    "data": [10, 30, 50, 20, 25, 44],
    // 柱状图填充色 
    "backgroundColor": "rgb(255, 99, 132)",
    // 柱状图悬浮时的填充色
    "hoverBackgroundColor": "rgb(255, 159, 64)"
}]
```

> **环形图（饼图）**

```javascript
"datasets": [{
    "data": [10, 20, 30],
    "backgroundColor": ["rgb(255, 99, 132)", "rgb(54, 162, 235)", "rgb(255, 205, 86)"]
}]
```
#### 3、options设置

> **有坐标轴**

```javascript
"options": {
    "scales": {
        "xAxes": [
            {
                 // 是否堆叠
                "stacked": true
            }
        ],
        "yAxes": [
            {
                // 是否堆叠
                "stacked": true
            }
        ]
   }
}
```

> **无坐标轴**

**（1）环形图**

```javascript
"options": {
    // 环形图百分比
    "cutoutPercentage": 50
}
```

**参考**

[Chart.js](https://www.chartjs.org/)

[Chart.js 文档](http://chartjs.cn/docs/)

[chartjs API](https://chartjs.bootcss.com/docs/)