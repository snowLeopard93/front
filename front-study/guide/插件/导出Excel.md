
### 一、xlsx

#### 1、引入

```
npm i file-saver xlsx xlsx-style --save
```

#### 2、ant-design-vue + xlsx

```vue
<template>
  <div>
    <button @click="exportData()" >导出Excel</button>
    <a-table
        id="tableExport"
        :columns="columns"
        :data-source="dataSource"
        :bordered="bordered"
        :scroll="{ x: 900 }"
    />
  </div>
</template>
<script>
import FileSaver from "file-saver";
import XLXS from "xlsx";
import XLXSStyle from "xlsx-style";

export default {
  name: "ExportExcel",
  data() {
    return {
      columns: [
         {
            title: "标题",
            width: "200px",
            dataIndex: "title"
         },
         {
            title: "内容",
            width: "200px",
            dataIndex: "content"
         },
      ],
      dataSource: [
        {
            id: 1, name: "测试标题1", content: "测试内容1"
        },
        {
            id: 1, name: "测试标题2", content: "测试内容2"
        }
      ]
    };
  },
  mounted() {},
  methods: {
    exportData() {
      let et = XLXS.utils.table_to_book(
        // 对应的是 a-table组件的id
        document.querySelector("#tableExport"),
        {
          // 此处处理部分单元格值过长时变成科学计数法的问题
          raw: true,
        }
      );

      let keys = Object.keys(et.Sheets["Sheet1"]);
      keys.forEach((keyItem) => {
          if (
            et.Sheets["Sheet1"][keyItem]) && et.Sheets["Sheet1"][keyItem].v
          ) {
            // 此处做excel单元格的样式设置
            et.Sheets["Sheet1"][keyItem].s = {
              alignment: {
                horizontal: "center",
                vertical: "center",
              },
            };
          }
        });

      let etout = XLXSStyle.write(et, {
        bookType: "xlsx",
        type: "binary",
        // bookSST: true,
        // type: "array",
      });

      try {
        FileSaver.saveAs(
          new Blob([this.s2ab(etout)], {
            type: "application/octet-stream",
          }),
          // new Blob([etout], {
          //   type: "application/octet-stream",
          // }),
          "导出.xlsx"
        );
      } catch (e) {
        console.log(e, etout);
      }
    },
    s2ab(etout) {
      let buf = new ArrayBuffer(etout.length);
      let view = new Uint8Array(buf);
      for (let i = 0; i < etout.length; i++) {
        view[i] = etout.charCodeAt(i) & 0xff;
      }
      return buf;
    },
  },
};
</script>
<style scoped>
</style>
```

#### 3、单元格样式设置

| Style Attribute	| Sub Attributes	| Values |
| ------------- |-------------|------------- |
| fill	| patternType		| "solid" or "none"	|
| 	| fgColor	| COLOR_SPEC	|
| 	| bgColor		| COLOR_SPEC
| font		| name		| "Calibri" // default
| 	| sz	| 	"11" // font size in points
| 	| color		| COLOR_SPEC
| 	| bold	| 	true || false
| 	| underline		| true || false
| 	| italic	| 	true || false
| 	| strike		| true || false
| 	| outline		| true || false
| 	| shadow		| true || false
| 	| vertAlign		| true || false
| 	numFmt			| 	| "0" // integer index to built in formats, see StyleBuilder.SSF property
| 	| 	| "0.00%" 	// string matching a built-in format, see StyleBuilder.SSF
| 	| 	| "0.0%" 	// string specifying a custom format
| 	| 	| "0.00%;	\\(0.00%\\);\\-;@" // string specifying a custom format, escaping special characters
| 	| 	| "m/dd/yy"  // string a date format using Excel's format notation
| alignment		|  vertical	| 	"bottom"||"center"||"top"
| 	| horizontal 		| "bottom"||"center"||"top"
| 	| wrapText		| 	true || false
| 	| readingOrder		| 	2 // for right-to-left
| 	| textRotation		|  Number from 0 to 180 or 255 (default is 0)
| 	| 	| 90 is rotated up 90 degrees
| 	| 	| 45 is rotated up 45 degrees
| 	| 	| 135 is rotated down 45 degrees
| 	| 	| 180 is rotated down 180 degrees
| 	| 	| 255 is special, aligned vertically
| border	| 	top		| { style: BORDER_STYLE, color: COLOR_SPEC }
| 	| bottom		| { style: BORDER_STYLE, color: COLOR_SPEC }
| 	| left		| { style: BORDER_STYLE, color: COLOR_SPEC }
| 	| right		| { style: BORDER_STYLE, color: COLOR_SPEC }
| 	| diagonal	| 	{ style: BORDER_STYLE, color: COLOR_SPEC }
| 	| diagonalUp	| 	true||false
| 	| diagonalDown	| 	true||false

#### 4、问题处理

**（1）引用xlsx-style组件时 提示:This relative module was not found: ./cptable in ./node_modules/xlsx-style**

**解决：**

```vue.config.js
module.exports = {
  configureWebpack: (config) => {
    config.externals = [
      {
        "./cptable": "var cptable",
      },
    ];
  },
}
```

**（2）合并单元格样式丢失**

**解决：**

```vue
<template>
</template>
<script>
  export default {
    name: "ExportExcel",
    data() {
      return {},
    },
    methods: {
      // 导出数据
      exportData() {
        let et = XLSX.utils.table_to_book(
          document.querySelector("#tableExport"),
          {
            raw: true,
          }
        );

        let columnsConfig = [];
        const columnsLength = this.columns && this.columns[0].children.length;
        for (let i = 0; i < columnsLength; i++) {
          columnsConfig.push({ wch: "50" });
        }

        let sheet = this.getSheetBySheetName(et, "Sheet1");
        sheet["!cols"] = columnsConfig;
        let keys = Object.keys(sheet);
        if (isNotEmpty(keys)) {
          keys.forEach((keyItem) => {
            if (
              isNotEmpty(sheet[keyItem]) &&
              !isNullOrUndefined(sheet[keyItem].v)
            ) {
              if (firstRowRE.test(keyItem) || secondRowRE.test(keyItem)) {
                sheet[keyItem].s = {
                  font: {
                    bold: true,
                  },
                  alignment: {
                    horizontal: "center",
                    vertical: "center",
                  },
                  border: {
                    top: { style: "thin" },
                    bottom: { style: "thin" },
                    left: { style: "thin" },
                    right: { style: "thin" },
                  },
                };
              } else {
                sheet[keyItem].s = {
                  alignment: {
                    horizontal: "center",
                    vertical: "center",
                  },
                  border: {
                    top: { style: "thin" },
                    bottom: { style: "thin" },
                    left: { style: "thin" },
                    right: { style: "thin" },
                  },
                };
              }
            }
          });
        }

        this.setMergeColumnStyle(et, "Sheet1");
        let etout = XLSXStyle.write(et, {
          bookType: "xlsx",
          type: "binary",
        });

        let excelName = (this.currentTemplate.excelName || "导出") + ".xlsx";
        try {
          FileSaver.saveAs(
            new Blob([this.s2ab(etout)], {
              type: "application/octet-stream",
            }),
            excelName
          );
        } catch (e) {
          console.log(e, etout);
        }
      },
      getSheetBySheetName(et, sheetName) {
        let sheet = et.Sheets[sheetName];
        return sheet;
      },
      // 处理合并单元格样式
      setMergeColumnStyle(et) {
        let cols = [
          "A",
          "B",
          "C",
          "D",
          "E",
          "F",
          "G",
          "H",
          "I",
          "J",
          "K",
          "L",
          "M",
          "N",
          "O",
          "P",
          "Q",
          "R",
          "S",
          "T",
          "U",
          "V",
          "W",
          "X",
          "Y",
          "Z",
        ];
        let sheet = this.getSheetBySheetName(et, "Sheet1");
        let merges = sheet["!merges"];
        merges.forEach((mergeItem) => {
          // s 表示 start, c 表示 end
          // c 表示 column, r 表示 row
          for (let i = mergeItem.s.c; i <= mergeItem.e.c; i++) {
            if (!sheet[`${cols[i]}` + `${mergeItem.s.r + 1}`]) {
              sheet[`${cols[i]}` + `${mergeItem.s.r + 1}`] = {
                s: {
                  border: {
                    top: { style: "thin" },
                    bottom: { style: "thin" },
                    left: { style: "thin" },
                    right: { style: "thin" },
                  },
                },
              };
            }
          }
        });
      }
    }
  }
</script>
```

**参考：**

[npm xlsx](https://www.npmjs.com/package/xlsx)

[npm xlsx-style](https://www.npmjs.com/package/xlsx-style)