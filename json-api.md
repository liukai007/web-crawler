
# 项目 JSON API 规范

## 文档中的关键字依据 [RFC2119](https://tools.ietf.org/html/rfc2119) 规范解释。 
+ MUST
+ MUST NOT
+ REQUIRED
+ SHALL
+ SHALL NOT
+ SHOULD
+ SHOULD NOT
+ RECOMMENDED
+ MAY
+ OPTIONAL
    
## 文档结构

>JSON 对象必须***[MUST]***位于每个JSON API文档的根级，这个对象定义文档的*top level*。

>文档必须***[MUST]***包含以下至少一种*top-level*键，且data键和errors键不能***[MUST NOT]***在一个文档中同时存在。这里取消了原项目中当请求成功后返回的成功状态码，现在使用HTTP（协议语义）状态码替代。

* data: 文档的"primary data"。(对象或数组)
* errors: 错误对象列表。(只可以是数组)
* meta: 包含非标准元信息的元对象。

## 资源对象 [data]

* 每个资源对象必须***[MUST]***包含一个id键。id键的值必须是字符串类型。
* 对于每一个既定API，每个资源对象的id必须***[MUST]***定义一个单独且唯一的资源。
* 资源对象的属性可以包含任何合法JSON值

### Primary data 必须是以下列举的一种：

* 如果请求要求单一资源，应该是一个单一资源对象，或一个单一资源标识符，或null。
* 如果请求要求资源集合，应该是一个资源对象列表，或一个空列表([])。

以下primary data表示一个单一资源对象
```json
{
  "data": {
    "id": "1",
    "name" : "admin",
    "age" : 30
  }
}
```

以下primary data表示资源的集合
```json
{
  "data": [{
    "id": "1",
    "name" : "user1",
    "age" : 30
  },{
    "id": "2",
    "name" : "user2",
    "age" : 22
  }]
}
```

## 元信息 [meta]

meta键可用于包含非标准的元信息。每个meta键的值必须***[MUST]***是一个对象("meta object")。任何键都可以包含在meta对象中(e.g:客户端自定义的一些非标准键如分页信息total-page等)。
```json
{
  "meta": {
    "copyright": "Copyright 2015 Example Corp.",
    "authors": [
      "Yehuda Katz",
      "Steve Klabnik",
      "Dan Gebhardt",
      "Tyler Kellen"
    ]
  },
  "data": {
    // ...
  }
}
```

## Errors Objects

>错误对象提供执行操作时遇到问题的额外信息。 在JSON API文档顶层，错误对象必须[MUST]被作为errors键对应的数组返回。

>错误对象可能[MAY]有以下成员:

* id - 特定问题的唯一标识符。
* links - 一个包括以下成员的连接对象:
    * about - 指向特定问题更多具体内容的连接。
* __status__ - 适用于这个问题的HTTP状态码，使用字符串表示。
* __code__ - 应用特定的错误码，以字符串表示。
* __title__ - 简短的，可读性高的问题总结。除了国际化本地化处理之外，不同场景下，相同的问题，值是不应该变动的。
* __detail__ - 针对该问题的高可读性解释。与title相同,这个值可以被本地化。
* source - 包括错误资源的引用的对象。它也可以包括以下任意成员:
    * pointer - 一个指向请求文档中相关实体的JSON Pointer。
    * parameter - 指出引起错误的查询对象的字符串。
* meta - 一个包括关于错误的非标准元信息的元对象。


