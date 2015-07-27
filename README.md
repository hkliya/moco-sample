## Mock Server指南

Mock Server的目的是模拟一个HTTP/HTTPS的后台服务器，用以提高开发效率，或者提高测试稳定性及隔离问题。

使用开源的工具[Moco](https://github.com/dreamhead/moco)可以轻易地搭建Mock Server。

### 运行

运行如下命令：

```
java -jar moco-runner-0.10.2.jar http -p 80 -c config.json
```
>80表示HTTP端口号，`config.json`是配置文件。

### 示例
#### 基本使用
`config.json`

```
[
  {
    "request": {
      "uri": "/users"
    },
    "response": {
      "file": {
        "name": "users.response"
      }
    }
  }
]

```

`users.response`

```
[
  {
    "name":"zhang san",
    "age": "18"
  },{
    "name":"li si",
    "age": "28"
  }
]

```

#### 动态数据
有时同一个接口，相同的URL，相同的参数，但需要返回不同的数据。那可以使用这个特性，基本的原理是配置时，将返回设置成多个，moco会按请求的顺序依次返回，次数大于返回个数时将始终返回最后一条数据。

`config.json`

```
[
  {
    "request": {
      "uri": "/seq"
    },
    "responses" : [
      {
        "file": {
          "name": "r1.response"
        }
      }, {
        "file": {
          "name": "r2.response"
        }
      }]
  }
]
```

当通过浏览器访问`http://localhost/seq`时，将返回`r1.response`的内容，此时刷新页面，将返回`r2.response`的内容，再次刷新，将一直返回`r2.response`的内容。

>更多高级特性，请参考官方文档：[https://github.com/dreamhead/moco/blob/master/moco-doc/apis.md](https://github.com/dreamhead/moco/blob/master/moco-doc/apis.md)
