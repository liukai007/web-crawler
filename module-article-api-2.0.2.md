FORMAT: 1A
HOST: http://192.168.1.138/

# topics

+ 2017年9月5日
    + 部分新API增加了模块前缀名称，未来老接口会逐渐过渡到这种格式
        + /article
    + 增加用户收藏主题API
    + 增加留言板API
    + 增加反馈信息API

文章主题模块2.0.2

## (POST|DELETE|PATCH)用户与主题关联关系(收藏) [/users/{userId}/relationships/topics]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + [OPTIONAL] 不写meta的话:/users/{userId}/relationships/topics/favorite
    
+ Meta
    + relationships (string) - favorite:表明这是用户与主题资源多对多关系中的_收藏关联_
    
+ Data
    + id (long) - 主题资源唯一标识符

+ Parameters
    + userId (long) - 用户ID

### 增加关联 [POST]

+ Request (application/json)

        {
            "meta": {
                "relationships": "favorite"
            },
            "data": [
                {
                    "id": "88"
                },
                {
                    "id": "90"
                }
            ]
        }

+ Response 200 (application/json)

+ Response 202 (application/json)

+ Response 204 (application/json)

+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "应用程序code编码",
                    "title": "Bad Request",
                    "detail": "错误信息描述"
                }
            ]
        }

### 删除关联 [DELETE]

+ Request (application/json)

        {
            "meta": {
                "relationships": "favorite"
            },
            "data": [
                {
                    "id": "2"
                },
                {
                    "id": "3"
                }
            ]
        }

+ Response 200 (application/json)

+ Response 202 (application/json)

+ Response 204 (application/json)

+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "应用程序code编码",
                    "title": "Bad Request",
                    "detail": "错误信息描述"
                }
            ]
        }
        
### 修改关联 [PATCH]

如果data数组为空则删除所有关联关系

+ Request (application/json)

        {
            "meta": {
                "relationships": "favorite"
            },
            "data": [
                {
                    "id": "88"
                },
                {
                    "id": "90"
                }
            ]
        }
        
+ Response 200 (application/json)

+ Response 202 (application/json)

+ Response 204 (application/json)

+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "应用程序code编码",
                    "title": "Bad Request",
                    "detail": "错误信息描述"
                }
            ]
        }
        
## (POST|GET) 留言板 [/article/messages]

+ Description
    + sort=-created 按创建时间降序排序查询列表
    
+ Data
    + message (string) - 用户留言

### 新增留言 [POST]

+ Request (application/json)

        {
            "data": {
                "message": "米饭网挺好"
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /article/messages/1

    + Body

            {
                "data": {
                    "id": 1,
                    "type": "messages"
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
        
### 查询留言板集合 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "totalPages": 1,
                "totalElements": 3,
                "size": 10,
                "number": 1,
                "numberOfElements": 3,
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
                "self": "/article/messages?sort=-created&page[number]=1&page[size]=10",
                "first": "/article/messages?sort=-created&page[number]=1&page[size]=10",
                "last": "/article/messages?sort=-created&page[number]=1&page[size]=10"
            },
            "data": [
                {
                    "id": 3,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-09-02 15:09:29",
                    "modified": "2017-09-02 15:09:29",
                    "message": "messagemessagemessagemessage"
                },
                {
                    "id": 2,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-09-02 15:09:27",
                    "modified": "2017-09-02 15:09:27",
                    "message": "messagemessagemessagemessage"
                },
                {
                    "id": 1,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-09-02 15:09:24",
                    "modified": "2017-09-02 15:09:24",
                    "message": "messagemessagemessagemessage"
                }
            ]
        }   
        
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
        
### 查询留言板集合 [GET]

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
        
