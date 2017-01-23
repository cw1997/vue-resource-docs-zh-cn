# HTTP

你可以使用全局对象方式`Vue.http`或者在一个Vue实例的内部使用`this.$http`来发起HTTP请求。

## 语法

一个Vue实例内部提供的`this.$http`服务可以发送HTTP请求。一个请求方法的回调请参考 [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)它可以解决这些响应。当然Vue实例将自动绑定到`this`在所有的函数回调中。

```js
{
  // GET /someUrl
  this.$http.get('/someUrl').then((response) => {
    // success callback
  }, (response) => {
    // error callback
  });
}
```

## 方法

我们为所有的请求类型提供了一套简便的方法。 这些方法可以在全局或一个Vue实例中工作。

```js
// global Vue object
Vue.http.get('/someUrl', [options]).then(successCallback, errorCallback);
Vue.http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);

// in a Vue instance
this.$http.get('/someUrl', [options]).then(successCallback, errorCallback);
this.$http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);
```
简便方法的列表:

* `get(url, [options])`
* `head(url, [options])`
* `delete(url, [options])`
* `jsonp(url, [options])`
* `post(url, [body], [options])`
* `put(url, [body], [options])`
* `patch(url, [body], [options])`

## 选项

参数 | 类型 | 描述
--------- | ---- | -----------
url | `string` | 请求的目标URL
body | `Object`, `FormData`, `string` | 作为请求体发送的数据
headers | `Object` | 作为请求头部发送的头部对象
params | `Object` | 作为URL参数的参数对象
method | `string` | HTTP方法 (例如GET，POST，...)
timeout | `number` | 请求超时（单位：毫秒） (`0`表示永不超时)
before | `function(request)` | 在请求发送之前修改请求的回调函数
progress | `function(event)` | 用于处理上传进度的回调函数 [ProgressEvent](https://developer.mozilla.org/en-US/docs/Web/API/ProgressEvent)
credentials | `boolean` | 是否需要出示用于跨站点请求的凭据
emulateHTTP | `boolean` | 是否需要通过设置`X-HTTP-Method-Override`头部并且以传统POST方式发送PUT，PATCH和DELETE请求。
emulateJSON | `boolean` | 设置请求体的类型为`application/x-www-form-urlencoded`

## 响应

通过如下属性和方法处理一个请求获取到的响应对象：

属性 | 类型 | 描述
-------- | ---- | -----------
url | `string` | 响应的URL源
body | `Object`, `Blob`, `string` | 响应体数据
headers | `Header` | 请求头部对象
ok | `boolean` | 当HTTP响应码为200到299之间的数值时该值为true
status | `number` | HTTP响应吗
statusText | `string` | HTTP响应状态
**方法** | **类型** | **描述**
text() | `约定值` | 以字符串方式返回响应体
json() | `约定值` | 以格式化后的json对象方式返回响应体
blob() | `约定值` | 以二进制Blob对象方式返回响应体

## 例子

```js
{
  // POST /someUrl
  this.$http.post('/someUrl', {foo: 'bar'}).then((response) => {

    // get status
    response.status;

    // get status text
    response.statusText;

    // get 'Expires' header
    response.headers.get('Expires');

    // set data on vm
    this.$set('someData', response.body);

  }, (response) => {
    // error callback
  });
}
```

使用blob()方法从响应中获取一副图像的内容。

```js
{
  // GET /image.jpg
  this.$http.get('/image.jpg').then((response) => {

    // resolve to Blob
    return response.blob();

  }).then(blob) => {
    // use image Blob
  });
}
```

## 拦截器

全局定义拦截器后，它可用于前置和后置处理请求。

### 请求处理
```js
Vue.http.interceptors.push((request, next) => {

  // modify request
  request.method = 'POST';

  // continue to next interceptor
  next();
});
```

### 请求与响应处理
```js
Vue.http.interceptors.push((request, next)  => {

  // modify request
  request.method = 'POST';

  // continue to next interceptor
  next((response) => {

    // modify response
    response.body = '...';

  });
});
```

### 返回一个响应并且停止处理
```js
Vue.http.interceptors.push((request, next) => {

  // modify request ...

  // stop and return response
  next(request.respondWith(body, {
    status: 404,
    statusText: 'Not found'
  }));
});
```
