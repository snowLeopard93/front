
### 一、引入X6

#### 1、npm安装

```
npm install @antv/x6 --save
```

#### 2、script标签引入
```
<script src="https://unpkg.com/@antv/x6/dist/x6.js"></script>
```

### 二、初始化设置

```javascript
let graph = new X6.Graph({
    container: document.getElementById(domId),
    // 设置画布宽高
    // width: 6896,
    // height: 2900,
    // 是否异步渲染
    async: true,
    // 网格线
    grid: true,
    // 对齐线
    snapline: {
        enabled: true
    },
    connecting: {
        // 连接桩自动吸附
        snap: true,
    },
    // 开启平移、拖拽等
    scroller: {
        enabled: true,
        pannable: true,
    },
    selecting: {
        enabled: true,
        rubberband: true, // 启用框选
        showNodeSelectionBox: true
    },
    keyboard: {
        enabled: true,
    },
    // 对节点进行放大缩小操作
    resizing: true,
    // 背景图片
    // background: {
    //     image: "",
    //     position: "center",
    //     size: {
    //         width: 6896,
    //         height: 2900
    //     },
    //     repeat: "no repeat",
    //     opacity: 1
    // }
    // 忽略某个鼠标事件，比如只能平移拖拽，点和线之间的相对位置不变
    guard: function() {
        return true;
    },
    // 开启撤销
    history: {
        enabled: true,
        // ignoreAdd: true,
        // 移除操作不记录到撤销的队列中
        ignoreRemove: true,
        beforeAddCommand(event, args) {
            if(args.key === "tools") {
                return false;
            }
        },
    },
    // 开启嵌套节点（即某个节点拖拽到另一个节点里面时，将其设置为它的子节点）
    embedding: {
        enabled: true,
        findParent({ node }) {
            const bbox = node.getBBox();
            return this.getNodes().filter((a) => {
                const data = a.getData();
                if (data && data.parent) {
                    const targetBBox = a.getBBox();
                    return bbox.isIntersectWithRect(targetBBox)
                }
                return false
            })
        },
    },
    // 限制子节点的移动范围
    translating: {
        restrict(view) {
            const cell = view.cell;
            if (cell.isNode()) {
                const parent = cell.getParent();
                if (parent) {
                    return parent.getBBox()
                }
            }
            return null
        },
    },
    // 开启鼠标缩放
    mousewheel: {
        enabled: true,
        modifiers: ['ctrl', 'meta'],
        minScale: 0.5,
        maxScale: 2,
    },
});
```

### 三、graph

#### 1、冻结、解除冻结

**注：适用于需要新增大量的节点或连线时使用**

```javascript
// 冻结
graph.freeze();
// 此处可进行新增节点、连线的操作
// ......
// 解除冻结
graph.unfreeze();
```

**参考：**

[async freeze](https://x6.antv.vision/zh/docs/api/graph/view#async)

#### 2、缩放

```javascript
graph.zoomTo(5);
```

#### 3、居中显示

```javascript
graph.scrollToContent();
```

**参考：**

[graph scroller](https://x6.antv.vision/zh/docs/api/graph/scroller)

#### 4、撤销

需要配合`graph-history`一起使用。

```javascript
graph.undo();
```

**参考：**

[graph history](https://x6.antv.vision/zh/docs/api/graph/history)

[graph mousewheel](https://x6.antv.vision/zh/docs/api/graph/mousewheel)

[graph](https://x6.antv.vision/zh/docs/api/graph/graph)

### 四、节点

#### 1、添加节点

```javascript
graph.addNode({
    id: "1",
    label: "节点",
    x: 100,
    y: 100,
    width: 50,
    height: 50,
})
```

#### 2、修改节点

**（1）修改宽高**

**（2）修改label**

#### 3、删除节点

```javascript
graph.removeNode(node);
```

#### 4、连接桩

**（1）简单连接桩**

```javascript
graph.addNode({
    x: 100,
    y: 100,
    width: 100,
    height: 100,
    ports: {
        groups: {
            // 输入链接桩群组定义
            left: {
                position: 'left',
                attrs: {
                    circle: {
                        r: 6,
                        magnet: true,
                        stroke: '#31d0c6',
                        strokeWidth: 2,
                        fill: '#fff',
                        style: {
                            visibility: 'hidden',
                        },
                    },
                },
            },
            // 输出链接桩群组定义
            right: {
                position: 'right',
                    attrs: {
                        circle: {
                            r: 6,
                            magnet: true,
                            stroke: '#31d0c6',
                            strokeWidth: 2,
                            fill: '#fff',
                            style: {
                                visibility: 'hidden',
                            },
                        },
                    },
                },
                top: {
                    position: 'top',
                    attrs: {
                        circle: {
                            r: 6,
                            magnet: true,
                            stroke: '#31d0c6',
                            strokeWidth: 2,
                            fill: '#fff',
                            style: {
                                visibility: 'hidden',
                            },
                        },
                    },
                },
                bottom: {
                    position: 'bottom',
                    attrs: {
                        circle: {
                            r: 6,
                            magnet: true,
                            stroke: '#31d0c6',
                            strokeWidth: 2,
                            fill: '#fff',
                            style: {
                                visibility: 'hidden',
                            },
                        },
                    },
                },
            },
        items: [
            {
                id: 'port1',
                group: 'left',
            },
            {
                id: 'port2',
                group: 'left',
            },
            {
                id: 'port3',
                group: 'right',
            },
            {
                id: 'port4',
                group: 'right',
            },
            {
                id: 'port5',
                group: 'top',
            },
            {
                id: 'port6',
                group: 'top',
            },
            {
                id: 'port7',
                group: 'bottom',
            },
            {
                id: 'port8',
                group: 'bottom',
            },
        ]
    }
});
```

**（2）连接桩隐藏显示：**

```javascript
graph.on('node:mouseenter', ({ cell }) => {
    // 连接桩显示
    const ports = document.getElementById(domId).querySelectorAll('.x6-port-body');
    for (let i = 0, len = ports.length; i < len; i = i + 1) {
        ports[i].style.visibility = 'visible';
    }
});

graph.on('node:mouseleave', ({ cell }) => {
    // 连接桩隐藏
    const ports = document.getElementById(domId).querySelectorAll('.x6-port-body');
    for (let i = 0, len = ports.length; i < len; i = i + 1) {
        ports[i].style.visibility = 'hidden';
    }
});
```

**参考：**

[node ports](https://x6.antv.vision/zh/docs/api/model/node#ports)

### 五、边

**注意：** 边需要依赖于节点存在。

#### 1、添加边

```javascript
const { Shape } = X6;
let edge = new Shape.Edge({
    source: sourceNodeId, // String，必须，起始节点 id
    target: targetNodeId, // String，必须，目标节点 id
    markup: [
        {
            tagName: 'path',
            selector: 'stroke',
        }
    ],
    attrs: {
        stroke: {
            fill: 'none',
            stroke: '#99cce7',
            connection: true,
            strokeWidth: 1,
        },
    },
});
graph.addEdge(edge);
```

#### 2、修改边

#### 3、删除边

### 六、元素

#### 1、删除元素

```javascript
graph.removeCell(cell);
```

#### 2、获取选中的元素

```javascript
graph.getSelectedCells();
```

#### 3、获取全部元素

```javascript
graph.getCells();
```

#### 4、获取子元素

```javascript
graph.getCells().forEach(item => {
    let children = item.getChildren();
    console.log(children);
});

```
#### 5、常用方法

**（1）判断是否是节点**

```javascript
cell.isNode()
```

**（2）判断是否是边**

```javascript
cell.isEdge()
```

### 七、事件

#### 1、节点单击

```javascript
graph.on('node:click', ({ e, node, view }) => {
});
```

#### 2、节点双击

```javascript
graph.on('node:dblclick', ({ e, node, view }) => {
});
```

#### 3、鼠标移入

```javascript
graph.on('cell:mouseenter', ({ cell }) => {
});
```

#### 4、鼠标移出

```javascript
graph.on('cell:mouseleave', ({ cell }) => {
});
```

#### 5、绑定键盘事件

```javascript
let graph = new X6.Graph({
    container: document.getElementById(domId),
    keyboard: {
        enabled: true,
    }
});

graph.bindKey('enter', (e) => {
    console.log("enter");
});

graph.bindKey('delete', (e) => {
    console.log("delete");
    //可以在此处调用删除节点或边的方法
});

graph.bindKey('backspace', (e) => {
    console.log("backspace");
});
```

#### 6、节点移除

```javascript
graph.on('node:removed', ({ node, index, o }) => {
    console.log("remove node");
});
```

**参考：**

[antV-X6-事件系统](https://x6.antv.vision/zh/docs/tutorial/intermediate/events)

[antV-X6-keyboard](https://x6.antv.vision/zh/docs/api/graph/keyboard#bindkey)

### 八、工具箱

#### 1、节点工具

```javascript
node.addTools([
    {
        name: 'boundary',
        args: {
            attrs: {
                fill: '#7c68fc',
                stroke: '#333',
                stroke-width: 1,
                fill-opacity: 0.2,
            },
        },
    },
    {
        name: 'button-remove',
        args: {
            x: 0,
            y: 0,
            offset: { x: 10, y: 10 },
        },
    },
])
```

#### 2、边工具

```javascript
edge.addTools([
    {
        name: 'boundary',
        args: {
            attrs: {
                fill: '#7c68fc',
                stroke: '#333',
                stroke-width: 1,
                fill-opacity: 0.2,
            },
        },
    },
    {
        name: 'button-remove',
        args: {
            x: 0,
            y: 0,
            offset: { x: 10, y: 10 },
        },
    },
    {
        // 路径点工具
        name: 'vertices'
    },
    {
        // 线段工具
        name: 'segments'
    }
])
```

#### 3、（元素）移除工具

```javascript
cell.removeTools();
```

**参考**

[antV-X6](https://x6.antv.vision/zh/docs/tutorial/about)

[antV-X6图表示例](https://x6.antv.vision/zh/examples)

[antV-使用工具](https://x6.antv.vision/zh/docs/tutorial/intermediate/tools)

[antV-X6图表示例-流程编排](https://x6.antv.vision/zh/examples/showcase/practices#flowchart)