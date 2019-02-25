# 高阶组件

### [参考简书](https://www.jianshu.com/p/85f165ca002c)

### withLogin.js
```js
/**
 * 高阶组件
 * 功能：用户需要登录才能访问的页面
 */

 import React,{Component} from 'react';
 import {connect} from 'react-redux';

 import axios from 'axios';

 const withLogin = MyComponent=>{
     class Container extends Component{
        async componentWillMount(){
            let {token,history} = this.props;

            if(!token){
                // 如redux中无token，则从本地存储中获取
                token = localStorage.getItem('token');
            }

            // 发起ajax请求验证token
            let res = await axios.get('http://localhost:4008/verifytoken',{
                headers:{
                    token
                }
            });
            res = res.data;
            
            if(!token || res.code === 302){
                // 无token则调到登录页面
                history.push('/login');
            }
        }
        shouldComponentUpdate(){
            return !!this.props.token;
        }
        render(){
            //把props的参数传进去，不然里面组件拿不到。如有必要可以加上 {...this.state}
            return <MyComponent {...this.props}/>
        }
    }

    // 利用connect高阶组件映射token到props
    return connect(state=>({
        token:state.common.user.token
    }))(Container);
 }

 export default withLogin;

```
