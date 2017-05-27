---
title: axios使用
date: 2017-05-25 24:10:24
tags: vue
---
在VUE2.0出来以后，官方就不推荐使用vue-resource了，而是使用axios ,使用方法都大同小异，在项目中有俩种使用方法，教程地址: [axios](https://www.npmjs.com/package/axios)

### 第一种(配置文件)
这里使用的是vue-webpack脚手架，在src目录下，新建一个api目录，里面分别创建这俩个文件 api.js  / config.js,详情看代码。
#### api.js
``` bash
import axios from 'axios'		//导入axios模块
import config from './config'	//导入配置文件
import qs from 'qs'

//axios(config);
class API {
	getList (param) {
		console.log(config);   //这里打印的是配置文件中的信息
		//发送请求
		return axios.post(config.url,{
			id:param
		})
	}
}
export default API;
```

#### config.js   //这里配置信息可参考上面的连接地址
``` bash
import Qs from 'qs'
export default{
	// `url` is the server URL that will be used for the request 
  url: 'http://localhost:80/cms/getinfo.php',
 
  // `method` is the request method to be used when making the request 
  method: 'get', // default 
 
  // `baseURL` will be prepended to `url` unless `url` is absolute. 
  // It can be convenient to set `baseURL` for an instance of axios to pass relative URLs 
  // to methods of that instance. 
  baseURL: 'https://some-domain.com/api/',
 
  // `transformRequest` allows changes to the request data before it is sent to the server 
  // This is only applicable for request methods 'PUT', 'POST', and 'PATCH' 
  // The last function in the array must return a string or an instance of Buffer, ArrayBuffer, 
  // FormData or Stream 
  transformRequest: [function (data) {
    // Do whatever you want to transform the data 
 
    return data;
  }],
 
  // `transformResponse` allows changes to the response data to be made before 
  // it is passed to then/catch 
  transformResponse: [function (data) {
    // Do whatever you want to transform the data 
 
    return data;
  }],
 
  // `headers` are custom headers to be sent 
  headers: {'X-Requested-With': 'XMLHttpRequest'},
  headers: {'Content-Type': 'application/x-www-form-urlencoded'},
 
  // `params` are the URL parameters to be sent with the request 
  // Must be a plain object or a URLSearchParams object 
  params: {
    ID: 12345
  },
 
  // `paramsSerializer` is an optional function in charge of serializing `params` 
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/) 
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },
 
  // `data` is the data to be sent as the request body 
  // Only applicable for request methods 'PUT', 'POST', and 'PATCH' 
  // When no `transformRequest` is set, must be of one of the following types: 
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams 
  // - Browser only: FormData, File, Blob 
  // - Node only: Stream, Buffer 
  data: {
    firstName: 'Fred'
  },
 
  // `timeout` specifies the number of milliseconds before the request times out. 
  // If the request takes longer than `timeout`, the request will be aborted. 
  timeout: 1000,
 
  // `withCredentials` indicates whether or not cross-site Access-Control requests 
  // should be made using credentials 
  withCredentials: false, // default 
 
  // `adapter` allows custom handling of requests which makes testing easier. 
  // Return a promise and supply a valid response (see lib/adapters/README.md). 
  adapter: function (config) {
    /* ... */
  },
 
  // `auth` indicates that HTTP Basic auth should be used, and supplies credentials. 
  // This will set an `Authorization` header, overwriting any existing 
  // `Authorization` custom headers you have set using `headers`. 
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },
 
  // `responseType` indicates the type of data that the server will respond with 
  // options are 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream' 
  responseType: 'json', // default 
 
  // `xsrfCookieName` is the name of the cookie to use as a value for xsrf token 
  xsrfCookieName: 'XSRF-TOKEN', // default 
 
  // `xsrfHeaderName` is the name of the http header that carries the xsrf token value 
  xsrfHeaderName: 'X-XSRF-TOKEN', // default 
 
  // `onUploadProgress` allows handling of progress events for uploads 
  onUploadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event 
  },
 
  // `onDownloadProgress` allows handling of progress events for downloads 
  onDownloadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event 
  },
 
  // `maxContentLength` defines the max size of the http response content allowed 
  maxContentLength: 2000,
 
  // `validateStatus` defines whether to resolve or reject the promise for a given 
  // HTTP response status code. If `validateStatus` returns `true` (or is set to `null` 
  // or `undefined`), the promise will be resolved; otherwise, the promise will be 
  // rejected. 
  validateStatus: function (status) {
    return status >= 200 && status < 300; // default 
  },
 
  // `maxRedirects` defines the maximum number of redirects to follow in node.js. 
  // If set to 0, no redirects will be followed. 
  maxRedirects: 5, // default 
}
```
#### 组件中调用
``` bash
import API from '../api/API'	//导入模块
let api = new API()      		//实例化对象，然后对象调用不同的方法。
```

### 第二种方法(组件注入试)
在src中的main.js做如下配置
``` bash
import axios from 'axios';		//导入模块
Vue.prototype.$http = axios;	// 把axios的信息赋值到$http上，我们组件中就可以使用 this.$http方法
```
以上就是俩种使用方法，不过目前我还没解决跨域问题，如果有知道的可以告诉我一下。