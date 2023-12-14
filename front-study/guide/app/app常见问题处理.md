
### 1、vue项目监听手机物理返回键和虚拟返回键

在`main.js`中添加如下代码：

```javascript
// 监听手机物理返回键和虚拟返回键
const plusReady = (callback) => {
  if (window.plus) {
    callback();
  } else {
    document.addEventListener('plusready', callback);
  }
};

plusReady(() => {
  const { plus, history } = window;
  console.log('plus', plus);
  let firstBack = 0;
  plus.key.addEventListener('backbutton', () => {
    const currentWebView = plus.webview.currentWebview();
    currentWebView.canBack((evt) => {
      if (currentWebView.id === plus.runtime.appid) {
        if (evt.canBack) {
          // eslint-disable-next-line no-restricted-globals
          history.back();
        } else if (!firstBack) {
          firstBack = new Date().getTime();
          plus.nativeUI.toast('再按一次退出应用');
          setTimeout(() => {
            firstBack = 0;
          }, 2000);
        } else if (new Date().getTime() - firstBack < 2000) {
          plus.runtime.quit();
        }
      } else if (evt.canBack) {
        // eslint-disable-next-line no-restricted-globals
        console.log('history', history);
        // eslint-disable-next-line no-restricted-globals
        history.back();
      } else {
        currentWebView.close('auto');
      }
    });
  });
});
```