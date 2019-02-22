# axios

在React中使用axios的话 可以把axios绑定在React.Component的原型链上

### 设置main.js
```js
import React from 'react';
import axios from 'axios';
React.Component.protoype.$axios = axios;
```

### 使用
xxx.jsx
```jsx
···
async getData(){
    let data = await this.$axios(url,{name:'abc',age:20});
    console.log(data);
}
···
```
