es6 的promise 逐步解决了层层回调的问题,es8的async await让异步变成了同步的写法,在vue中,可以通过封装axios,使得所有的请求都可以使用同步写法,同时处理错误信息等,可以建一个api.js文件,全局创建api实例.

```
import axios from 'axios'
const qs = require('qs')
const api = {
  async get (url, data) {
    try {
      let res = await axios.get(url, {params: data})
      res = res.data
      return new Promise((resolve) => {
        if (res.code === 0) {
          resolve(res)
        } else {
          resolve(res)
        }
      })
    } catch (err) {
      alert('服务器出错')
      console.log(err)
    }
  },
  async post (url, data) {
    try {
      let res = await axios.post(url, qs.stringify(data))
      res = res.data
      return new Promise((resolve, reject) => {
        if (res.code === 0) {
          resolve(res)
        } else {
          reject(res)
        }
      })
    } catch (err) {
      // return (e.message)
      alert('服务器出错')
      console.log(err)
    }
  },
}
export { api }
```

上述代码中,首先采用try,catch 捕获请求的错误, 如果网络状态差,服务器错误等 ,然后在请求成功状态中,亦可统一处理请求代码,这个可以根据具体项目处理,上例表示code=0的时候为结果正确状态. 使用可以参考如下,以vue项目为例:

```
  import { api } from 'common/js/api'

  export default {
    data () {
      return {
        list: [],
      }
    },
    created () {
      this.getList()
    },
    methods: {
      async getList () {
        let {data} = await api.get('/ferring/test/list')
        console.log(data)
        this.list = data
      }
    },
  }
```