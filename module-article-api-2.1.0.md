FORMAT: 1A
HOST: http://192.168.1.138/

# 米饭平台2.1.0

+ 2017年10月31日
    + 新增种子管理相关URL
    
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


