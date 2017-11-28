
+ 2017年11月24日
    + 增加filename和titleChinese
    
## (POST|GET)类别 [/article/forumCategories]

+ Description
    + [MUST] ROLE_ADMIN 除了GET

+ Field
    + forumId (long) - forum ID
    + parentId (long) - 父节点ID
    + title (string) - 类别名称
    + filename (string) - 类别图片地址
    + titleChinese (string) - 类别中文 
    + displayOrder (int, nullable) - 排序字段, 可选

+ Meta
    + number (int) - 当前页
    + size (int) - 每页大小
    + numberOfElements (int) - 当前页有多少记录
    + last (boolean) - 是否是最后一页
    + totalPages (int) - 总页数
    + sort (object) - 排序相关信息
    + first (boolean) - 是否是第一页
    + totalElements - 总记录数

### 新增类别 [POST]

+ Request (application/json)
```
        {
            "data": {
                "forumId": 1,
                "parentId": 2147,
                "title": "吉他和贝斯",
                "filename": "http://static.budee.com/iyyren/image/12313.jpg"
                "displayOrder": 0
            }
        }
```
+ Response 201 (application/json)

    + Headers

            Location: /article/forumCategories/1

    + Body
```
            {
                "data": {
                    "id": 1,
                    "type": "forumCategories"
                }
            }
```           
+ Response 204 (application/json)
        
+ Response 400 (application/json)
```
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
```
### 查询类别列表 [GET]

+ Response 200 (application/json)
```
        {
            "meta": {
                "totalPages": 215,
                "totalElements": 2143,
                "size": 10,
                "number": 1,
                "numberOfElements": 10,
                "first": true,
                "last": false,
                "sort": null
            },
            "links": {
                "self": "/article/forumCategories?page[number]=1&page[size]=10",
                "first": "/article/forumCategories?page[number]=1&page[size]=10",
                "next": "/article/forumCategories?page[number]=2&page[size]=10",
                "last": "/article/forumCategories?page[number]=215&page[size]=10"
            },
            "data": [
                {
                    "id": 1,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 11,
                    "created": "2017-10-31 17:07:52",
                    "modified": "2017-11-01 13:16:01",
                    "forumId": 1,
                    "rootId": 1,
                    "parentId": 0,
                    "title": "Accessories",
                    "path": "1",
                    "depth": 1,
                    "leaf": 0,
                    "displayOrder": 0
                },
                {
                    "id": 2,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-10-31 17:07:52",
                    "modified": "2017-10-31 17:07:52",
                    "forumId": 1,
                    "rootId": 1,
                    "parentId": 1,
                    "title": "Accessories for Mics",
                    "path": "1.2",
                    "depth": 2,
                    "leaf": 0,
                    "displayOrder": 0
                },
                {
                    "id": 3,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-10-31 17:07:52",
                    "modified": "2017-11-01 11:46:46",
                    "forumId": 1,
                    "rootId": 1,
                    "parentId": 2,
                    "title": "Booms & Handles",
                    "path": "1.2.3",
                    "depth": 3,
                    "leaf": 1,
                    "displayOrder": 0
                }
            ]
        }        
```
## (GET|DELETE|PATCH)类别 [/article/forumCategories/{id}]

+ Description
    + [MUST] ROLE_ADMIN
    
+ Field
    + id (long) - 类别ID标识符
    + forumId (long) - forum ID
    + rootId (long) - 根节点ID
    + parentId (long) - 父节点ID
    + title (string) - 类别名称
    + path (string) - 节点路径
    + depth (int) - 深度
    + leaf (int) - 是否是叶子节点 0:否, 1:是
    + displayOrder (int) - 排序字段

+ Parameters
    + id (long) - 类别ID

### 查询类别详情 [GET]

+ Response 200 (application/json)
```
{
    "data": {
        "id": 1,
        "enabled": 1,
        "creator": 0,
        "modifier": 11,
        "created": "2017-10-31 17:07:52",
        "modified": "2017-11-01 13:16:01",
        "forumId": 1,
        "rootId": 1,
        "parentId": 0,
        "title": "Accessories",
        "filename": "http://static.budee.com/iyyren/image/12313.jpg",
        "path": "1",
        "depth": 1,
        "leaf": 0,
        "displayOrder": 0,
        "titleChinese": "配件"
    }
}
```
### 删除类别 [DELETE]

+ Response 204 (application/json)

### 修改类别[PATCH]

+ Request (application/json)
```
        {
            "data": {
                "id": 1,
                "enabled": 1,
                "title": "Accessories",
                "displayOrder": 0
            }
        }
```      
+ Response 200 (application/json)

+ Response 204 (application/json)