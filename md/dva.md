# dva脚手架
[官网](https://dvajs.com/)

### 全局安装
`$ npm install dva-cli -g`

### 建立脚手架
`$ dva new projectName（项目名）`

### 安装 antd（按需加载）
通过 npm 安装 antd 和 babel-plugin-import 。babel-plugin-import 是用来按需加载 antd 的脚本和样式的

`$ cd projectName`
`$ npm install antd babel-plugin-import --save`

编辑 .webpackrc，使 babel-plugin-import 插件生效。

```js
{
+  "extraBabelPlugins": [
+    ["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }]
+  ]
}
```
