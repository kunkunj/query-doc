## wxPage 构造器

```
const { wxPage } = require('wx-query/index')
```

- 由于微信官方只有简易双向绑定，为了解决日常开发对双向绑定的需求，提供一个复杂双向绑定的核心 `observeData`

```
// index.js
const { wxPage } = require('wx-query/index')

// 构造page
wxPage.init({
  observeData:{
    form:{
      name:'张三'
    }
  }
})
```

- 在 wxml 中我们可以这么使用双向绑定,使用`$$`符号作为取值符号

```
<input model:value="{{observeData$$form$$name}}" />

<view>
  内容：{{observeData$$form$$name}}
</view>

```

- 对于数组的操作，我们也提供了一些遍历方法`push`、`pop`、`unshift`、`shift`

  在原生小程序开发中，你需要通过 setData()函数来更新您对数组的操作，如：

  ```
  //js
  Page({
    data:{
      arr:[1,2]
    },
    push(){
      this.data.arr.push(3)
      this.setData({
        arr:this.data.arr
      })
    },
    pop(){
      this.data.arr.pop()
      this.setData({
        arr: arr
      })
    },
    shift(){
      this.data.arr.shift()
      this.setData({
        arr: arr
      })
    },
    unshift(){
      this.data.arr.unshift()
      this.setData({
        arr: arr
      })
    }
  })

  //wxml
  <text wx:for="{{arr}}">{{item}}</text>

  ```

  但是在 wxPage 的构造器中您可以直接使用，如：

  ```
  //js
  wxPage.init({
    observeData:{
      arr:[1,2]
    },
    push(){
      this.observeData.arr.push(3)
    },
    pop(){
      this.observeData.arr.pop()
    },
    shift(){
      this.observeData.arr.shift()
    },
    unshift(){
      this.observeData.arr.unshift()
    }
  })

  //wxml
  <text wx:for="{{observeData$$arr}}">{{item}}</text>

  ```

### wxPage.init

- 该方法是构造器的初始化方法

- 参数:

  `原生小程序参数`: 原生小程序参数如：onReady、onLoad等

  `observeData`: 双向绑定数据

  `$keyName`: 插件的系统容器

```
const { wxPage } = require('wx-query/index')
wxPage.init({
  observeData:{},
  onReady(){
    console.log('执行了----onReady')
  },
  onLoad(){
    console.log('执行了----onLoad')
  },
  onHide(){
    console.log('执行了----onHide')
  }
})

```

### wxPage.use

- wx-query提供了插件系统，通过`use`方法对插件进行注册

  开发一个WxRequest插件：

  ````
  //WxRequest.js
  exprot const WxRequest = {}
  WxRequest.install = function (page) {
    page.plugins.$request = function(){
      ...
    };
  }
  ````

  使用WxRequest插件:
  
  使用`use`方法注册插件后，会把方法放在`init`方法的`$keyName`命名中，如果没有声明`$keyName`，则放在`$`下

  ````
  const { WxRequest,wxPage } = require('wx-query/index')
  wxPage.use(WxRequest)

  // 不声明$keyName
  wxPage.init({
    onLoad(){
      //使用方法
      this.$.$request()
    }
  })

  // 声明$keyName
  wxPage.init({
    $keyName:'$plugin',
    onLoad(){
      //使用方法
      this.$plugin.$request()
    }
  })
  ````

### wxPage.create

- 该方法可以创建一个 Page 构造器的实例,可以创建多个构造器实例

  创建不同的实例可以根据需要加载不同的插件

```
//page1页面
const { WxRequest,wxPage } = require('wx-query/index')
const myPage1 = wxPage.create()
myPage1.use(WxRequest)

//page2页面
const { wxPage } = require('wx-query/index')
const myPage2 = wxPage.create()
```

## wxComponet 构造器

- 由于使用的同一套流程，`wxComponet`构造器和`wxPage`构造器的使用方法一致

````
const { wxComponet } = require('wx-query/index')

wxComponet.init({
  observeData: {
    name:'张三'
  },
  methods: {
    notice() {
      console.log(this.observeData.name)
    }
  }
})
````