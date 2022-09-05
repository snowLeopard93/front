### 一、常见问题

#### 1、上传图片之后删除图片，无法再次上传同一图片

在最新版本中，该问题已修复。如果不能升级wangEditor版本，需要处理该问题，可以尝试如下方法：

（1）定义`uploadImgHooks`的`customInsert`方法，

（2）在执行`insertImg`方法之后，遍历`editor.menus.menuList`，判断是`image`时，将`input`输入框的值清空。

```js
const editor = new wangEditor("#wangEditorToolbar_content1", "#wangEditorContent_content1");
editor.config.showLinkImg = false;
editor.config.uploadImgHooks = { //使用监听函数在上传图片的不同阶段做相应处理
    // insertImg 是插入图片的函数，editor 是编辑器对象，result 是服务器端返回的结果
    customInsert: function (insertImg, result, editor) {
        const url = result.data;
        insertImg(url);
        // 处理 上传图片之后删除图片，无法再次上传同一图片的问题
        const list = editor.menus.menuList
        for (let i = 0; i < list.length; i++) {
            if (list[i].key === 'image') {
                const el = list[i].$elem.elems[0].querySelector('input')
                if (el) {
                    el.value = ''
                }
                break
            }
        }
    }
};
```

[github issue](https://github.com/wangeditor-team/wangEditor/issues/3041)

[github Fix upload img](https://github.com/wangeditor-team/wangEditor/pull/3105/commits/04ec0a7325a762365e33a6e7622e96308c5e337e)