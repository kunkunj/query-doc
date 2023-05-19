#### createStore(id,options)

  `id`: 唯一id，重复则是替换

  `options`:

| 参数    | 类型     | 描述                |
| ------- | -------- | ------------------- |
| state   | function | 储存状态值          |
| actions | object   | 修改state的唯一动作 |



- 创建store

````
// store.js

const {
  createStore
} = require('wx-query/index')
export const store = createStore('user', {
  state: () => ({
    name: 'jack',
  }),
  actions: {
    changeName() {
      this.name = this.name == 'tom' ? 'jack' : 'tom'
    },
  }
})
````

- 引用store

````
// index.js
import { store } from 'store.js'
const { wxPage } = require('wx-query/index')

wxPage.init({
    Store:[store],
    changeStore(){
        store.changeName()
    }
})

//wxml
<text>{{$store$$user$$name}}</text>
<button bindtap='changeStore'>切换名称</button>
````