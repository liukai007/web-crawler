FORMAT: 1A
HOST: http://www.mifan.com/

# user center

+ 2017年4月10日
    + 新增用户签到 API
    + 新增查询被邀请人列表 API

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
    + 可以使用HTTP method.
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
    + code (int) [NOT NULL] - 行政区划代码
    + province (string) - 省
    + city (string) - 市
    + district (string) - 区
    + mobile (string) [NOT NULL] - 手机号
    + consignee (string) [NOT NULL] - 收件人/收货人
    + address (string) [NOT NULL] - 收货地址
    + priority (int) [NOT NULL] - 1:默认地址, 0:非默认
    
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
    + code (int) [NOT NULL] - 行政区划代码
    + province (string) - 省
    + city (string) - 市
    + district (string) - 区
    + mobile (string) [NOT NULL] - 手机号
    + consignee (string) [NOT NULL] - 收件人/收货人
    + address (string) [NOT NULL] - 收货地址
    + priority (int) [NOT NULL] - 1:默认地址, 0:非默认

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

## 查询(GET)/签到(POST)用户积分/米粒集合 [/api/grains]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + [MAY] 只查询需要用到的字段 
        + fields[grains]=score,action,created
    + [MAY] 按日期降序排序
        + sort=-created
        
+ Fields
    + created (date) - 时间
    + score (int) - [+|-]积分/米粒
    + action (string) - 行为/动作

### 查询集合 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "number": 1,
                "size": 10,
                "numberOfElements": 7,
                "last": true,
                "totalPages": 1,
                "sort": [
                    {
                        "direction": "DESC",
                        "property": "created",
                        "ignoreCase": false,
                        "nullHandling": "NATIVE",
                        "ascending": false,
                        "descending": true
                    }
                ],
                "first": true,
                "totalElements": 7
            },
            "links": {
                "self": "/api/grains?fields[grains]=score,action,created&sort=-created&page[number]=1&page[size]=10",
                "first": "/api/grains?fields[grains]=score,action,created&sort=-created&page[number]=1&page[size]=10",
                "last": "/api/grains?fields[grains]=score,action,created&sort=-created&page[number]=1&page[size]=10"
            },
            "data": [
                {
                    "created": "2017-04-01 11:35:50",
                    "score": 5,
                    "action": "邀请好友"
                },
                {
                    "created": "2017-04-01 11:35:49",
                    "score": 5,
                    "action": "邀请好友"
                },
                {
                    "created": "2017-04-01 11:35:48",
                    "score": 2,
                    "action": "连续签到"
                },
                {
                    "created": "2017-04-01 11:35:47",
                    "score": 2,
                    "action": "连续签到"
                },
                {
                    "created": "2017-04-01 11:35:46",
                    "score": 10,
                    "action": "每日签到"
                },
                {
                    "created": "2017-04-01 11:35:45",
                    "score": 10,
                    "action": "每日签到"
                },
                {
                    "created": "2017-04-01 11:35:44",
                    "score": 10,
                    "action": "每日签到"
                }
            ]
        }
        
### 签到 [POST]

+ Request (application/json)

        {
            "data": {
                "userId": 11
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /api/grains/36

    + Body

            {
                "data": {
                    "id": 35,
                    "type": "grains"
                }
            }
   
+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "title": "Bad Request",
                    "detail": "今天已签到!"
                }
            ]
        }
        
## 查询(GET)/修改(PATCH)用户资料 [/api/profiles/{id}]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + [MAY] 只查询需要用到的字段 
        + fields[profiles]=id,fullname,nickname,gender,birthday,city_id,company,profession,email,user_karma,user_avatar

+ Fields 
    + gender (int) [NOT NULL] - 性别 0:保密 1:男 2:女
    + birthday (date) - 生日
    + nickname (string) - 昵称
    + fullname (string) - 全名
    + email (string) - 邮箱
    + userKarma (int) [NOT NULL] - 用户积分/米粒
    + userAvatar (string) - 头像
    + cityId (int) - 城市编码
    + company (string) - 公司
    + profession (string) - 职位
    + signCount (int) [NOT NULL] - 签到总次数
    + signCountContinuos (int) [NOT NULL] - 连续签到次数
    + signTime (date) - 最后签到时间

### 查询用户资料 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 12,
                "gender": 2,
                "birthday": 1474560000000,
                "nickname": "失落的乖乖",
                "fullname": "请求222",
                "email": "651742081@qq.com",
                "userKarma": 435,
                "userAvatar": "http://static.budee.com/iyyren/image/201610/21/1452/132943086757232640.jpg",
                "cityId": 445100,
                "company": "请求ee",
                "profession": "录音师",
                "signCount": 43,
                "signCountContinuos": 1,
                "signTime": 1490687401000
            }
        }

### 修改用户资料 [PATCH]

+ Request (application/json)

        {
            "data": {
                "id": 12,
                "gender": 2,
                "birthday": 1474560000000,
                "nickname": "失落的乖乖",
                "fullname": "请求222",
                "email": "651742081@qq.com",
                "userKarma": 435,
                "userAvatar": "http://static.budee.com/iyyren/image/201610/21/1452/132943086757232640.jpg",
                "cityId": 445100,
                "company": "请求ee",
                "profession": "录音师",
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)

## 修改用户密码(PATCH) [/api/users/{id}]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    
+ Fields
    + password (string) - 新密码
    + oldPassword (string) - 原始密码

+ Parameters
    + id (string) - 资源标识符

### 修改用户密码 [PATCH]

+ Request (application/json)

        {
            "data": {
                "id": 12,
                "password": "newPassword",
                "oldPassword": "oldPassword"
            }
        }
        
## 被邀请人列表 [/api/invitations?sort=-created]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源

+ Fields
    + userId (int) - 邀请人ID
    + created (date) - 创建时间
    + target - (object) 被邀请人信息
    + target.nickname (string) - 被邀请人昵称

### 获取列表 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "number": 1,
                "size": 10,
                "numberOfElements": 3,
                "last": true,
                "totalPages": 1,
                "sort": [
                    {
                        "direction": "DESC",
                        "property": "created",
                        "ignoreCase": false,
                        "nullHandling": "NATIVE",
                        "ascending": false,
                        "descending": true
                    }
                ],
                "first": true,
                "totalElements": 3
            },
            "links": {
                "self": "/api/invitations?sort=-created&page[number]=1&page[size]=10",
                "first": "/api/invitations?sort=-created&page[number]=1&page[size]=10",
                "last": "/api/invitations?sort=-created&page[number]=1&page[size]=10"
            },
            "data": [
                {
                    "id": 3,
                    "created": "2017-04-07 18:37:23",
                    "userId": 12,
                    "targetId": 10335,
                    "target": {
                        "id": 10335,
                        "nickname": "13000000010",
                        "completed": false
                    }
                },
                {
                    "id": 2,
                    "created": "2017-04-07 18:01:47",
                    "userId": 12,
                    "targetId": 10333,
                    "target": {
                        "id": 10333,
                        "nickname": "13000000007",
                        "completed": false
                    }
                },
                {
                    "id": 1,
                    "created": "2017-04-07 18:00:45",
                    "userId": 12,
                    "targetId": 10332,
                    "target": {
                        "id": 10332,
                        "nickname": "13000000006",
                        "completed": false
                    }
                }
            ]
        }
        
## OAuth2.0 Token [/oauth/token]

+ Description
    + Resource Owner Password Credentials Grant
    
+ Fields
    + access_token (string) - 访问令牌
    + token_type (string) - 此处固定位bearer
    + expires_in (int) - 令牌过期时间

### TOKEN [POST]

+ Request (application/x-www-form-urlencoded)
            
    + Body
    
            {
                "username": "13611019209",
                "password": "admin123"
            }

+ Response 200 (application/json)

    + Headers

            Cache-Control: no-store
            Pragma: no-cache

    + Body

            {
                "access_token": "eyJhbGciOiJIUzUxMiJ9.eyJ1c2VyX2lkIjoiMTIiLCJ1c2VyX25hbWUiOiIxMzYxMTAxOTIwOSIsImNyZWF0ZWQiOjE0OTEwMzk4MTE2NDIsImV4cCI6MTQ5MTA0NzAxMX0.crzuNVKs25BZDCfR0leBq70upQAT8XeAFFO4XAcuH9NSq03KBdzcXUjSvNN304s01jia2ajQ0KWX8egJBNMy4Q",
                "token_type": "bearer",
                "expires_in": 7199
            }
