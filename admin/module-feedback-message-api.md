
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
## (GET|DELETE|PATCH) 留言  [/article/messages/{id}]

+ Description
    + [MUST] ROLE_ADMIN 除了GET
    + [PATCH] 
        + message 不能为空 并且大于等于2个字符小于等于140个字符


+ Parameters
    + id (long) - 留言ID

### 查询留言详情 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 3,
                "enabled": 1,
                "creator": 0,
                "modifier": 0,
                "created": "2017-09-02 15:09:29",
                "modified": "2017-09-02 15:09:29",
                "message": "messagemessagemessagemessage"
            }
        }

### 删除留言 [DELETE]

+ Response 204 (application/json)

### 修改留言 [PATCH]

+ Request (application/json)

        {
            "data": {
                "message": "messagemessagemessagemessage",
                "enabled": 1
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)
