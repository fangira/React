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

### dva脚手架中的路由重定向

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
