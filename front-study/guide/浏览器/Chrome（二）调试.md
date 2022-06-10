### 一、Console面板调试

#### 1、常用console命令

> **console.log() & console.warn()**

> **console.dir()**

与`console.log()`的差异在于：当使用`console.log()`打印时可以看到方便地看到dom结构，使用`console.dir()`则会打印出dom节点下的所有属性信息。

> **console.table()**

将数据以表格的形式输出。

> **console.assert()**

该方法接收多个参数，其中第一个参数为输入的表达式，只有在该表达式的值为false时，才会将剩余的参数输出到控制台中。

> **console.trace()**

该方法用于在控制台中显示当前代码在堆栈中的调用路径，通过这个调用路径我们可以很容易地在发生错误时找到原始错误点。

> **console.count()**

该方法相当于一个计数器，用于记录调用次数，并将记录结果打印到控制台中。其接收一个可选参数`console.count(label)`，label表示指定标签，该标签会在调用次数之前显示。

> **console.time() & console.timeEnd()**

这两个方法一般配合使用，是JavaScript中用于跟踪程序执行时间的专用函数，`console.time`方法是作为计算的起始时间，`console.timeEnd`是作为计算的结束时间，并将执行时长显示在控制台。如果一个页面有多个地方需要使用到计算器，则可以为方法传入一个可选参数label来指定标签，该标签会在执行时间之前显示。

> **console.group() & console.groupEnd()**

对数据信息进行分组，其中`console.group()`方法用于设置分组信息的起始位置，该位置之后的所有信息将写入分组，`console.groupEnd()`方法用于结束当前的分组。

#### 2、其他常用命令

> **copy()**

`copy()`用来将断点过程中的值拷贝出来。

### 二、Sources面板调试

#### 1、断点时查看是否存在闭包

###  三、常用快捷键

**Ctrl-o** 打开某一文件

**Ctrl-g** 定位到某一行

**Ctrl-h** 打开历史记录

**Ctrl-shift-delete** 清除缓存

**Shift-Esc** 打开Chrome任务管理器

**待补充~~**