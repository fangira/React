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

### 使用Scss样式必须安装
```
$ npm install node-sass sass-loader --save
```

### 去除css/scss后面的hash值
编辑 .webpackrc

```
"disableCSSModules":true
```
### 路由重定向及嵌套路由

```js
import React from 'react'

import { Route, Switch, Link, Redirect } from 'dva/router';

import Introduce from './about/Introduce.jsx';
import Borad from './about/Borad';

class About extends React.Component {
    render() {
        return (
            <div>
                <Link to="/about/introduce">GO TO introduce</Link>
                <br />
                <Link to="/about/borad">GO TO borad</Link>
                {/*Switch作用就是只能匹配一个路由，例如：输入/about网址 就不会在渲染/和/about了*/}
                <Switch>
                
                    {/*写在前面的路由 先判断。 如果为/about/*的路由地址 直接执行render的方法 重定向为XXX*/}
                    <Route exact path="/about/" render={() =>
                        <Redirect to='/about/introduce'></Redirect>}></Route>
                    <Route path="/about/introduce" exact component={Introduce} />
                    <Route path="/about/borad" exact component={Borad} />
                </Switch>
            </div>
        )
    }
}

export default About;
```


### redux使用

先在index.js写入
```js
//引入redux仓库设置
app.model(require('./models/example').default);
```

models/example.js

```js
export default {

  //组件中connect的名字
  namespace: 'example',
  
//仓库中的数据
  state: {
    introduceInputNum:0
  },

//操作仓库的方法
  reducers: {
  //这个叫save
    save(state, action) {
      return { ...state, ...action.payload };
    },
  },

};
```

borad.jsx

```jsx
import React from 'react'
import { connect } from 'dva';

class Board extends React.Component {
    constructor({ dispatch }) {
        super(dispatch);
        this.dispatch = dispatch;
    }
    increaseNum() {
    
    //触发仓库中的方法
        this.dispatch({
        
        //type：为命名空间+方法名
            type: 'example/save',
            
            // payload：传入类似this.setstate的参数
            payload: { introduceInputNum: ++this.props.example.introduceInputNum },
        });
    }
    render() {
        return (
            <div>
            
                {/*链接仓库后，通过this.props.[命名空间].[仓库数据] 来获取仓库中的数据*/}
                <h2>目前仓库值为{this.props.example.introduceInputNum}</h2>
                
                <button onClick={this.increaseNum.bind(this)}>点击增加仓库的值</button>
            </div>
        )
    }
}
//一般写法 export default connect()(Board);  这样连接不了任何仓库
//下面写法才能连接到命名空间为example的仓库
export default connect(({ example }) => ({ example }))(Board);
```
