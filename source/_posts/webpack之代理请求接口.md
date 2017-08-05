---
title: webpack之代理请求接口
date: 2017-08-04 13:12:24
tags: webpack
---
前提依赖axios，
``` bash
var axios = require('axios')
```

原理大概为 去们请求axios服务，axios伪造host等信息请求数据，大概配置如下：

``` bash
//请求代理
var apiRoutes = express.Router()
apiRoutes.get('/getDiscList', function (req, res) {
  var url = 'https://c.y.qq.com/splcloud/fcgi-bin/fcg_get_diss_by_tag.fcg'
  axios.get(url, {
    headers: {
      referer: 'https://c.y.qq.com/',
      host: 'c.y.qq.com'
    },
    params: req.query
  }).then((response) => {
    res.json(response.data)
  }).catch((e) => {
    console.log(e)
  })
})

app.use('/api',apiRoutes)

```