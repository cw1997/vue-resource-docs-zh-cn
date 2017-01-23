# HTTP
# HTTP

The http service can be used globally `Vue.http` or in a Vue instance `this.$http`.
你可以使用全局对象方式`Vue.http`或者在一个Vue实例的内部使用`this.$http`来发起HTTP请求。

## Usage
## 语法

A Vue instance provides the `this.$http` service which can send HTTP requests. A request method call returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) that resolves to the response. Also the Vue instance will be automatically bound to `this` in all function callbacks.
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

## Methods
## 方法

Shortcut methods are available for all request types. These methods work globally or in a Vue instance.
我们为所有的请求类型提供了一套简便的方法。 这些方法可以在全局或一个Vue实例中工作。

```js
// global Vue object
Vue.http.get('/someUrl', [options]).then(successCallback, errorCallback);
Vue.http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);

// in a Vue instance
this.$http.get('/someUrl', [options]).then(successCallback, errorCallback);
this.$http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);
```
List of shortcut methods:
简便方法的列表:

* `get(url, [options])`
* `head(url, [options])`
* `delete(url, [options])`
* `jsonp(url, [options])`
* `post(url, [body], [options])`
* `put(url, [body], [options])`
* `patch(url, [body], [options])`

## Options
## 选项

Parameter | Type | Description
--------- | ---- | -----------
url | `string` | URL to which the request is sent
body | `Object`, `FormData`, `string` | Data to be sent as the request body
headers | `Object` | Headers object to be sent as HTTP request headers
params | `Object` | Parameters object to be sent as URL parameters
method | `string` | HTTP method (e.g. GET, POST, ...)
timeout | `number` | Request timeout in milliseconds (`0` means no timeout)
before | `function(request)` | Callback function to modify the request options before it is sent
progress | `function(event)` | Callback function to handle the [ProgressEvent](https://developer.mozilla.org/en-US/docs/Web/API/ProgressEvent) of uploads
credentials | `boolean` | Indicates whether or not cross-site Access-Control requests should be made using credentials
emulateHTTP | `boolean` | Send PUT, PATCH and DELETE requests with a HTTP POST and set the `X-HTTP-Method-Override` header
emulateJSON | `boolean` | Send request body as `application/x-www-form-urlencoded` content type

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

## Response
## 响应

A request resolves to a response object with the following properties and methods:
通过如下属性和方法处理一个请求获取到的响应对象：

Property | Type | Description
-------- | ---- | -----------
url | `string` | Response URL origin
body | `Object`, `Blob`, `string` | Response body data
headers | `Header` | Response Headers object
ok | `boolean` | HTTP status code between 200 and 299
status | `number` | HTTP status code of the response
statusText | `string` | HTTP status text of the response
**Method** | **Type** | **Description**
text() | `Promise` | Resolves the body as string
json() | `Promise` | Resolves the body as parsed JSON object
blob() | `Promise` | Resolves the body as Blob object

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

## Example
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

Fetch an image and use the blob() method to extract the image body content from the response.
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

## Interceptors
## 拦截器

Interceptors can be defined globally and are used for pre- and postprocessing of a request.
全局定义拦截器后，它可用于前置和后置处理请求。

### Request processing
### 请求处理
```js
Vue.http.interceptors.push((request, next) => {

  // modify request
  request.method = 'POST';

  // continue to next interceptor
  next();
});
```

### Request and Response processing
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

### Return a Response and stop processing
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
