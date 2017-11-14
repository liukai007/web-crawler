## (POST|GET) 分类模型 [/article/topicsModel]

+ Description
    + [MUST] ROLE_ADMIN
    + [POST] 新增暂时只需要以下字段
        + forumId
        + modelName

+ Field
    + id (long) - 模型ID
    + forumId (long) - 版块ID
    + modelName (string) - 模型名称, 全局应该唯一
    + modelStatus (string) - 模型状态枚举
    + modelStatusValue (string) - 模型状态枚举中文描述
    + priority (int) - 模型优先级, 取值[0,100]
    + confRandomSelection (int) - [5, 50] 测试数据占样本数据百分比
    + confOverwrite (int) - 是否覆盖 0:否, 1:是
    + resultSamples (int) - 模型样本总数
    + resultText (string, nullable) - 模型分析结果数据或一样信息

+ Meta
    + number (int) - 当前页
    + size (int) - 每页大小
    + numberOfElements (int) - 当前页有多少记录
    + last (boolean) - 是否是最后一页
    + totalPages (int) - 总页数
    + sort (object) - 排序相关信息
    + first (boolean) - 是否是第一页
    + totalElements - 总记录数

### 新增分类模型 [POST]

+ Request (application/json)

        {
            "data": {
                "forumId": 1,
                "modelName": "topic-1"
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /article/topicsModel/1

    + Body

            {
                "data": {
                    "id": 1,
                    "type": "topicsModel"
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

### 查询分类模型列表 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "totalPages": 1,
                "totalElements": 1,
                "size": 10,
                "number": 1,
                "numberOfElements": 1,
                "first": true,
                "last": true,
                "sort": null
            },
            "links": {
                "self": "/article/topicsModel?page[number]=1&page[size]=10",
                "first": "/article/topicsModel?page[number]=1&page[size]=10",
                "last": "/article/topicsModel?page[number]=1&page[size]=10"
            },
            "data": [
                {
                    "id": 1,
                    "enabled": 1,
                    "creator": 11,
                    "modifier": 11,
                    "created": "2017-11-08 16:26:38",
                    "modified": "2017-11-08 16:32:10",
                    "forumId": 1,
                    "modelStatus": "RUNNABLE",
                    "modelName": "topic-2",
                    "priority": 0,
                    "confRandomSelection": 20,
                    "confOverwrite": 1,
                    "resultSamples": 0,
                    "modelStatusValue": "模型训练中"
                }
            ]
        }
        
## (GET|DELETE|PATCH) 分类模型  [/article/topicsModel/{id}]

+ Description
    + [MUST] ROLE_ADMIN
    + [PATCH] 后台对修改模型做了如下限制, 强制修改会返回错误提示信息
        + modelName 模型名称不能修改
        + forumId 不能修改
        + modelStatus 状态为RUNNABLE的模型不能修改
    + 执行训练模型
        + 将状态修改为RUNNABLE

+ Parameters
    + id (long) - 模型ID

### 查询分类模型详情 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 1,
                "enabled": 1,
                "creator": 11,
                "modifier": 11,
                "created": "2017-11-08 16:26:38",
                "modified": "2017-11-08 16:26:38",
                "forumId": 1,
                "modelStatus": "NEW",
                "modelName": "topic-1",
                "priority": 0,
                "confRandomSelection": 20,
                "confOverwrite": 1,
                "resultSamples": 0,
                "modelStatusValue": "新建"
            }
        }

### 删除分类模型 [DELETE]

+ Response 204 (application/json)

### 修改分类模型 [PATCH]

+ Request (application/json)

        {
            "data": {
                "priority": 22,
                "modelStatus": "RUNNABLE"
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)
