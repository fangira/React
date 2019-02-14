# 简单配置路由例子


### App.js
```js
import React, { Component } from 'react';
import './App.css';

// 要提前安装react-router-dom 
import { BrowserRouter as Router, Route, Link } from "react-router-dom";

//引入组件
import Xheader from './page/Xheader.jsx'
import Xlist from './page/Xlist.jsx'

class App extends Component {
  render() {
    return (
    //只需写一次Router标签 包着全部
      <Router>
      <div>
        <ul>
        //路由跳转用Link标签
          <li><Link to="/">Xheader</Link></li>
          <li><Link to="/about">Xlist</Link></li>
        </ul>
  
        <hr/>
        //配置路由，exact加在'/' 有嵌套路由的不能加exact
        <Route exact path="/" component={Xheader}/>
        <Route path="/about" component={Xlist}/>
      </div>
    </Router>
    );
  }
}

export default App;


```
