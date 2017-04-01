# user center

## 关于响应

+ POST 
    + [MUST] 请求的资源成功被创建，服务器必须返回[201 Created]状态码。
    + [SHOULD] 响应应该包含[Location]首部，用以表示请求创建的资源的位置。
    + [MAY] 响应可以含有一个文档，用以存储所创建的主要资源或其唯一标识符(primary data)。
    + [MUST] 请求的资源被创建成功，服务器必须返回[201 Created]状态码和响应文档(如上所述)，或者只返回[204 No Content]状态码，没有响应文档。

+ DELETE
    + [MUST] 如果删除资源的请求被接受处理，但在服务器响应时处理并未完成，那么服务器必须返回[202 Accepted]状态码。
    + [MUST] 如果删除请求成功，并且top-level中有且仅有元数据(meta)信息作为返回内容，服务器必须返回[200 OK]状态码。
    + [MUST] 如果删除请求成功，并且没有返回内容，服务器必须返回[204 No Content]状态码。
    + [SHOULD] 如果因为资源不存在导致删除请求失败，那么服务器应该返回[404 Not Found]状态码。

+ PATCH
    + [MUST] 如果更新成功，服务器必须返回[200 OK]状态码。
    + [MUST] 如果更新成功，并且请求中的资源与更新后的结果一致，那么服务器必须返回[204 No Content]状态码。
        + 该状态码适用于向To-Many关联中添加 [POST] 了一个已经存在的关联。
        + 该状态码同样适用于向To-Many关联中删除 [DELETE] 了一个不存在的关联。

## 关于 HTTP method

+ 兼容性
    + 所有非GET方法可以使用POST方法并在URL中增加参数_method=[POST|PATCH|DELETE]来请求一个资源。
        
用户服务 APIs

## 新增(POST)/查询集合(GET)地址 [/api/addresses]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + [MAY] 只查询需要用到的字段 
        + fields[addresses]=id,user_id,code,province,city,district,address,priority
    + [MAY] 查询列表时降序排序 __/api/addresses?sort=-priority__

+ Fields
    + code [NOT NULL] - 行政区划代码
    + province - 省
    + city - 市
    + district - 区
    + mobile [NOT NULL] - 手机号
    + consignee [NOT NULL] - 收件人/收货人
    + address [NOT NULL] - 收货地址
    + priority [NOT NULL] - 1:默认地址, 0:非默认
    
+ Meta
    + number (int) - 当前页
    + size (int) - 每页大小
    + numberOfElements (int) - 当前页有多少记录
    + last (boolean) - 是否是最后一页
    + totalPages (int) - 总页数
    + sort (object) - 排序相关信息
    + first (boolean) - 是否是第一页
    + totalElements - 总记录数

### 新增地址 [POST]

+ Request (application/json)

        {
            "data": {
                "code": 150600,
                "province": "北京省",
                "city": "北京市",
                "district": "东城区",
                "address": "雨儿胡同乙10号",
                "consignee": "安抚22233",
                "mobile": "13611019209"
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /api/addresses/2

    + Body

            {
                "data": {
                    "id": 2,
                    "type": "addresses"
                }
            }
            
+ Response 204 (application/json)
        
+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "Pattern.UserAddresses.mobile",
                    "title": "Bad Request",
                    "detail": "手机格式错误",
                    "source": {
                        "pointer": "UserAddresses -> mobile"
                    }
                }
            ]
        }
        
### 查询地址集合 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "number": 1,
                "size": 2,
                "numberOfElements": 2,
                "last": false,
                "totalPages": 2,
                "sort": null,
                "first": true,
                "totalElements": 4
            },
            "links": {
                "self": "/api/addresses?page[number]=1&page[size]=2",
                "first": "/api/addresses?page[number]=1&page[size]=2",
                "next": "/api/addresses?page[number]=2&page[size]=2",
                "last": "/api/addresses?page[number]=2&page[size]=2"
            },
            "data": [
                {
                    "id": 1,
                    "enabled": 1,
                    "creator": 1,
                    "modifier": 1,
                    "created": "2017-03-31 12:12:12",
                    "modified": "2017-03-31 17:36:37",
                    "userId": 12,
                    "code": 610116,
                    "province": "陕西省",
                    "city": "西安市",
                    "district": "长安区",
                    "mobile": "13611019209",
                    "consignee": "曲子乐",
                    "address": "雨儿胡同乙10号",
                    "priority": 0
                },
                {
                    "id": 2,
                    "enabled": 1,
                    "creator": 12,
                    "modifier": 12,
                    "created": "2017-03-31 17:00:10",
                    "modified": "2017-03-31 17:36:37",
                    "userId": 12,
                    "code": 150600,
                    "province": "",
                    "city": "",
                    "district": "",
                    "mobile": "13611019209",
                    "consignee": "安抚22233",
                    "address": "阿萨德的222qqUI个我个我个我个我个我个我还好",
                    "priority": 0
                }
            ]
        }
        
## 查询(GET)/删除(DELETE)/修改(PATCH)用户地址 [/api/addresses/{id}]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + [MAY] 只查询需要用到的字段 
        + fields[addresses]=id,user_id,code,province,city,district,address,priority
    + [MAY] 只修改需要修改的字段
    + NOTE : 设置默认地址相当于只修改priority字段
    + NOTE : PATCH要在资源对象(Primary Data)中明确指定id

+ Fields
    + code [NOT NULL] - 行政区划代码
    + province - 省
    + city - 市
    + district - 区
    + mobile [NOT NULL] - 手机号
    + consignee [NOT NULL] - 收件人/收货人
    + address [NOT NULL] - 收货地址
    + priority [NOT NULL] - 1:默认地址, 0:非默认

+ Parameters
    + id (string) - 资源标识符

### 查询地址信息 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 1,
                "enabled": 1,
                "creator": 1,
                "modifier": 1,
                "created": "2017-03-31 12:12:12",
                "modified": "2017-03-31 12:12:12",
                "userId": 12,
                "code": 610116,
                "province": "陕西省",
                "city": "西安市",
                "district": "长安区",
                "mobile": "13611019209",
                "consignee": "曲子乐",
                "address": "雨儿胡同乙10号",
                "priority": 1
            }
        }

### 删除地址信息 [DELETE]

+ Response 204 (application/json)

### 修改地址信息 [PATCH]

+ Request (application/json)

        {
            "data": {
                "id": 5,
                "code": 150600,
                "province": "北京省",
                "city": "北京市",
                "district": "东城区",
                "address": "雨儿胡同乙10号",
                "consignee": "曲子乐",
                "mobile": "13611019209",
                "priority": 1
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)


