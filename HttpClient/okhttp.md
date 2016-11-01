# OkHttp

## 简介
`OkHttp` 作为一个简洁高效的 `HTTP` 客户端，可以在 `Java` 和 `Android` 程序中使用。相对于 `Apache HttpClient` 来说，`OkHttp` 的性能更好，其 `API` 设计也更加简单实用。

## Http Request
一个`http request`由`Request-line（请求行）`、`Header（消息头）`和`Message-body（请求实体）`组成，后两者之间有一空行隔开，且后两者可以为空，而`Request-line`不得为空。

举个例子：

```
POST /index.php HTTP/1.1
user-agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_0)
Accept: text/html
Accept-Language: zh-cn,zh;q=0.5
Referer: http://localhost/
```

第一行是请求行，指出了请求的方式为`POST`，访问的是根目录下的`index.php`，`http`版本为`1.1`。

余下的为消息头，包括`user-agent`（指明终端类型），`accept`（终端可以解释的类型），`accept-language`（浏览器能解释的语言）等等。

在`OkHttp`库中，通过`new Request.Builder().build()`来创建`Request`对象，并且可以通过`url()`指定`url`，`header()`指定消息头细节。


## Get方式
### 实例化OkHttpClient
一般来说，一个`app`只需要一个`OkHttpClient`：

```Java
OkHttpClient mClient = new OkHttpClient();
```

### Sync get（同步get）

```Java
private void syncGet(String url) {

    final Request request = new Request.Builder()
            .url(url)
            .build();

    new Thread() {
        Response mResponse = null;

        @Override
        public void run() {
            super.run();
            try {
                mResponse = mClient.newCall(request).execute();
                System.out.println(mResponse.body().string());
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }.start();
}
```
可以看到，`Request`通过`Request.Builder().build()`来生成。可以在生成过程中插入`url`，`header`等。

同步方法相当于阻塞当前线程（UI线程），故需要开启子线程。通过`OkHttpClient.newCall(request).execute()`来访问网络。

### Async get（异步get）

```Java
private void asyncGet(String url) {

    Request request = new Request.Builder()
            .url(url)
            .build();

    mClient.newCall(request).enqueue(mPrintResponseCallback);

}
```

异步方式，`request`的生成方式跟同步一样，但是执行使用的是`enqueue`方法。`enqueue`字眼说明了是进入线程调度，而不是直接阻塞当前线程。  

`enqueue`方法需要传入一个`Callback`接口，这里自定义一个`Callback`接口：

```Java
private class printResponseCallback implements Callback {

    @Override
    public void onFailure(Call call, IOException e) {
        e.printStackTrace();
    }

    @Override
    public void onResponse(Call call, Response response) throws IOException {
        if (response.isSuccessful())
            System.out.println(response.body().string());
    }
}
```

需要重写两个方法，`onFailure`是访问失败时调用，`onResponse`是有`Response`返回时调用的方法，需要注意的是`onResponse`仍然是运行在***工作线程***，故不可以直接在此处更新`UI`。

## Post方式
### Sync post（同步post）

```Java
private void syncPost(String url) {

    MediaType MEDIA_TYPE_TEXT = MediaType.parse("text/plain");
    RequestBody requestBody = RequestBody.create(MEDIA_TYPE_TEXT, "Hello World");

    final Request request = new Request.Builder().url(url).post(requestBody).build();

    new Thread(){
        Response mResponse = null;
        @Override
        public void run() {
            super.run();
            try {
                mResponse = mClient.newCall(request).execute();
                System.out.println(mResponse.body().string());
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }.start();

}
```

`post`请求需要指定`MediaType`，通过`RequestBody.create(MediaType, String)`可以将`String`类型转换成`MediaType`类型的`RequestBody`。

在构建`Request`时，需要多加`post(RequestBody)`来添加请求实体。网络请求部分的代码跟`get`方式一样。

### Async post（异步post）

```Java
private void asyncPost(String url) {

    MediaType MEDIA_TYPE_TEXT = MediaType.parse("text/plain");
    RequestBody body = RequestBody.create(MEDIA_TYPE_TEXT, "Hello World");

    Request request = new Request.Builder().url(url).post(body).build();

    mClient.newCall(request).enqueue(mPrintResponseCallback);

}
```

### 流方式构建实体

```Java
private void postFlow(String url){

    final MediaType MEDIA_TYPE_TEXT = MediaType.parse("text/plain");
    final String requestBody = "Hello World";
    RequestBody body = new RequestBody() {
        @Override
        public MediaType contentType() {
            return MEDIA_TYPE_TEXT;
        }

        @Override
        public void writeTo(BufferedSink sink) throws IOException {
            sink.writeUtf8(requestBody);
        }
    };

    Request request = new Request.Builder().url(url).post(body).build();
    mClient.newCall(request).enqueue(mPrintResponseCallback);
}
```
这里创建了 `RequestBody` 的一个匿名子类。该子类的 `contentType` 方法需要返回内容的媒体类型，而 `writeTo` 方法的参数是一个 `BufferedSink` 对象。需要做的就是把请求的内容写入到 `BufferedSink` 对象。

## Http Response
通过上述的几种方法我们都可以得到`Response`对象。  

一个`Http Response`对象包括：`Status-line`、若干`Header`和`Message-body`，其中后两者可以没有，消息头和实体内容间有一个空行。

举个例子：

```
HTTP/1.1 200 OK
Date: Mon, 27 Jul 2009 12:28:53 GMT
Server: Apache/2.2.14 (Win32)
Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
Content-Length: 88
Content-Type: text/html
Connection: Closed
```

通过`Response`对象，使用`response.body()`可以获得`message-body`，通过`request.headers`可以得到集合对象`Headers`。利用`headers.name(index)`和`headers.value(index)`可以分别得到每个`header`的名字和值。

## 总结
正在适应`OkHttp`。

