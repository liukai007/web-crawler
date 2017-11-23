
## (POST|GET) 用户文章举报 [/users/relationships/topics/report]

+ Description
    + sort=-created 按创建时间降序排序查询列表
    
+ Data
    + userId (long) - 用户留言
    + topicId (long) - 用户留言
    + remark (string) - 举报原因
    + options (array) - 举报选项 前端需取voteOptions用以替换
        
### 查询文章举报集合 [GET]

+ Response 200 (application/json)
```
{
    "meta": {
        "totalPages": 1,
        "totalElements": 3,
        "size": 10,
        "number": 1,
        "numberOfElements": 3,
        "first": true,
        "last": true,
        "sort": null
    },
    "links": {
        "self": "/users/relationships/topics/report?page[number]=1&page[size]=10",
        "first": "/users/relationships/topics/report?page[number]=1&page[size]=10",
        "last": "/users/relationships/topics/report?page[number]=1&page[size]=10"
    },
    "data": [
        {
            "id": 2,
            "enabled": 1,
            "created": "2017-09-26 13:33:15",
            "modified": "2017-09-26 13:33:15",
            "userId": 2370,
            "topicId": 572449,
            "remark": "fsdf ",
            "options": [
                2
            ]
        },
        {
            "id": 3,
            "enabled": 1,
            "created": "2017-09-27 18:37:15",
            "modified": "2017-09-27 18:37:15",
            "userId": 2370,
            "topicId": 508283,
            "remark": "环境",
            "options": [
                2
            ]
        },
        {
            "id": 4,
            "enabled": 1,
            "created": "2017-10-14 14:11:54",
            "modified": "2017-10-14 14:11:54",
            "userId": 1101,
            "topicId": 619739,
            "remark": "是打发似懂非懂发",
            "options": [
                1,
                2
            ]
        }
    ]
}
```
## (GET|DELETE) 举报  [/users/relationships/topics/report/{id}]

+ Description
    + [MUST] ROLE_ADMIN 

+ Parameters
    + id (long) - 举报ID

### 查询举报详情 [GET]

+ Response 200 (application/json)
```
        {
            "data": {
                "id": 3,
                "enabled": 1,
                "created": "2017-09-27 18:37:15",
                "modified": "2017-09-27 18:37:15",
                "userId": 2370,
                "topicId": 508283,
                "remark": "环境",
                "options": [
                        2
                ]
            }
        }
```
### 删除举报 [DELETE]

+ Response 204 (application/json)
