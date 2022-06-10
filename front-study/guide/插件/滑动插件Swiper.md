
### 一、安装

#### 1、`npm`安装

```
npm i swiper --save
```

### 二、引入

#### 1、script标签引入

```html
<link rel="stylesheet" href="https://cdn.bootcdn.net/ajax/libs/Swiper/6.7.5/swiper-bundle.min.css">

<script src="https://cdn.bootcdn.net/ajax/libs/Swiper/6.7.5/swiper-bundle.min.js"></script>
```

#### 2、import引入

```
import Swiper from 'swiper';
```

### 三、初始化

**以6.7.5为例**

#### 1、示例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Swiper demo</title>
    <meta name="viewport" content="width=device-width, initial-scale=1, minimum-scale=1, maximum-scale=1">

    <!-- Link Swiper's CSS -->
    <link rel="stylesheet" href="https://cdn.bootcdn.net/ajax/libs/Swiper/6.7.5/swiper-bundle.min.css">

    <!-- Demo styles -->
    <style>
    html, body {
        position: relative;
        height: 100%;
    }
    body {
        background: #eee;
        font-family: Helvetica Neue, Helvetica, Arial, sans-serif;
        font-size: 14px;
        color:#000;
        margin: 0;
        padding: 0;
    }
    .swiper-container {
        width: 100%;
        height: 100%;
        margin-left: auto;
        margin-right: auto;
    }
    .swiper-slide {
        text-align: center;
        font-size: 18px;
        background: #fff;

        /* Center slide text vertically */
        display: -webkit-box;
        display: -ms-flexbox;
        display: -webkit-flex;
        display: flex;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        -webkit-justify-content: center;
        justify-content: center;
        -webkit-box-align: center;
        -ms-flex-align: center;
        -webkit-align-items: center;
        align-items: center;
    }
    </style>
</head>
<body>
    <!-- Swiper -->
    <div class="swiper-container">
        <div class="swiper-wrapper">
            <div class="swiper-slide">Slide 1</div>
            <div class="swiper-slide">Slide 2</div>
            <div class="swiper-slide">Slide 3</div>
            <div class="swiper-slide">Slide 4</div>
            <div class="swiper-slide">Slide 5</div>
            <div class="swiper-slide">Slide 6</div>
            <div class="swiper-slide">Slide 7</div>
            <div class="swiper-slide">Slide 8</div>
            <div class="swiper-slide">Slide 9</div>
            <div class="swiper-slide">Slide 10</div>
        </div>
        <!-- Add Pagination -->
        <div class="swiper-pagination"></div>
        <!-- Add Arrows -->
        <div class="swiper-button-next"></div>
        <div class="swiper-button-prev"></div>
    </div>

    <!-- Swiper JS -->
    <script src="https://cdn.bootcdn.net/ajax/libs/Swiper/6.7.5/swiper-bundle.min.js"></script>

    <!-- Initialize Swiper -->
    <script>
    new Swiper('.swiper-container', {
        loop: false,
        observer: true,
        autoplay: {
            delay: 5000,
            disableOnInteraction: false
        },
        pagination: {
            el: '.swiper-pagination',
            clickable: true,
        },
        navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev',
        }
    });
    </script>
</body>
</html>
```

#### 2、效果

![Swiper基础效果](../../images/插件/swiper-basic.gif)

### 四、常用参数和常见问题

#### 1、常用参数

```javascript
new Swiper('.swiper-container', {
    // 设置是否循环轮播
    // 注意：如果是循环轮播的话，则会将块全部都拷贝一份，用于循环轮播时切换显示
    loop: false,
    // 用来控制轮播时滑动的时长
    spped: 500,
    // 用来监听子节点数据变化
    // 如原先轮播的有5个块，定时器刷新之后只有4个块
    // 如果 observer 值为true，则会自动更新轮播的块
    observer: true,
    // 用来指明滑动方向，默认是水平方向，vertical 表明是垂直方向
    direction: 'vertical',
    autoplay: {
        // 用于设置 鼠标悬浮在Swiper上方时，暂停轮播
        pauseOnMouseEnter: true,
        delay: 5000,
        // 用于控制 手动翻页之后，会继续自动翻页
        disableOnInteraction: false
    },
    // 翻页的小圆点
    pagination: {
        el: '.swiper-pagination',
        clickable: true,
    },
    // 翻页箭头
    navigation: {
        nextEl: '.swiper-button-next',
        prevEl: '.swiper-button-prev',
    }
});
```

### 五、vue + swiper

#### 1、问题

**（1）引入不生效（6.8.3）**

修改引入如下：

```
import Swiper from 'swiper/swiper-bundle.min.js';
import 'swiper/swiper-bundle.min.css';
```

**（2）动态数据渲染时不能滑动**

将`swiper`创建放到数据渲染，`$nextTick`触发之后执行。

```
new Swiper('.swiper-container', {
    loop: false,
    observer: true,
    speed: 1000,
    autoplay: {
        // 控制鼠标悬浮在swiper上方的时候停止轮播
        pauseOnMouseEnter: true,
        delay: 10000,
        disableOnInteraction: false
    },
    pagination: {
        el: '.swiper-pagination',
        clickable: true,
    },
    navigation: {
        nextEl: '.swiper-button-next',
        prevEl: '.swiper-button-prev',
    }
});
```

**参考：**

[Swiper](https://www.swiper.com.cn/)

[Swiper API](https://www.swiper.com.cn/api/)

[Swiper demo](https://www.swiper.com.cn/demo/index.html)