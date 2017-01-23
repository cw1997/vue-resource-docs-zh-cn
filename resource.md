
# 资源

你可以使用全局的`Vue.resource`或者在一个Vue实例内部使用`this.$resource`发起一个Resource请求。

## 方法

* `resource(url, [params], [actions], [options])`

## 默认操作

```js
get: {method: 'GET'},
save: {method: 'POST'},
query: {method: 'GET'},
update: {method: 'PUT'},
remove: {method: 'DELETE'},
delete: {method: 'DELETE'}
```

## 例子

```js
{
  var resource = this.$resource('someItem{/id}');

  // GET someItem/1
  resource.get({id: 1}).then((response) => {
    this.$set('item', response.json())
  });

  // POST someItem/1
  resource.save({id: 1}, {item: this.item}).then((response) => {
    // success callback
  }, (response) => {
    // error callback
  });

  // DELETE someItem/1
  resource.delete({id: 1}).then((response) => {
    // success callback
  }, (response) => {
    // error callback
  });
}
```

## 自定义操作

```js
{
  var customActions = {
    foo: {method: 'GET', url: 'someItem/foo{/id}'},
    bar: {method: 'POST', url: 'someItem/bar{/id}'}
  }

  var resource = this.$resource('someItem{/id}', {}, customActions);

  // GET someItem/foo/1
  resource.foo({id: 1}).then((response) => {
    this.$set('item', response.json())
  });

  // POST someItem/bar/1
  resource.bar({id: 1}, {item: this.item}).then((response) => {
    // success callback
  }, (response) => {
    // error callback
  });
}
```
