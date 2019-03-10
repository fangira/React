# 简单配置Redux.md

### App.js(redux的设置)

```js
import React, { Component } from 'react';

import './App.css';

import { HashRouter as Router, Route, Link } from "react-router-dom";

import Xheader from './page/Xheader.jsx'
import Xlist from './page/Xlist.jsx'

//先安装react-redux和redux,然后引入redux依赖 
import { Provider } from 'react-redux';
import { createStore } from 'redux';

//创建state仓库
const store =createStore((state = {
  count: 0,
  name:'aaa',
  age:'10'
}, action) {
  //判断action.type
  switch (action.type) {
    case 'increase':
    //推荐写法，返回值会直接更新state仓库，如果只返回state.count=10,则state仓库的其他数据将会被覆盖
      return {
        ...state,
        count: state.count + 2
      }
    case 'multi':
      return Object.assign({}, state, { name: action.name });
    default:
      return state
  }
})

class App extends Component {
  state = {
    num: 10
  }
  render() {
    return (
    //要用Provider标签套着，把仓库设置store加载到其属性上，
    原理：把仓库store放到父context（不懂可以回去看基础-老谢的context的定义）,
    利用react自带的context加上redux的reducer和state的设置组合成react-redux
      <Provider store={store}>
        <Router>
          <div>
            <ul>
              <li><Link to="/">Xheader</Link></li>
              <li><Link to={`/about/${this.state.num}`}>Xlist</Link></li>
            </ul>

            <hr />

            <Route exact path="/" component={Xheader} />
            <Route path="/about/:idx" component={Xlist} />
          </div>
        </Router>
      </Provider>
    );
  }
}

export default App;

```


### Xheader.jsx（子组件映射）

```jsx
import React from 'react';
//引入依赖
import { connect } from 'react-redux';
class Xheader extends React.Component {

    render() {
        return (
            <div>
                <h2>Xheader</h2>
                //获取仓库的值
                <h4>{this.props.count}</h4>
                //点击后触发下面定义的方法onIncreaseClick
                <button onClick={this.props.onIncreaseClick}>点击</button>
            </div>
        )
    }
}
//connect要传两个函数，1.返回仓库中的所有值或指定值。2.定义方法onIncreaseClick，触发dispatch仓库中的type:'increase'的case。
//第一个函数映射state用于获取仓库的值。第二个函数映射dispatch用于修改仓库的值
export default connect((state) => {
return state
}, (dispatch) => {
    return {onIncreaseClick: () => {
        dispatch({type:'increase'})
      },onTest(){
      //可以触发多个仓库中的case。在仓库中通过action.test获取参数'lalala'
          dispatch({type:'仓库中没有这个action.type',test:'lalala'})
          dispatch({type:'仓库中没有这个action.type',test:'hahaha'})
      }}
})(Xheader)
```
