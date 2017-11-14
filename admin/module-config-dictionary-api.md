## (POST|GET)双语词典 [/article/wordDictionary]

+ Description
    + [MUST] ROLE_ADMIN

+ Field
    + wordEn (string) - 英文词
    + wordEn (string) - 中文词

+ Meta
    + number (int) - 当前页
    + size (int) - 每页大小
    + numberOfElements (int) - 当前页有多少记录
    + last (boolean) - 是否是最后一页
    + totalPages (int) - 总页数
    + sort (object) - 排序相关信息
    + first (boolean) - 是否是第一页
    + totalElements - 总记录数

### 新增双语词典 [POST]

+ Request (application/json)

        {
            "data": {
                "wordEn": "accessories",
                "wordCn": "配件"
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /article/wordDictionary/1

    + Body

            {
                "data": {
                    "id": 1,
                    "type": "wordDictionary"
                }
            }
            
+ Response 204 (application/json)
        
+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "some code",
                    "title": "Bad Request",
                    "detail": "格式错误",
                    "source": {
                        "pointer": "{class} -> {field}"
                    }
                }
            ]
        }

### 查询双语词典列表 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "totalPages": 270,
                "totalElements": 2694,
                "size": 10,
                "number": 1,
                "numberOfElements": 10,
                "first": true,
                "last": false,
                "sort": null
            },
            "links": {
                "self": "/article/wordDictionary?page[number]=1&page[size]=10",
                "first": "/article/wordDictionary?page[number]=1&page[size]=10",
                "next": "/article/wordDictionary?page[number]=2&page[size]=10",
                "last": "/article/wordDictionary?page[number]=270&page[size]=10"
            },
            "data": [
                {
                    "id": 1,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-10-31 18:30:30",
                    "modified": "2017-10-31 18:30:30",
                    "wordEn": "accessories",
                    "wordCn": "配件"
                },
                {
                    "id": 2,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-10-31 18:30:31",
                    "modified": "2017-10-31 18:30:31",
                    "wordEn": "accessories for mics",
                    "wordCn": "麦克风配件"
                },
                {
                    "id": 3,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-10-31 18:30:31",
                    "modified": "2017-10-31 18:30:31",
                    "wordEn": "booms & handles",
                    "wordCn": "吊杆和手柄配件"
                }
            ]
        }
        
## (GET|DELETE|PATCH)双语词典  [/article/wordDictionary/{id}]

+ Description
    + [MUST] ROLE_ADMIN

+ Parameters
    + id (long) - 标识符ID

### 查询双语词典详情 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 1,
                "enabled": 1,
                "creator": 0,
                "modifier": 0,
                "created": "2017-10-31 18:30:30",
                "modified": "2017-10-31 18:30:30",
                "wordEn": "accessories",
                "wordCn": "配件"
            }
        }

### 删除双语词典 [DELETE]

+ Response 204 (application/json)

### 修改双语词典 [PATCH]

+ Request (application/json)

        {
            "data": {
                "wordEn": "accessories",
                "wordCn": "配件"
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)