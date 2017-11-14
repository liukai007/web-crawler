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