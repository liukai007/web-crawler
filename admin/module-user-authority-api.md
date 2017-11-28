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

```   
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
```
### 增加新权限 [POST] /user/authorities
+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN

+ Parameters
    + siteId (String) 必填
    + authType (String) 必填 值为 0或者1 为0时会多一个对象存储 权限操作的相关信息
    + name (String) 必填 字符限制 2-100
    + description 非必填 字符限制 0-255
    + priority (int) 非必填 字符限制 -128-127
    + enabled (int) 默认1
    + authOperation 权限操作信息 authType为0时有效
        + id (long) 非必填 权限操作id
        + args (String)  参数列表字符串
        + argsLength (int) 0-255 参数数量
        + authorityId (Long) 非必填 
        + functionClass (String) 必填  最大255字符 符合正则表达式 ([a-zA-Z0-9]+(.[a-zA-Z0-9]+)*)
        + functionSignature (String) 必填  最大50字符 符合正则表达式 ^[a-zA-Z_][a-zA-Z0-9_]*
        + enabled (int) -  是否启用 0：否，1：是
        + creator (long) - 创建人
        + modifier (long) - 修改人
        + created (date) 创建时间
        + modified (date) 修改时间
+ 新增Request (application/json) 
```
    {
        "data":{
            "authType":1,
            "name":"权限测试",
            "siteId":14
      
    }
```
### 修改权限 [PATCH] /user/authorities/{id}
+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN

+ Parameters
    + siteId (String) 必填
    + authType (String) 必填 值为 0或者1 为0时会多一个对象存储 权限操作的相关信息
    + name (String) 必填 字符限制 2-100
    + description 非必填 字符限制 0-255
    + priority (int) 非必填 字符限制 -128-127
    + enabled (int) 默认1
    + authOperation 权限操作信息 authType为0时有效
        + id (long) 非必填 权限操作id
        + args (String)  参数列表字符串
        + argsLength (int) 0-255 参数数量
        + authorityId (Long) 非必填 
        + functionClass (String) 必填  最大255字符 符合正则表达式 ([a-zA-Z0-9]+(.[a-zA-Z0-9]+)*)
        + functionSignature (String) 必填  最大50字符 符合正则表达式 ^[a-zA-Z_][a-zA-Z0-9_]*
        + enabled (int) -  是否启用 0：否，1：是
        
+ 修改Request (application/json)
```  

    {
        "data":{
            "enabled":0
        }
    }
``` 
### 获取权限 [GET] /user/authorities/{id}
+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN

+ Parameters
    + siteId (String) 必填
    + authType (String) 必填 值为 0或者1 为0时会多一个对象存储 权限操作的相关信息
    + name (String) 必填 字符限制 2-100
    + description 非必填 字符限制 0-255
    + priority (int) 非必填 字符限制 -128-127
    + enabled (int) 默认1
    + authOperation 权限操作信息 authType为0时有效
        + id (long) 非必填 权限操作id
        + args (String)  参数列表字符串
        + argsLength (int) 0-255 参数数量
        + authorityId (Long) 非必填 
        + functionClass (String) 必填  最大255字符 符合正则表达式 ([a-zA-Z0-9]+(.[a-zA-Z0-9]+)*)
        + functionSignature (String) 必填  最大50字符 符合正则表达式 ^[a-zA-Z_][a-zA-Z0-9_]*
        + enabled (int) -  是否启用 0：否，1：是
        
+ 修改Request (application/json)
```
{
    "data": {
        "id": 3,
        "enabled": 0,
        "creator": null,
        "modifier": null,
        "created": 1449128107000,
        "modified": 1449804739000,
        "siteId": 1,
        "authType": 0,
        "name": "通天塔3",
        "description": null,
        "authGroup": null,
        "priority": 0,
        "authOperation": {
            "id": 2,
            "enabled": null,
            "creator": null,
            "modifier": null,
            "created": null,
            "modified": null,
            "authorityId": 3,
            "functionClass": "package name111111",
            "functionSignature": "method name11111111",
            "args": null,
            "argsLength": 0
        },
       "permission": "package name111111.method name11111111"
    }
}
```

### 查找某一个角色下的所有权限列表 [GET] /user/authorities/roles/{roleId}
+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN

+ Parameters
    + roleId (long) - 角色id
```  
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
```