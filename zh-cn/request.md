### WxRequest(options)

- `options`:

| 属性                 | 类型                                               | 默认值 | 必填 | 说明                                                         |
| -------------------- | -------------------------------------------------- | ------ | ---- | ------------------------------------------------------------ |
| url                  | string                                             |        | 是   | 开发者服务器接口地址                                         |
| data                 | string/object/ArrayBuffer                          |        | 否   | 请求的参数                                                   |
| header               | Object                                             |        | 否   | 设置请求的 header，header 中不能设置 Referer。<br/>`content-type` 默认为 `application/json` |
| timeout              | number                                             |        | 否   | 超时时间，单位为毫秒。默认值为 60000                         |
| method               | OPTIONS/HEAD/CONNECT<br/>POST/PUT/DELETE/TRACE/GET | GET    | 否   | HTTP 请求方法                                                |
| dataType             | json/其他                                          | json   | 否   | 返回的数据格式<br/>json: 返回的数据为 JSON，返回后会对返回的数据进行一次 JSON.parse<br/>其他: 不对返回的内容进行 JSON.parse |
| responseType         | text/arraybuffer                                   | text   | 否   | text: 响应的数据为文本<br/>arraybuffer:响应的数据为 ArrayBuffer |
| enableHttp2          | boolean                                            | false  | 否   | 开启 http2                                                   |
| enableQuic           | boolean                                            | false  | 否   | 开启 quic                                                    |
| enableCache          | boolean                                            | false  | 否   | 开启 cache                                                   |
| enableHttpDNS        | boolean                                            | false  | 否   | 是否开启 HttpDNS 服务。如开启，需要同时填入 httpDNSServiceId 。 HttpDNS 用法详见 [移动解析HttpDNS](https://developers.weixin.qq.com/miniprogram/dev/framework/ability/HTTPDNS.html) |
| httpDNSServiceId     | string                                             |        | 否   | HttpDNS 服务商 Id。 HttpDNS 用法详见 [移动解析HttpDNS](https://developers.weixin.qq.com/miniprogram/dev/framework/ability/HTTPDNS.html) |
| enableChunked        | boolean                                            | false  | 否   | 开启 transfer-encoding chunked。                             |
| forceCellularNetwork | boolean                                            | false  | 否   | wifi下使用移动网络发送请求                                   |



````
WxRequest({url:'xxxxx'}).then(res=>{
    console.log(res)
}).catch(err=>{
    console.log(err)
})
````

### create

- WxRequest.create(config)方法得到一个request实例

- config

  `baseURL`: 同一实例的统一url，不为空时，请求时链接为 `baseURL + url`

  `timeout`: 同一实例的统一timeout，请求options和config都不为空时，请求的优先级高

  `headers`: 同一实例的统一headers，请求options和config都不为空时，请求的优先级高

  ````
  const request = WxRequest.create(config)
  request.request(options)
  request.get(options)
  request.post(options)
  request.delete(options)
  request.head(options)
  request.trace(options)
  request.put(options)
  request.options(options)
  request.connect(options)
 ```` 


 ### interceptors

 - 拦截器分为`请求拦截器`和`相应拦截器`

   例：

   ````
   const { wxRequest } = require('wx-query/index')

   WxRequest.interceptors.response.use(function(response){
    return response
   },function(error){
    return Promise.reject(error)
   })

   WxRequest.interceptors.request.use(function(response){
    return response
   },function(error){
    return Promise.reject(error)
   })

   ````

#### use

- WxRequest.interceptors.request.use(callback,callback)

  WxRequest.interceptors.response.use(callback,callback)

- 添加一个拦截器，第一个回调函数是成功回调，第二个回调是失败回调

#### eject

- WxRequest.interceptors.request.eject(id)

  WxRequest.interceptors.response.eject(id)

- 注销某一个拦截器

  ````
  const id = WxRequest.interceptors.request.use(callback,callback)
  // 注销
  WxRequest.interceptors.response.eject(id)
  ````

#### clear

- 清空拦截器

  ````
  // 清空
  WxRequest.interceptors.response.clear()
  ````

 