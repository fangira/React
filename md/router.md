# 简单配置路由例子


### App.js
```js
import React, { Component } from 'react';
import './App.css';

// 要提前安装react-router-dom 
//HashRouter 就是hash路由 带#号的 如http://localhost:3000/#/
import { HashRouter  as Router, Route, Link } from "react-router-dom";
//BrowserRouter 就是浏览器路由 利用H5特性 不兼容
// import { BrowserRouter as Router, Route, Link } from "react-router-dom";

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

# 动态路由例子

### App.js

```js
import React, { Component } from 'react';
import './App.css';

import { HashRouter as Router, Route, Link } from "react-router-dom";


import Xheader from './page/Xheader.jsx'
import Xlist from './page/Xlist.jsx'

class App extends Component {
  state={
    num:10
  }
  render() {
    return (
      <Router>
      <div>
        <ul>
          <li><Link to="/">Xheader</Link></li>
          //跳转时传参
          <li><Link to={`/about/${this.state.num}`}>Xlist</Link></li>
        </ul>
  
        <hr/>
  
        <Route exact path="/" component={Xheader}/>
        //设置   ：动态变量
        <Route path="/about/:idx" component={Xlist}/>
      </div>
    </Router>
    );
  }
}

export default App;

```

### Xlist.jsx

```jsx
import React from 'react'
import { Route, Link } from "react-router-dom";

import Xa from './Xlist/Xa.jsx';
import Xb from './Xlist/Xb.jsx';

class Xlist extends React.Component {
 
    render() {
        return (
            <div>
                <h2>Xlist</h2>
                //通过 this.props.match.params 获取路由上的参数
                <h3>动态路由idx的值为{ this.props.match.params.idx}</h3>
                <ul>
                    <li><Link to="/about/a">Xa</Link></li>
                    <li><Link to="/about/b">Xb</Link></li>

                </ul>
                <Route exact path="/about/a" component={Xa} />
                <Route exact path="/about/b" component={Xb} />
            </div>
        )
    }
}

export default Xlist;

```

### 编程式导航例子

```jsx
···
goTo(){
        this.props.history.push({pathname:'/abc'})
        
        //通过replace方法跳转的 不会记录在history里面
        this.props.history.replace({pathname:'/abc'})
    }
 ···
//非路由组件就必须通过withRouter修饰后才能获取到history,location,match
export default withRouter(Xheader);

```

