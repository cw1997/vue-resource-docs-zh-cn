# Code Recipes
# 编码规范

The Recipes provide code examples to common use-cases. If you want to share your recipe feel free to send a [pull request](https://github.com/vuejs/vue-resource/pulls).
这里提供了一套通用的编码规范。如果你想分享你的较为自由的编码规范，可以发起[pull request](https://github.com/vuejs/vue-resource/pulls).

## Forms
表单

Sending forms using [FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData).
使用[FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)发送表单。

```js
{
  var formData = new FormData();

  // append string
  formData.append('foo', 'bar');

  // append Blob/File object
  formData.append('pic', fileInput, 'mypic.jpg');

  // POST /someUrl
  this.$http.post('/someUrl', formData).then((response) => {
    // success callback
  }, (response) => {
    // error callback
  });
}
```

## Abort a request
## 终止请求

Abort a previous request when a new request is about to be sent. For example when typing in a autocomplete input.
当一个新的请求被发送的时候请终止上一个请求。例如在一个自动完成的输入框中输入的时候。

```js
{
  // GET /someUrl
  this.$http.get('/someUrl', {

    // use before callback
    before(request) {

      // abort previous request, if exists
      if (this.previousRequest) {
        this.previousRequest.abort();
      }

      // set previous request on Vue instance
      this.previousRequest = request;
    }

  }).then((response) => {
    // success callback
  }, (response) => {
    // error callback
  });
}
```
