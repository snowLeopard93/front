
### 一、常用属性

`list-style`

```css
/* 可用于有序列表 标序号 */
list-style: decimal;

/* 可用于在列表前面添加 图片等 */
list-style: square inside url('eg_arrow.gif')
```

### 二、flex布局

#### 1、常用属性

##### `flex-direction`

可选值有 `row` `column` `row-reverse` `column-reverse`

##### `justify-content`

可选值有 `flex-start` `flex-end` `center` `space-between` `space-around`

`center` 表示居中显示

`space-between` 表示两端对齐，中间间隔均匀分布

`space-around` 表示每个块之间的左右间隔相等

##### `align-items`

可选值有 `flex-start` `flex-end` `center` `baseline` `stretch`


### 三、常见效果

#### 1、省略号

**（1）单行省略号**

```css
.ellipsis {
    overflow: hidden;
    text-overflow: ellipsis;
    word-break: keep-all;
    white-space: nowrap;
}
```

**（2）双行省略号**

```css
.double-ellipsis {
    overflow: hidden;
    text-overflow: ellipsis;
    word-break: break-all;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
}
```

#### 2、浏览器滚动条

```css
::-webkit-scrollbar {
    width: 10px;
    height: 10px;
    background-color: #f5f5f5;
}

/* 定义滚动条轨道 内阴影+圆角 */
::-webkit-scrollbar-track {
    box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
    -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
    border-radius: 10px;
    background-color: #f5f5f5;
}

/* 定义滑块 内阴影+圆角 */
::-webkit-scrollbar-thumb {
    box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
    -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
    border-radius: 10px;
    background-color: #bdbdbd;
}

/* 火狐美化滚动条 */
* {
    scrollbar-color: #c8d2e0 #f3f4f9;
    /* 滑块颜色  滚动条背景颜色 */
    scrollbar-width: thin;
    /* 滚动条宽度有三种：thin、auto、none */
}
```

#### 3、渐变色

```
linear-gradient(direction, color1, stop1, color2, stop2,...);
```

其中：

`direction`可选值有

（1）`to top`, `to bottom`, `to left`, `to right`等。

（2）具体的`deg`值，如`270deg`等。

```css
.linear-gradient {
  background-image: linear-gradient(
    to left,
    rgba(6, 30, 61, 0.4),
    300px,
    #4c1113
  );
}
```

### 四、动画

#### 1、线条效果

```css
.step-arrow {
    animation: arrow-animation 3s linear infinite;
}

@keyframes arrow-animation {
    0%   {
        background: linear-gradient(to left, #9CD4FE, 130px, #5AA8FC);
    }
    25%   {
        background: linear-gradient(to left, #9CD4FE, 97.5px, #5AA8FC);
    }
    50%   {
        background: linear-gradient(to left, #9CD4FE, 65px, #5AA8FC);
    }
    75%   {
        background: linear-gradient(to left, #9CD4FE, 32.5px, #5AA8FC);
    }
    100% {
        background: linear-gradient(to left, #9CD4FE, 0px, #5AA8FC);
    }
}
```

#### 2、气泡呼吸效果

```css
.step-circle:after {
    content: "";
    display: inline-block;
    position: absolute;
    width: 80px;
    height: 80px;
    background: linear-gradient(to top, #95DB57, 40px, #E4F99C);
    -moz-border-radius: 80px;
    -webkit-border-radius: 80px;
    border-radius: 80px;
    animation: breathe 1.1s infinite;
}

@keyframes breathe {
    0%   {
        transform: scale(0.99);
    }
    50%   {
        transform: scale(1.03);
    }
    100%   {
        transform: scale(0.99);
    }
}
```

### 五、图形

#### 1、圆形

```css
.step-circle:after {
    content: "";
    display: inline-block;
    position: absolute;
    width: 80px;
    height: 80px;
    background: linear-gradient(to top, #95DB57, 40px, #E4F99C);
    -moz-border-radius: 80px;
    -webkit-border-radius: 80px;
    border-radius: 80px;
}
```

#### 2、矩形

```css
.step-rectangle:after {
    content: "";
    display: inline-block;
    position: absolute;
    width: 150px;
    height: 80px;
    background: linear-gradient(to top, #5AA8FC, 40px, #BDE6FE);
    border-radius: 8px;
}
```