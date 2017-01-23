# Configuration
# 配置

Set default values using the global configuration.

使用全局配置设置默认值

```js
Vue.http.options.root = '/root';
Vue.http.headers.common['Authorization'] = 'Basic YXBpOnBhc3N3b3Jk';
```

Set default values inside your Vue component options.

在你的Vue组件配置中设置默认值

```js
new Vue({

  http: {
    root: '/root',
    headers: {
      Authorization: 'Basic YXBpOnBhc3N3b3Jk'
    }
  }

})
```

## Webpack/Browserify
## Webpack/Browserify（两种前端打包/构建工具）

Add `vue` and `vue-resource` to your `package.json`, then `npm install`, then add these lines in your code:

在你项目中的`package.json`文件中添加`vue`和`vue-resource`依赖，然后在命令行中执行`npm install`，然后将如下代码拷贝到你的项目入口文件中。

```js
var Vue = require('vue');
var VueResource = require('vue-resource');

Vue.use(VueResource);
```

## Legacy web servers
## 传统WEB服务器

If your web server can't handle requests encoded as `application/json`, you can enable the `emulateJSON` option. This will send the request as `application/x-www-form-urlencoded` MIME type, as if from an normal HTML form.

如果你的WEB服务器不能处理编码为`application/json`的请求，你可以将`emulateJSON`选项配置为enable。然后Vue-resource会将发送的请求MIME类型设为`application/x-www-form-urlencoded`，就像一个标准的HTML表单提交的MIME类型一样。

```js
Vue.http.options.emulateJSON = true;
```

If your web server can't handle REST/HTTP requests like `PUT`, `PATCH` and `DELETE`, you can enable the `emulateHTTP` option. This will set the `X-HTTP-Method-Override` header with the actual HTTP method and use a normal `POST` request.

如果你的WEB服务器不能处理类似于`PUT`，`PATCH`和`DELETE`这种REST/HTTP请求，你可以将`emulateHTTP`选项配置为enable。这样将会通过设置HTTP Header（头部）为`X-HTTP-Method-Override`的方式携带上原始的HTTP请求方法，并且通过标准的POST方法发送请求。

```js
Vue.http.options.emulateHTTP = true;
```
