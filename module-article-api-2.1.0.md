FORMAT: 1A
HOST: http://192.168.1.138/

# 米饭平台2.1.0

+ 2017年11月1日
    + 新增类别相关API

+ 2017年10月31日
    + 新增种子相关API
    
## (POST|GET)爬虫种子网站 [/article/seeds]

+ Description
    + [MUST] ROLE_ADMIN

+ Field
    + url (string) - 种子URL
    + source (string) - 种子来源名称/简称
    + conf (string) - 种子配置, JSON格式的大文本
    + name (string, nullable) - 类型名称 e.g products产品 | brands品牌
    + agencyIp (string, nullable) - 代理IP
    + agencyIpPort (string, nullable) - 代理端口
    + charset (string, nullable) - 字符集你
    + description (string, nullable) - 种子描述
    + language (string) - 语言 1:中文, 2英文

+ Meta
    + number (int) - 当前页
    + size (int) - 每页大小
    + numberOfElements (int) - 当前页有多少记录
    + last (boolean) - 是否是最后一页
    + totalPages (int) - 总页数
    + sort (object) - 排序相关信息
    + first (boolean) - 是否是第一页
    + totalElements - 总记录数

### 新增种子 [POST]

+ Request (application/json)

        {
            "data": {
                "url": "https://www.sweetwater.com/store/detail/KM183",
                "source": "Sweetwater",
                "conf": "{}",
                "name": "products",
                "agencyIp": "192.168.1.234",
                "agencyIpPort": 9000,
                "charset": "UTF-8",
                "description": "甜水网",
                "language": 1
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /article/seeds/1

    + Body

            {
                "data": {
                    "id": 1,
                    "type": "seeds"
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

### 查询种子列表 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "totalPages": 8,
                "totalElements": 74,
                "size": 10,
                "number": 1,
                "numberOfElements": 10,
                "first": true,
                "last": false,
                "sort": null
            },
            "links": {
                "self": "/article/seeds?page[number]=1&page[size]=10",
                "first": "/article/seeds?page[number]=1&page[size]=10",
                "next": "/article/seeds?page[number]=2&page[size]=10",
                "last": "/article/seeds?page[number]=8&page[size]=10"
            },
            "data": [
                {
                    "id": 1,
                    "enabled": 0,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2016-06-16 10:29:36",
                    "modified": "2017-10-31 09:57:01",
                    "url": "http://www.music123.com/browse/shopByBrand/shopByBrand.jsp",
                    "source": "Music123",
                    "conf": "",
                    "name": "products",
                    "description": "Music123",
                    "rank": 500,
                    "updateRate": 1,
                    "language": 2
                },
                {
                    "id": 2,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "url": "https://www.sweetwater.com/store/detail/KM183",
                    "source": "Sweetwater",
                    "conf": "",
                    "name": "products",
                    "description": "甜水网",
                    "rank": 0,
                    "updateRate": 1,
                    "language": 2
                },
                {
                    "id": 3,
                    "enabled": 0,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2016-06-22 10:29:36",
                    "modified": "2016-06-22 10:29:36",
                    "url": "http://www.guitarcenter.com",
                    "source": "Guitar Center",
                    "conf": "",
                    "name": "products",
                    "description": "GuitarCenter",
                    "rank": 400,
                    "updateRate": 1,
                    "language": 2
                }
            ]
        }

## (GET|DELETE|PATCH)爬虫种子网站 [/article/seeds/{id}]

+ Description
    + [MUST] ROLE_ADMIN

+ Parameters
    + id (long) - 种子ID

### 查询种子详情 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 1,
                "enabled": 0,
                "creator": 0,
                "modifier": 0,
                "created": "2016-06-16 10:29:36",
                "modified": "2017-10-31 09:57:01",
                "url": "http://www.music123.com/browse/shopByBrand/shopByBrand.jsp",
                "source": "Music123",
                "conf": "",
                "name": "products",
                "description": "Music123",
                "rank": 500,
                "updateRate": 1,
                "language": 2
            }
        }

### 删除种子 [DELETE]

+ Response 204 (application/json)

### 修改种子[PATCH]

+ Request (application/json)

        {
            "data": {
                "url": "https://www.sweetwater.com/store/detail/KM183",
                "source": "Sweetwater",
                "conf": "{}",
                "name": "products",
                "agencyIp": "192.168.1.234",
                "agencyIpPort": 9000,
                "charset": "UTF-8",
                "description": "甜水网",
                "language": 1
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)

## (POST|GET)类别 [/article/forumCategories]

+ Description
    + [MUST] ROLE_ADMIN

+ Field
    + forumId (long) - forum ID
    + parentId (long) - 父节点ID
    + title (string) - 类别名称
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

        {
            "data": {
                "forumId": 1,
                "parentId": 2147,
                "title": "吉他和贝斯",
                "displayOrder": 0
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /article/forumCategories/1

    + Body

            {
                "data": {
                    "id": 1,
                    "type": "forumCategories"
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

### 查询类别列表 [GET]

+ Response 200 (application/json)

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
                "path": "1",
                "depth": 1,
                "leaf": 0,
                "displayOrder": 0
            }
        }

### 删除类别 [DELETE]

+ Response 204 (application/json)

### 修改类别[PATCH]

+ Request (application/json)

        {
            "data": {
                "id": 1,
                "enabled": 1,
                "title": "Accessories",
                "displayOrder": 0
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)

## (PATCH)主题主推数/加入撤销到样本集 [/topics/{id}]

+ Description
    + [MUST] ROLE_ADMIN

+ Field
    + boost (double) - 搜索引擎助推数, 值越高排名越靠前
    + trainSample (int) - 是否加入到训练样本集中, 0:否, 1:是

### 修改主题[PATCH]

+ Request (application/json)

        {
            "data": {
                "boost": 1,
                "trainSample": 1
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)

