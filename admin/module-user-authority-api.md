## 权限管理
+ Data
    + id (long)
    + siteId (long) - 站点id
    + authType (int) - 权限类型 
    + name (String) - 权限名
    + description (String) - 描述
    + priority (int) - 优先级
    + permission (String) - 权限细则
    + enabled (int) -  是否启用 0：否，1：是
    + created (date) 创建时间
    + modified (date) 修改时间
    
### 查询权限列表 [GET] /user/authorities
+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN

    
    {
        "meta": {
            "totalPages": 30,
            "totalElements": 60,
            "size": 2,
            "number": 1,
            "numberOfElements": 2,
            "first": true,
            "last": false,
            "sort": null
        },
        "links": {
            "self": "/user/authorities?page[number]=1&page[size]=2",
            "first": "/user/authorities?page[number]=1&page[size]=2",
            "next": "/user/authorities?page[number]=2&page[size]=2",
            "last": "/user/authorities?page[number]=30&page[size]=2"
        },
        "data": [
            {
                "id": 10,
                "enabled": 1,
                "created": "2015-12-15 14:36:17",
                "modified": "2016-01-18 14:21:39",
                "siteId": 2,
                "authType": 0,
                "name": "sites:create",
                "description": "站点",
                "priority": 0,
                "permission": "sites:create"
            },
            {
                "id": 11,
                "enabled": 1,
                "created": "2015-12-15 14:36:43",
                "modified": "2015-12-22 19:03:12",
                "siteId": 2,
                "authType": 0,
                "name": "sites:delete",
                "priority": 0,
                "permission": "sites:delete"
            }
        ]
    }

### 增加新权限 [POST] /user/authorities
+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN

+ Parameters
    + siteId 必填
    + authType 必填
    + name 必填 字符限制 2-100
    + description 非必填 字符限制 0-255
    + priority 非必填 字符限制 -128-127
    + enabled 默认1
+ 新增Request (application/json)
    

    {
        "data":{
            "authType":0,
            "name":"权限测试",
            "siteId":14
        }
    }
### 修改权限 [PATCH] /user/authorities/{id}
+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN

+ Parameters
    + siteId
    + authType
    + name
    + description
    + priority
    + enabled
+ 修改Request (application/json)
    

    {
        "data":{
            "enabled":0
        }
    }

### 查找某一个角色下的所有权限列表 [GET] /user/authorities/roles/{roleId}
+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN

+ Parameters
    + roleId (long) - 角色id

    
    {
        "data": [
            {
                "id": 61,
                "siteId": 11,
                "name": "dashboard-AdminViewController",
                "permission": "dashboard-AdminViewController"
            }
        ]
    }