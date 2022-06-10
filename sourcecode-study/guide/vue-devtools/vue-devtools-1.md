### 第一期 | vue-devtools

【若川】vue-devtools源码解读：https://juejin.cn/post/6959348263547830280

【纪年】vue-devtools源码解读：https://www.yuque.com/ruochuan12/group9/hcgcnh

本篇源码笔记是第一期，通过阅读【若川】和【纪年】写的源码笔记，然后尝试逐步断点调试代码，初步熟悉了阅读源码的步骤和思路。

### 一、源码阅读

#### 1、在编辑器中打开组件

`node_modules/@vue/cli-service/lib/commands/serve.js`

`node_modules/launch-editor-middleware/index.js`的`launchEditorMiddleWare`的方法

```javascript
srcRoot = process.cwd()

// req.url => /?file=src/components/HelloWorld.vue
const { file } = url.parse(req.url, true).query || {}

// file => src/components/HelloWorld.vue

if(!file) {
    ...
} else {
    // srcRoot => E:\\github\\open-in-editor\\vue3-project
    // path.resolve(srcRoot, file) => 
    // E:\\github\\open-in-editor\\vue3-project\\src\\components\\HelloWorld.vue
    launch(path.resolve(srcRoot, file), specifiedEditor, onErrorCallback)
}
```

`node_modules/launch-editor/index.js`的`launch`方法

```javascript
// 用正则将传入的文件名解析成 fileName、lineNumber和columnNumber
const parsed = parsedFile(file);
let { fileName } = parsed
const { lineNumber, columnNumber } = parsed
// 判断文件是否存在
if(!fs.existsSync(fileName)) {
    return;
}

if(typeof specifiedEditor === "function") {
    onErrorCallback = specifiedEditor;
    specifiedEditor = null;
}

onErrorCallback = wrapErrorCallback(onErrorCallback);
// 猜测编辑器类型
const [editor, ...args] = guessEditor(specifiedEditor);
// 如果编辑器不存在，则抛出错误，返回
if(!editor) {
    onErrorCallback(fileName, null);
    return;
}

// 判断_childProcess存在且是终端编辑器
// _chidProcess值为null，则跳过
if(_childProcess && isTerminalEditor(editor)) {
    _childProcess.kill('SINKILL');
}

if(process.platform === "win32") {
    // 进入此处
    _childProcess = childProcess.spawn(
        ...
    )
} else {
    _childProcess = childProcess.spawn(
        ...
    )
}

```

`node_modules/launch-editor/index.js`的`wrapErrorCallback`方法

```javascript
// 传入文件名，然后抛出错误信息
```

`node_modules/launchguess.js`的`guessEditor`方法

```javascript
// process.platform === "darwin"
// return [COMMON_EDITORS_OSX[processName]]

// process.platform === "win32" 
// return [fullProcessPath]
// D:\\Program Files\\Microsoft VS Code\\Code.exe

// process.platform === "linux"
// return [COMMON_EDITORS_OSX[processName]]
```

**推荐：**

[vue-devtools 在编辑器中打开组件](https://github.com/vuejs/vue-devtools/blob/dev/docs/open-in-editor.md)

[lxchuan12/open-in-editor](https://github.com/lxchuan12/open-in-editor)