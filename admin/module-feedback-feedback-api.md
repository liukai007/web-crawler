## (POST|GET) 反馈 [/article/feedback]

+ Description
    + [MUST] Authenticated
    + <del>feedback没有单独的复数表述形式</del>
    
+ Data
    + username (string) - 姓名
    + mobile (string) - 手机号, 联系方式
    + email (string) - 邮箱
    + suggestion (string) - 反馈建议信息

### 新增反馈 [POST]

+ Request (application/json)

        {
            "data": {
                "username": "张三",
                "mobile": "13523456434",
                "email": "12323@qq.com",
                "suggestion": "反馈建议信息"
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /article/feedback/1

    + Body

            {
                "data": {
                    "id": 1,
                    "type": "feedback"
                }
            }
            
+ Response 204 (application/json)
        
+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "Pattern.Messages.message",
                    "title": "Bad Request",
                    "detail": "留言字符长度应在2~140之间",
                    "source": {
                        "pointer": "Messages -> message"
                    }
                }
            ]
        }
        
### 查询反馈列表集合 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "totalPages": 1,
                "totalElements": 4,
                "size": 10,
                "number": 1,
                "numberOfElements": 4,
                "first": true,
                "last": true,
                "sort": [
                    {
                        "direction": "DESC",
                        "property": "created",
                        "ignoreCase": false,
                        "nullHandling": "NATIVE",
                        "ascending": false,
                        "descending": true
                    }
                ]
            },
            "links": {
                "self": "/article/feedback?sort=-created&page[number]=1&page[size]=10",
                "first": "/article/feedback?sort=-created&page[number]=1&page[size]=10",
                "last": "/article/feedback?sort=-created&page[number]=1&page[size]=10"
            },
            "data": [
                {
                    "id": 4,
                    "enabled": 1,
                    "creator": 11,
                    "modifier": 11,
                    "created": "2017-09-02 15:07:14",
                    "modified": "2017-09-02 15:07:14",
                    "username": "quzile",
                    "mobile": "17611019209",
                    "email": "1929593@qq.com",
                    "suggestion": "建议22333233"
                },
                {
                    "id": 3,
                    "enabled": 1,
                    "creator": 11,
                    "modifier": 11,
                    "created": "2017-09-02 15:06:51",
                    "modified": "2017-09-02 15:06:51",
                    "username": "quzile",
                    "mobile": "17611019209",
                    "email": "1929593@qq.com",
                    "suggestion": "建议22333233"
                }
            ]
        }

## (GET|DELETE|PATCH) 反馈  [/article/feedback/{id}]

+ Description
    + [MUST] ROLE_ADMIN 除了GET
    + [PATCH] 
        + username 大于等于1个字符小于等于100个字符
        + mobile 符合正则表达式(^1(3[0-9]|4[579]|5[0-35-9]|7[0135678]|8[0-9])\d{8}$)
        + email 符合正则表达式(^([a-z0-9A-Z]+[-|\.|_]?)+[a-z0-9A-Z]@([a-z0-9A-Z]+(-[a-z0-9A-Z]+)?\.)+[a-zA-Z]{2,}$) 且大于等于2个字符小于等于255个字符
        + suggestion 大于等于1个字符小于等于2048个字符

+ Data
    + username (string) - 姓名
    + mobile (string) - 手机号, 联系方式
    + email (string) - 邮箱
    + suggestion (string) - 反馈建议信息

+ Parameters
    + id (long) - 反馈ID

### 查询反馈详情 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 3,
                "enabled": 1,
                "creator": 11,
                "modifier": 11,
                "created": "2017-09-02 15:06:51",
                "modified": "2017-09-02 15:06:51",
                "username": "quzile",
                "mobile": "17611019209",
                "email": "1929593@qq.com",
                "suggestion": "建议22333233"
            }
        }

### 删除反馈 [DELETE]

+ Response 204 (application/json)

### 修改反馈 [PATCH]

+ Request (application/json)

        {
            "data": {
                "username": "quzile",
                "mobile": "17611019209",
                "email": "1929593@qq.com",
                "suggestion": "建议22333233"
                "enabled": 1
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)