
### 一、常用标签

#### 1、video标签

|  **属性**    | **作用**     |
| ------------- |-------------|
| **autoplay** | 是否自动播放 |
| **loop** | 是否循环播放 |

**示例：**

```html
<video muted controls="controls" autoplay="autoplay" loop="loop">
	<source src="video/1.webm" type="video/webm"/>
</video>
```

#### 2、audio标签

|  **属性**    | **作用**     |
| ------------- |-------------|
| **autoplay** | 是否自动播放 |
| **loop** | 是否循环播放 |
| **src** | 文件路径 |

```html
<audio id="audioDemo" src="music/1.mp3" autoplay="autoplay" loop="loop"></audio>
```

```javascript
var audio = document.getElementById('audioDemo');
if(audio !== null) {
    if (!audio.paused) {
        audio.pause();
    } else {
        audio.play();
    }
}
```

#### 3、marquee标签（已废弃）

|  **属性**    | **作用**     |
| ------------- |-------------|
| **direction** | 方向，可选值有left、right、up、down |
| **scrolldelay** | 滚动延时 |
| **height** | 高度，方向为up、down的时候需要设置 |'
| **loop** | 循环次数 |

```html
<marquee id="mar" direction="up" scrolldelay="300" height="300px" loop="-1">
    <div class="tr">
        <div class="td" title="aaa">aaa</div>
        <div class="td" title="bbb">bbb</div>
        <div class="td" title="ccc">ccc</div>
    </div>
    <div class="tr">
        <div class="td" title="aaa">aaa</div>
        <div class="td" title="bbb">bbb</div>
        <div class="td" title="ccc">ccc</div>
    </div>
    <div class="tr">
        <div class="td" title="aaa">aaa</div>
        <div class="td" title="bbb">bbb</div>
        <div class="td" title="ccc">ccc</div>
    </div>
</marquee>
```

```javascript
// 鼠标放入时停止滚动
$("#mar").mouseover(function() {
    this.stop();
});
// 鼠标放入时继续滚动
$("#mar").mouseout(function() {
    this.start();
});
```

**推荐：**

[video](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video)

[audio](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/audio)

[marquee](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/marquee)
