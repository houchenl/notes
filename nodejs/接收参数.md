
# body-parser解析请求体

POST请求的请求体，可能包含多种类型，需要解析之后，才能使用。express提供了中间件`body-parser`用于解析POST请求的请求体，其它用于解析请求体的模块有`body`、`co-body`。  

POST请求，请求体(body)的内容，常见的类型有json、表单、字符串、二进制数据、文件。 分别对应的请求体类型(Content-Type)为：`application/json`、`application/x-www-form-urlencoded`、`text/plain`、`application/octet-stream`、`multipart/form-data`。 `body-parser`为前4种请求体准备了解析模块，分别为`json()`、`urlencoded()`、`text()`、`raw()`。 请求体为文件的，`body-parser`没有做处理，如果要解析这种请求体，使用以下模块：`busboy` and `connect-busboy`，`multiparty` and `connect-multiparty`，`formidable`，`multer`。  

POST请求的请求体类型及编码，在请求头的Content-Type中指定。  

请求体(body)、请求头中的请求类型(Content-Type)、解析模块、解析模块中的type都对应时，`body-parser`也能解析成功。  

## 安装[body-parser](https://github.com/expressjs/body-parser)

`$ npm install body-parser -g`  

## 基本使用

### 准备工作
``` javascript
const express = require('express');
const app = express();
const bodyParser = require('body-parser');

// create application/json parser
const jsonParser = bodyParser.json()

// create application/x-www-form-urlencoded parser
const urlencodedParser = bodyParser.urlencoded({ extended: false })
```

### 统一指定解析方式
第一种使用方法，是为所有请求使用一样的解析方式，不推荐这种方式。  
``` javascript
app.use(jsonParser);
app.use(urlencodedParser);
```

### 逐个指定解析方式
另一种方法，是为每个POST请求，分别指定解析方式，推荐这种方式。
``` javascript
app.post('url', jsonParser, (req, res) => {});
```

### 使用数据
解析成功后，就可以使用`req.body`或`req.on('data', (chunk) => {}).on('end', () => {});`来获取、使用body中的数据。

## POST报文
```
POST /test HTTP/1.1
Host: 127.0.0.1:3000
Content-Type: text/plain; charset=utf8
Content-Encoding: gzip

chyingp
```
Content-Type: 请求报文主体的类型、编码。常见的类型有：`text/plain`, `application/json`, `application/x-www-form-urlencoded`, `multipart/form-data`。常见的编码有`utf-8`, `gbk`。
Content-Encoding: 声明报文主体的压缩格式，常见有`gzip`, `deflate`, `identity`。
报文主体： `chyingp`。

## options
`body-parser`提供的4种解析方法，都有options参数，具体含义参考[官方文档](https://github.com/expressjs/body-parser)。
