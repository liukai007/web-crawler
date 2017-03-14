
# 项目 API 规范

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

* JSON 对象必须***[MUST]***位于每个JSON API文档的根级，这个对象定义文档的*top level*。
* 文档必须***[MUST]***包含以下至少一种*top-level*键，且data键和errors键不能***[MUST NOT]***在一个文档中同时存在。
    * data: 文档的"primary data"。(对象或数组)
    * errors: 错误对象列表。(只可以是数组)
    * meta: 包含非标准元信息的元对象。
* 文档可以***[MAY]***包含以下任何top-level键：
    * links: 与primary data相关的连接对象。（e.g:分页请求）
    
## 资源对象 [data]

* 每个资源对象必须***[MUST]***包含一个id键，且id键的值必须是字符串类型。
* 对于每一个既定API，每个资源对象的id必须***[MUST]***定义一个单独且唯一的资源。
* 资源对象的属性可以包含任何合法JSON值。
* Primary data必须***[MUST]***是以下格式的一种：
    * 如果请求要求单一资源，应该是一个单一资源对象，或一个单一资源标识符，或null。
    * 如果请求要求资源集合，应该是一个资源对象列表，或一个空列表([])。

>以下primary data表示一个单一资源对象
```json
{
  "data": {
    "id": "1",
    "name" : "admin",
    "age" : 30
  }
}
```

>以下primary data表示资源的集合
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

>meta键可用于包含非标准的元信息。每个meta键的值必须***[MUST]***是一个对象("meta object")。任何键都可以包含在meta对象中(e.g:客户端自定义的一些非标准键如分页信息total-page等)。
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
  }
}
```

## 响应

### 200 OK 状态码

* 当请求成功获取一个单独资源或资源集合时，服务器必须***[MUST]***以200 OK状态码响应该请求。
* 当请求成功获取一个资源集合时，服务器必须***[MUST]***将一个资源对象的列表或空列表([])作为响应文档的主数据。

>一个空集合不能返回null，应该返回空列表
```json
{
  "data": []
}
```

* 当请求成功获取一个单独资源时，服务器必须***[MUST]***将一个资源对象或null作为响应文档的主数据。

### 404 Not Found 状态码 

* 当通过一个请求获取一个不存在的单独资源时，服务器必须***[MUST]***以404 Not Found状态码响应。
* 如果资源存在，但是资源为空，必须***[MUST]***以200 OK状态码返回一个null作为响应文档的主数据。

### 其他响应码

* 服务器可以***[MAY]***响应其他HTTP状态码。
* 服务器可以***[MAY]***在错误响应中包含错误详情。
* 服务器和客户端必须依据[HTTP协议语义](https://tools.ietf.org/html/rfc7231)分别生成和解析响应。

## 内联资源

* 后端可以***[MAY]***默认响应与主数据（primary data）相关的资源。
* 后端也可以***[MAY]***支持**include**请求参数以保证客户端可以指定需要返回的相关资源。
* 如果后端不支持include参数，必须***[MUST]***以400 Bad Request状态码回复任何包含此参数的请求。
* include参数的值必须***[MUST]***是一个由逗号分隔符(U+002C COMMA, “,”) 分割的关联路径(relationship paths)列表。
* 关联路径(relationship paths)是由点号分隔符(U+002E FULL-STOP, “.”) 分割的关联名称(relationship names)。
* 如果服务器不能识别关联路径或不能通过路径支持内联资源，必须***[MUST]***以400 Bad Request状态码响应。

>同时请求产品评论与图集, 通过逗号分隔多个关联资源：

>GET /products/1?include=comments,images HTTP/1.1

## 稀疏字段集

* 客户端可以***[MAY]***通过fields[TYPE]参数,请求后端返回只包含特定字段的响应。
* fields参数的值必须***[MUST]***用逗号分隔开,用来表示需要返回字段的名称。
* 如果客户端请求了一组给定类型的字段集,那么后端不能***[MUST NOT]***在资源对象中包括额外的字段。

>只获取产品的标题与内容字段和与其关联资源作者的名称字段

>GET /products?include=author&fields[products]=title,text&fields[author]=name HTTP/1.1 

## 排序
* 服务器可以***[MAY]***选择性支持，根据一个或多个条件(排序字段)对资源集合排序。
* 后端可以***[MAY]***支持带有sort查询参数的请求,来排序主数据（primary data）。sort的值必须***[MUST]***代表排序字段。
* 后端可以***[MAY]***带有多个排序字段的请求,允许用逗号分隔排序字段。排序字段应该***[SHOULD]***被按照特定顺序执行。
* 每个排序字段必须***[MUST]***按升序排列。除非它带有-前缀,这种情况下将按降序排列。
* 如果服务器不支持查询参数sort指定的排序,必须***[MUST]***返回400 Bad Request。

>按产品创建时间升序排序

>GET /products?sort=created HTTP/1.1

>按多个字段排序

>GET /products?sort=created,rating HTTP/1.1

>按创建时间降序排序

>GET /products?sort=-created HTTP/1.1

## 分页

* 服务器可以***[MAY]***选择性限制响应返回的资源数量，作为所有可获取资源的一个子集("page")。
* 服务器可以***[MAY]***提供传送分页后数据集的连接("pagination links")。
* 查询参数***page***是分页的保留字，服务器和客户端应该***[SHOULD]***用这个参数进行分页操作。
* pagination links必须***[MUST]***出现在集合相关的连接对象中。需要在top-level的links对象中提供pagination links。
* 下面的键必须***[MUST]***被用于pagination links中
    * first: 首页
    * last: 末页
    * prev: 上一页
    * next: 下一页
* 如需表示特定的连接不可用，这些键必须***[MUST]***被省略，或者值为null。
    * page-based 使用***page[number]***和***page[size]***参数
    * offset-based 使用***page[offset]***和***page[limit]***参数
    * cursor-based 使用***page[cursor]***参数
    
Example:    
```json
{
  "meta": {
    "total-pages": 13
  },
  "data": [
    {
      "type": "articles",
      "id": "3",
      "attributes": {
        "title": "JSON API paints my bikeshed!",
        "body": "The shortest article. Ever.",
        "created": "2015-05-22T14:56:29.000Z",
        "updated": "2015-05-22T14:56:28.000Z"
      }
    }
  ],
  "links": {
    "self": "http://example.com/articles?page[number]=3&page[size]=1",
    "first": "http://example.com/articles?page[number]=1&page[size]=1",
    "prev": "http://example.com/articles?page[number]=2&page[size]=1",
    "next": "http://example.com/articles?page[number]=4&page[size]=1",
    "last": "http://example.com/articles?page[number]=13&page[size]=1"
  }
}
```
    
## 过滤

* 查询参数filter是过滤的保留字，服务器和客户端应该***[SHOULD]***用这个参数进行过滤。

>查找品牌为雅马哈的所有产品

>GET /products?filter[brand]=Yamaha HTTP/1.1

>查找品牌为雅马哈或EV, 且类别为guitar所有产品

>GET /products?filter[brand]=Yamaha,EV&filter[category]=guitar HTTP/1.1

## 创建、更新、删除资源

* 服务器可能***[MAY]***允许对给定类型资源的创建，服务器也可能***[MAY]***允许对已存在资源的修改与删除。
* 一个请求必须***[MUST]***完全成功或失败（在单一事务中），不允许部分更新成功。

### 创建资源

#### 响应

##### 201 Created

* 请求的资源成功被创建，服务器必须***[MUST]***返回[201 Created]状态码。
* 响应应该***[SHOULD]***包含[Location]首部，用以表示请求创建的资源的位置。
* 响应可以***[MAY]***含有一个文档，用以存储所创建的主要资源或其唯一标识符(primary data)。

>HTTP/1.1 201 Created<br>
Location: http://www.mifan.cn/products/308761<br>
Content-Type: application/json
```json
{
  "data": {
    "type": "products",
    "id": "308761"
  }
}
```
##### 202 Accepted

* 如果创建资源的请求被接受处理，但在服务器响应时处理并未完成，那么服务器必须***[MUST]***返回[202 Accepted]状态码。

##### 204 No Content

* 请求的资源被创建成功，服务器必须***[MUST]***返回[201 Created]状态码和响应文档(如上所述)，或者只返回[204 No Content]状态码，没有响应文档。

##### 403 Forbidden

* 服务器可能***[MAY]***向不支持的创建资源请求返回[403 Forbidden]的响应。

##### 404 Not Found

* 如果创建请求所引用的相关资源不存在，服务器必须***[MUST]***返回[404 Not Found]的响应。

##### 409 Conflict

* 如果在创建请求中，客户端生成的ID已经存在，服务器必须***[MUST]***返回[409 Conflict]的响应。
* 如果在创建请求中，资源对象的type不在后端支持的类型里，服务器必须***[MUST]***返回[409 Conflict]的响应。
* 服务器应该***[SHOULD]***在响应中包括错误详情和足够的信息以识别出冲突的原因。

##### 其他响应 

* 服务器可以***[MAY]***响应其他HTTP状态码。
* 服务器在错误响应(error responses)中可以***[MAY]***包含错误信息(error details)。
* 服务器与客户端必须***[MUST]***依照[HTTP协议语义](https://tools.ietf.org/html/rfc7231)来准备和解译响应。

### 更新资源  (暂不支持PATCH方法, POST替代) 

* 向表示资源的URL发出PATCH请求，即可进行资源更新。
* PATCH请求必须包括一个资源对象(resource object)作为主数据(primary data)。资源对象必须***[MUST]***包含id成员。

#### 更新属性

* 资源的任何一个或所有属性，可能***[MAY]***被包含在PATCH请求的资源对象中(resource object)。
* 如果请求不包括资源所有的属性，那么服务器解译请求时，必须***[MUST]***添加这些属性并赋予其当前的值。服务器不能***[MUST NOT]***给缺失的属性赋值为null。

Example:如下请求只更新产品ID=1的数据的标题与描述信息

>PATCH /products/1 HTTP/1.1<br>
Content-Type: application/json<br>
Accept: application/json
```json
{
  "data": {
    "id": "1",
    "name": "Gretsch Drums Catalina Club",
    "description": "everything you need to start drumming in one complete package with Pearl's Roadshow 5-piece drum set."
 }
}
```

#### 更新资源关联

#### 响应



### 删除资源 (暂不支持DELETE方法 POST替代) 

* 向资源URL发出DELETE请求即可删除单个资源。

>DELETE /product/1 HTTP/1.1<br>
Accept: application/json

#### 响应

##### 202 Accepted

* 如果删除资源的请求被接受处理，但在服务器响应时处理并未完成，那么服务器必须***[MUST]***返回[202 Accepted]状态码。

##### 204 No Content

* 如果删除请求成功，并且没有返回内容，服务器必须***[MUST]***返回[204 No Content]状态码。

##### 200 OK

* 如果删除请求成功，并且top-level中有且仅有元数据(meta)信息作为返回内容，服务器必须***[MUST]***返回[200 OK]状态码。

##### 404 NOT FOUND

* 如果因为资源不存在导致删除请求失败，那么服务器应该***[SHOULD]***返回[404 Not Found]状态码。

##### 其他响应 

* 服务器可以***[MAY]***响应其他HTTP状态码。
* 服务器在错误响应(error responses)中可以***[MAY]***包含错误信息(error details)。
* 服务器与客户端必须***[MUST]***依照[HTTP协议语义](https://tools.ietf.org/html/rfc7231)来准备和解译响应。

## 错误

### 错误处理

* 当遇到一个问题时，服务器可以***[MAY]***选择停止处理，也可以***[MAY]***继续处理并遇到多个问题。 比如，服务器可能处理多个属性，并在一个响应中返回多个验证问题。
* 当服务器在一个请求中遇到多个问题时，应该***[SHOULD]***在响应中使用HTTP最通用的错误状态码。比如：
    * 400 Bad Request 可被用于多个4xx错误。
    * 500 Internal Server Error 可被用于多个5xx错误。
    
### 错误对象 [Errors Objects]

* 错误对象提供执行操作时遇到问题的额外信息。 
* 在JSON API文档top-level，错误对象必须***[MUST]***被作为errors键对应的数组返回。
* 错误对象可能***[MAY]***有以下成员：（目前错误信息只使用加粗字体成员，其余字段作为保留关键字）
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

>HTTP/1.1 400 Bad Request<br>
Content-Type: application/json
```json
{
  "errors": [
    {
      "status": "403",
      "detail": "Editing secret powers is not authorized on Sundays."
    },
    {
      "status": "422",
      "detail": "Volume does not, in fact, go to 11."
    },
    {
      "status": "500",
      "title": "The backend responded with an error",
      "detail": "Reputation service not responding after three requests."
    }
  ]
}
```
