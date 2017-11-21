+ 2017年11月21日
    + API初始化

## votes

+ Data
    + voteType (String) -  投票类型: 'CHECKBOX'：复选框, 'RADIO'：单选,
    + voteText (String) - 投票描述
    + voteStart (int) - 投票开始日期
    + voteLength (String) - 投票期限 0:无限制 (单位天)
    + enabled (int) - 0：删除，1：可用
    + creator (long) - 创建人
    + modifier (long) - 修改人
    + created (date) - 创建时间
    + modified (date) - 修改时间

### 增加投票 [POST] /article/votes

+ Description
    + [MUST] Authenticated
    + [MUST] administrator

+ Parameters
    + voteType -  'CHECKBOX'| 'RADIO' 目前只有暂只有俩
    + voteText  - 
    + voteStart - 格式为yyyy-MM-dd HH:mm:ss
    + voteLength - 默认0
    + enabled - 默认1

+ 新增Request (application/json)
```
        {
            "data":{
                "voteType":"CHECKBOX",
                "voteText":"新增投票",
                "voteStart":"2017-09-13 14:52:41",
                "voteLength":8
            }
        }
```
+ Response 201 (application/json)

    + Headers

            Location: /votes/3
    + Body
```
            {
                "data": {
                    "id": 3,
                    "type": "votes"
                }
            }
```
### 修改投票 [PATCH] /article/votes/{id}

+ Description
    + [MUST] Authenticated
    + [MUST] administrator
+ Parameters
    + voteType -  'CHECKBOX'| 'RADIO' 目前只有暂只有俩
    + voteText  - 
    + voteStart - 格式为yyyy-MM-dd HH:mm:ss
    + voteLength - 默认0
    + enabled - 默认1

+ 修改Request (application/json)

        {
            "data":{
                "voteText":"修改投票"
            }
        }
+ Response 200 (application/json)

### 投票列表 [GET] /article/votes
+ Description
    + 列表展示字段：id,voteType,voteText,voteStart,voteLength,enabled,creator,modifier,created,modified

```
{
    "meta": {
        "totalPages": 1,
        "totalElements": 2,
        "size": 10,
        "number": 1,
        "numberOfElements": 2,
        "first": true,
        "last": true,
        "sort": null
    },
    "links": {
        "self": "/article/votes?page[number]=1&page[size]=10",
        "first": "/article/votes?page[number]=1&page[size]=10",
        "last": "/article/votes?page[number]=1&page[size]=10"
    },
    "data": [
        {
            "id": 1,
            "enabled": 1,
            "creator": 0,
            "modifier": 0,
            "created": "2017-09-05 11:37:48",
            "modified": "2017-09-05 11:37:48",
            "voteType": "CHECKBOX",
            "voteText": "举报原因",
            "voteStart": "2017-09-05 11:37:48",
            "voteLength": 0
        },
        {
            "id": 2,
            "enabled": 1,
            "creator": 0,
            "modifier": 0,
            "created": "2017-09-13 14:52:41",
            "modified": "2017-09-13 14:52:41",
            "voteType": "CHECKBOX",
            "voteText": "精翻部分",
            "voteStart": "2017-09-13 14:52:41",
            "voteLength": 0
        }
    ]
}
```

### 投票详情 [GET] /article/votes/{id}
+ Description
    + voteType -  'CHECKBOX'| 'RADIO' 目前只有暂只有俩
    + voteText  - 
    + voteStart - 格式为yyyy-MM-dd HH:mm:ss
    + voteLength - 默认0
    + options - 投票选项 根据voteType显示该字段
```
{
    "data": {
        "id": 1,
        "voteType": "CHECKBOX",
        "voteText": "举报原因",
        "voteStart": "2017-09-05 11:37:48",
        "voteLength": 0,
        "options": [
            {
                "id": 1,
                "voteOptionText": "反动言论",
                "voteOptionCount": 10
            },
            {
                "id": 2,
                "voteOptionText": "文题不符",
                "voteOptionCount": 10
            },
            {
                "id": 3,
                "voteOptionText": "错别字",
                "voteOptionCount": 1
            },
            {
                "id": 4,
                "voteOptionText": "其他",
                "voteOptionCount": 5
            }
        ]
    }
}
```  

### 删除投票 [DELETE] /article/votes/{id}
+ Description
    + [MUST] Authenticated
    + [MUST] administrator
+ Response 204 (application/json)
+ Response 400 (application/json)

## votes_option
+ Data
    + voteId (Long) - 关联投票ID
    + voteOptionText (String) - 选项描述
    + voteOptionCount (int) - 选项票数
    + displayOrder (int) - 选项排序
    + enabled (int) - 0：删除，1：可用


### 增加投票选项 [POST] /article/votes/option

+ Description
    + [MUST] Authenticated
    + [MUST] administrator

+ Parameters
    + voteId (Long) - 必填 前端控制
    + voteOptionText (String) - 
    + voteOptionCount (int) - 统计用，默认0
    + displayOrder (int) - 取值范围0-255
    + enabled (int) - 0：删除，1：可用

+ 新增Request (application/json)
```
        {
            "data":{
                "voteId":1,
                "voteOptionText":"新增投票选项",
                "displayOrder":8
            }
        }
```
+ Response 201 (application/json)

    + Headers

            Location: /article/votes/ption/3
    + Body
```
            {
                "data": {
                    "id": 3,
                    "type": "VotesOption"
                }
            }
```
### 修改投票选项 [PATCH] /article/votes/option/{id}

+ Description
    + [MUST] Authenticated
    + [MUST] administrator
+ Parameters
    + voteId (Long) - 最好不要修改
    + voteOptionText (String) - 
    + voteOptionCount (int) - 统计用，最好不要修改
    + displayOrder (int) - 取值范围0-255
    + enabled (int) - 0：删除，1：可用

+ 修改Request (application/json)

        {
            "data":{
                "voteOptionText":"修改投票选项"
            }
        }
+ Response 200 (application/json)

### 投票选项列表 [GET] /article/votes/option?filter[voteId]=1
+ Description
    + 列表展示字段：id,voteOptionText,voteOptionCount,displayOrder,enabled
    + 记得添加filter[voteId]={voteId} 该列表为二级列表 根据投票id显示其下的选项列表
```
{
    "meta": {
        "totalPages": 1,
        "totalElements": 4,
        "size": 10,
        "number": 1,
        "numberOfElements": 4,
        "first": true,
        "last": true,
        "sort": null
    },
    "links": {
        "self": "/article/votes/option?filter[voteId]=1&page[number]=1&page[size]=10",
        "first": "/article/votes/option?filter[voteId]=1&page[number]=1&page[size]=10",
        "last": "/article/votes/option?filter[voteId]=1&page[number]=1&page[size]=10"
    },
    "data": [
        {
            "id": 1,
            "enabled": 1,
            "voteId": 1,
            "voteOptionText": "反动言论",
            "voteOptionCount": 10,
            "displayOrder": 1
        },
        {
            "id": 2,
            "enabled": 1,
            "voteId": 1,
            "voteOptionText": "文题不符",
            "voteOptionCount": 10,
            "displayOrder": 2
        },
        {
            "id": 3,
            "enabled": 1,
            "voteId": 1,
            "voteOptionText": "错别字",
            "voteOptionCount": 1,
            "displayOrder": 3
        },
        {
            "id": 4,
            "enabled": 1,
            "voteId": 1,
            "voteOptionText": "其他",
            "voteOptionCount": 5,
            "displayOrder": 4
        }
    ]
}
```

### 投票选项详情 [GET] /article/votes/option/{id}
+ Description
    + voteId (Long) - 关联投票ID
    + voteOptionText (String) - 选项描述
    + voteOptionCount (int) - 选项票数
    + displayOrder (int) - 选项排序
    + enabled (int) - 0：删除，1：可用
```
{
    "data": {
        "id": 1,
        "enabled": 1,
        "voteId": 1,
        "voteOptionText": "反动言论",
        "voteOptionCount": 10,
        "displayOrder": 1
    }
}
```  

### 删除投票选项 [DELETE] /article/votes/option/{id}
+ Description
    + [MUST] Authenticated
    + [MUST] administrator
+ Response 204 (application/json)
+ Response 400 (application/json)