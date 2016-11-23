#	HTTP POST，请求与响应

HTTP 协议规定 POST 数据应当放在消息实体（entity-body）中，但并没有限制编码格式，唯一的约定是在请求头的 Content-Type 中对格式进行必要的描述。当然，如果客户端和服务端已有私有约定，也完全可以不作任何描述。

本节中，我们将以 NPM 流行模块 request 和 express 为工具，比较几种__常见的__ POST 请求格式的发起和处理。

假定：
```javascript
// suppose on [CLIENT SIDE]
var request = require('request');

// suppose on [SERVER SIDE]
var express = require('express')
var app = express();
```

##	简单请求

###	Request Content-Type: application/x-www-form-urlencoded  

```javascript
// [CLIENT SIDE]
request.post({ url: URL, form: FORM_JSON });

// [SERVER SIDE]
var bodyParser = require('body-parser');
app.use(bodyParser.urlencoded({ extended: true }));
app.post('*' function(req, res) {
	// form data now contained in req.body
	console.log(req.body);
	// ...
});
```

关于 ```bodyParser.urlencoded()``` 方法，请留意[官方说明](https://www.npmjs.com/package/body-parser)。

###	Request Content-Type: multipart/form-data

```javascript
// [CLIENT SIDE]
request.post({ url: URL, formData: FORM_JSON });

// [SERVER SIDE]
var multer = require('multer'); // v1.0.5
var upload = multer(); // for parsing multipart/form-data
app.post('*', upload.array(), function(req, res) {
	// form data now contained in req.body
	console.log(req.body);
	// ...
});
```

###	Request Content-Type: application/json

```javascript
// [CLIENT SIDE]
request.post({ url: URL, json: FORM_JSON });
// ATTENTION: In this case, request will set the next headers at the same time,
// accept:          application/json
// content-type:    application/json

// [SERVER SIDE]
var bodyParser = require('body-parser');
app.use(bodyParser.json());
app.post('*', function(req, res) {
	// form data now contained in req.body
	console.log(req.body);
	// ...
});
```

在使用 ```request``` 提交 JSON 格式的 POST 数据时，需要注意避免错误的写法：
```javascript
// ✅ CORRECT
// 以下两种提交方法正确且等效。
request.post({ url: URL, json: FORM_JSON }))
request.post({ url: URL, body: FORM_JSON, json: ture })

// ⛔ WARN
// 提交序列化值，但未声明 content-type / accept 头信息。
// 不建议使用。
request.post({ url: URL, body: FORM_JSON })

// ⛔ WARN
// 提交字符串 "<FORM_JSON 序列化值>" 的序列化值。
// 极易产生误解，强烈建议不要使用！
request.post({ url: URL, body: JSON.stringify(FORM_JSON), json: ture })
```

##	上传文件

##	在线资源

*	四种常见的 POST 提交数据方式  
	https://imququ.com/post/four-ways-to-post-data-in-http.html
