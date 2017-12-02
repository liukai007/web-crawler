FORMAT: 1A
HOST: http://192.168.1.138/

# topics

+ 2017年12月2日
    + 修改/topics/{id} API, 修改搜索API, 调整了from对象的属性
        + channelId -> 变更为 targetId
        + 增加type属性以区别targetId指向的资源类型, 可选值 users|channels

+ 2017年12月1日
    + 搜索API增加按category ID进行过滤的filter

+ 2017年9月29日
    + search API 修改, 具体见API排序部分

+ 2017年9月27日
    + 搜索API增加一个优化, 可以控制返回何种聚合数据, agg[mask]参数
        + 4bit长度的掩码, e.g : agg[mask]=3 将返回category与brand聚合
        + 参考搜索API

+ 2017年9月14日
    + 搜索API增加一个优化, 可以控制是否需要返回聚合数据, agg[size]参数
        + 小于0时 - 不返回聚合数据, e.g : -1
        + 等于0时 - 服务端控制聚合数据size大小
        + 大于0时 - 客户端控制聚合数据size大小

+ 2017年9月12日
    + 增加价格单位符号 priceSign 字段
        + 主题搜索API
        + 主题详情API

+ 2017年9月8日
    + [FIXED] 查看浏览记录API现在可以按filter[forumId]进行过滤了
    + [FIXED] 收藏列表现在可以按filter[forumId]进行过滤了
    + 主题搜索API与主题详情API增加新字段
        + from对象中增加image字段

+ 2017年9月6日
    + /channels API 增加host字段, 来满足__网站合作__API数据
        + 当频道类型为来源/网站频道时, host表示来源的主页地址
    + /channels API 简化
        + 登陆用户查询频道列表时不需要显式声明fields[]字段, 服务端根据条件使用最小数据集
    + /channels API 优化
        + 排序:使用索引排序
        + 缓存:第一页数据现有缓存

+ 2017年9月5日
    + 部分API增加新的返回字段: favorite 是否收藏
        + 搜索主题列表API
        + 查看详细主题API

+ 2017年8月24日
    + 搜索热词API /topics/keywords 返回内容增加字段
        + value : 现在有值了
        + score : 归一化
    + 查询相关主题API topics/1/relates 调整
        + 增加过滤条件 ?filter[forumId]=1,3,4
        + 增加过滤条件 ?filter[tag]=ACC-TI2POLOAR
    + 查询相关主题API topics/1/relates 
        + 增加了分页参数 page[number] 

+ 2017年8月23日
    + 增加搜索热词API /topics/keywords
        + 算法有待调整
    + /topics/search 搜索API增加新的排序参数 
        + sort=-hot, 按热度降序排序
        + sort=-new, 按新品降序排序
        + sort=-best, 按精选降序排序

+ 2017年8月19日
    + 增加用户浏览历史API  /topics/histories

+ 2017年8月18日
    + 搜索API /topics/search 增加按频道过滤 e.g /topics/search?filter[channel]={channelId}
        + filter[channel] | filter[channelId]

+ 2017年8月16日
    + 增加 /channels/users/{userId} API, 根据用户ID获取频道信息
    + /channels API 增加根据订阅类型过滤并查看是否订阅功能

+ 2017年8月15日
    + 修改/channels API, 详细信息请看描述
    + 增加/channels/{id} API
    + [FIXED] 修复watch不能正常计数的BUG

+ 2017年8月12日 (BUG FIXED)
    + [FIXED] 修改建立索引时查询附件的错误, 正文中的图片附件不参与索引;
    + [FIXED] 搜索API与MLT API增加过滤用户隐藏的功能

+ 2017年8月12日
    + 聚合统计相关API, 参数进行了简化, 优先使用指定语言, 否则服务器选择一个默认语言;
        + [OPTIONAL] catetories.en.raw 可简化为 catetories.raw
        + [OPTIONAL] level\_0.en.raw 可简化为 level\_0.raw
    + 增加一个品牌专属的详情API, 原可[OPTIONAL]选方案为/topics/{id}
        + /brands/{name}

+ 2017年8月11日
    + 主题详情与搜索API（改动）
        + 增加postType属性, 对应于postTypeValue的整型枚举值;
        + 增加similarities, 相似主题列表;
        + 修改属性 userLike -> liked, 由boolean -> int类型: -1:踩, 0:缺省, 1:赞

+ 2017年8月10日
    + /links API 增加了一个新的字段 blank ,用来表明是否在新窗口打开
    + /links API 增加了广告类型

+ 2017年8月4日
    + 增加产品比较API, 用来替换旧API
    + 增加搜索API按价按区间过滤条件描述
    + 增加>,>=,<,<=过滤机制

+ 2017年8月3日
    + 搜索引擎API更新
    + 原聚合API结构全部更新, 前查看搜索API
    + 相加相关主题API, 用来替换旧的more like this API

+ 2017年7月26日
    + 增加搜索/聚合API /topics/search (未完成)

+ 2017年7月22日
    + 修复channels API描述

+ 2017年7月18日
    + 主题详情API结构变化

+ 2017年7月12日
    + 多对多关联关系API调整, 由于数据绑定实现问题, 不依赖于具体的实体对象属性, 增加了attributes对象;
    + 链接API重构为符合rest规范的格式;
    + 颜色API重构为符合rest规范的格式;
    + 评论API移动到评论模块文档中;

+ 2017年6月30日
    + API初始化

文章主题模块

## (POST|DELETE|PATCH)用户与主题关联关系(隐藏) [/users/{userId}/relationships/topics]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + [OPTIONAL] 不写meta的话:/users/{userId}/relationships/topics/hide
    
+ Meta
    + relationships (string) - hide:表明这是用户与主题资源多对多关系中的_隐藏关联_
    
+ Data
    + id (long) - 主题资源唯一标识符
    + attributes (object, nullable) - 资源属性
    + attributes.ableremark (string, nullable) - 隐藏原因

+ Parameters
    + userId (long) - 用户ID

### 增加关联 [POST]

+ Request (application/json)

        {
            "meta": {
                "relationships": "hide"
            },
            "data": [
                {
                    "id": "88",
                    "attributes": {
                        "remark": "文章抄袭!"
                    }
                },
                {
                    "id": "90",
                    "attributes": {
                        "remark": "文章涉及政治与暴力!"
                    }
                }
            ]
        }

+ Response 200 (application/json)

+ Response 202 (application/json)

+ Response 204 (application/json)

+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "应用程序code编码",
                    "title": "Bad Request",
                    "detail": "错误信息描述"
                }
            ]
        }

### 删除关联 [DELETE]

+ Request (application/json)

        {
            "meta": {
                "relationships": "hide"
            },
            "data": [
                {
                    "id": "2"
                },
                {
                    "id": "3"
                }
            ]
        }

+ Response 200 (application/json)

+ Response 202 (application/json)

+ Response 204 (application/json)

+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "应用程序code编码",
                    "title": "Bad Request",
                    "detail": "错误信息描述"
                }
            ]
        }
        
### 修改关联 [PATCH]

如果data数组为空则删除所有关联关系

+ Request (application/json)

        {
            "meta": {
                "relationships": "hide"
            },
            "data": [
                {
                    "id": "88",
                    "attributes": {
                        "remark": "文章抄袭!"
                    }
                },
                {
                    "id": "90",
                    "attributes": {
                        "remark": "文章涉及政治与暴力!"
                    }
                }
            ]
        }
        
+ Response 200 (application/json)

+ Response 202 (application/json)

+ Response 204 (application/json)

+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "应用程序code编码",
                    "title": "Bad Request",
                    "detail": "错误信息描述"
                }
            ]
        }
        
## (POST|DELETE|PATCH)用户与主题关联关系(喜欢) [/users/{userId}/relationships/topics]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + [OPTIONAL] 不写meta的话:/users/{userId}/relationships/topics/like

+ Meta
    + relationships (string) - hide:表明这是用户与主题资源多对多关系中的_喜欢关联_

+ Data
    + id (long) - 主题资源唯一标识符
    + attributes (object) - 资源属性
    + attributes.up (int) - 1:喜欢, 0:不喜欢

+ Parameters
    + userId (long) - 用户ID

### 增加关联 [POST]

+ Request (application/json)

        {
            "meta": {
                "relationships": "like"
            },
            "data": [
                {
                    "id": "88",
                    "attributes": {
                        "up": 1
                    }
                },
                {
                    "id": "90",
                    "attributes": {
                        "up": 0
                    }
                }
            ]
        }

+ Response 200 (application/json)

+ Response 202 (application/json)

+ Response 204 (application/json)

+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "应用程序code编码",
                    "title": "Bad Request",
                    "detail": "错误信息描述"
                }
            ]
        }

### 删除关联 [DELETE]

+ Request (application/json)

        {
            "meta": {
                "relationships": "like"
            },
            "data": [
                {
                    "id": "2"
                },
                {
                    "id": "3"
                }
            ]
        }

+ Response 200 (application/json)

+ Response 202 (application/json)

+ Response 204 (application/json)

+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "应用程序code编码",
                    "title": "Bad Request",
                    "detail": "错误信息描述"
                }
            ]
        }
        
### 修改关联 [PATCH]

如果data数组为空则删除所有关联关系

+ Request (application/json)

        {
            "meta": {
                "relationships": "like"
            },
            "data": [
                {
                    "id": "88",
                    "attributes": {
                        "up": 1
                    }
                },
                {
                    "id": "90",
                    "attributes": {
                        "up": 0
                    }
                }
            ]
        }
        
+ Response 200 (application/json)

+ Response 202 (application/json)

+ Response 204 (application/json)

+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "应用程序code编码",
                    "title": "Bad Request",
                    "detail": "错误信息描述"
                }
            ]
        }
        
## (POST|DELETE|PATCH)用户与频道关联关系(订阅/关注) [/users/{userId}/relationships/channels]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + [OPTIONAL] 不写meta的话:/users/{userId}/relationships/channels/watch
    + 当type=users, 要先update[user_profile]表的用户是否开通频道字段（条件中需要带上该字段=false）
        + 如果返回1, 说明是首次开通频道, 需要创建一个用户频道;
        + 如果返回0, 说明已开通, 直接根据userId查找用户频道;
        + 如果没有找到用户频道, 抛出异常, 稍后在请求, 原因可能是并发请求时, 创建用户频道的事务还没有提交;
        + 如果找到用户频道, 就建立关联关系;
    + NOTE: 这是典型的SELECT FOR UPDATE应用场景, 转换为效率更高的先UPDATE, 再执行其他逻辑;
    + NOTE: 这是分布式事务, 先更新用户限界上下文, 再处理文章限界上下文, 先UPDATE可以确保用户频道的唯一性;
    + 对于频道来说是【订阅/取消】, 对于用户频道来说是【关注/取消关注】;
    + 资源与资源的多对多关联中, 可以存在多个多对多关联, 通过meta元数据区分他们;
    + meta中的relationships表明的是在用户(users)与频道(channels)的多对多关联中，这是一个watch多对多关联;
    
+ Meta
    + relationships (string) - watch:表明这是用户与频道资源多对多关系中的_订阅关联_
    
+ Data
    + id (long) - 被关联的资源ID. type=channels时, id为频道标识符; type=users时, id为用户标识符; 后端会为该用户创建频道再进行关联; 
    + type (string, nullable) - 缺省值:channels, 可选值:[users|channels]资源类型, 关注用户或订阅频道;
    + attributes (object, nullable) - 资源属性
    + attributes.displayOrder (int, nullable) - 缺省值:0, 排序字段;

+ Parameters
    + userId (long) - 用户ID

### 增加关联 [POST]

+ Request (application/json)

        {
            "meta": {
                "relationships": "watch"
            },
            "data": [
                {
                    "type": "users",
                    "id": "20",
                    "attributes": {
                        "displayOrder": 81
                    }
                },
                {
                    "type": "channels",
                    "id": "4",
                    "attributes": {
                        "displayOrder": 89
                    }
                }
            ]
        }

+ Response 200 (application/json)

+ Response 202 (application/json)

+ Response 204 (application/json)

+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "应用程序code编码",
                    "title": "Bad Request",
                    "detail": "错误信息描述"
                }
            ]
        }

### 删除关联 [DELETE]

+ Request (application/json)

        {
            "meta": {
                "relationships": "watch"
            },
            "data": [
                {
                    "id": "2"
                },
                {
                    "id": "3"
                }
            ]
        }

+ Response 200 (application/json)

+ Response 202 (application/json)

+ Response 204 (application/json)

+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "应用程序code编码",
                    "title": "Bad Request",
                    "detail": "错误信息描述"
                }
            ]
        }
        
### 修改关联 [PATCH]

如果data数组为空则删除所有关联关系

+ Request (application/json)

        {
            "meta": {
                "relationships": "watch"
            },
            "data": [
                {
                    "type": "users",
                    "id": "20",
                    "attributes": {
                        "displayOrder": 81
                    }
                },
                {
                    "type": "channels",
                    "id": "4",
                    "attributes": {
                        "displayOrder": 89
                    }
                }
            ]
        }
        
+ Response 200 (application/json)

+ Response 202 (application/json)

+ Response 204 (application/json)

+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "应用程序code编码",
                    "title": "Bad Request",
                    "detail": "错误信息描述"
                }
            ]
        }
        
## (GET)频道集合 [/channels]

+ Description
    + 删除tag字段, 删除user_id字段;
    + 增加cancellable字段, 增加channelImage字段;
    + 不加过滤条件查询所有频道集合
    + <del>filter[channelType]=1,2 : 只查询系统频道与订阅频道, 不查询用户频道(查询所有频道的时候用此过滤参数)</del>
    + <del>filter[UsersChannelsWatch.userId]={userId}&fields[UsersChannelsWatch]=displayOrder&fields[channels]=id,channelName,channelType,channelImage,description,watched,cancellable&include=UsersChannelsWatch&sort=UsersChannelsWatch.displayOrder</del>
    + filter[UsersChannelsWatch.userId]={userId}&include=UsersChannelsWatch&sort=UsersChannelsWatch.displayOrder
        + 查询用户订阅的频道, 并按displayOrder字段排序;
        + 只要参数中有该过滤条件, 如果用户没有登录, 抛出未认证异常;
        + 只要参数中有该过滤条件, 如果不是当前登录用户ID, 抛出未认证异常;
        + 只要参数中有该过滤条件, 后端需要进行联表查询;
        + 超级管理员ROLE_ADMIN, 可以查询任意过滤参数;
    + 后端强制添加enabled=1过滤条件
    + 排序:sort=users\_channels\_watch.displayOrder, 按用户设置的顺序排序(需要配合用户过滤条件使用);
    + 排序:sort=-watched, 按关注热度降序排序;
    + filter[targetId]=1,2,3,4,5,10&filter[channelType]=4
        + 查看当前用户在多个来源频道中是否已经订阅
        + targetId为seedId, 即网站种子来源的ID
        + channelType为在来源频道
    + filter[channelType]=4, 查看网站/来源频道

+ Data
    + channelType (int) - 频道类型, 1:版面频道, 2:标签频道, 3:用户频道, 4:来源频道
    + channelTypeValue (string) - 与channelType对应的枚举值
    + channelName (string) - 频道名称, 用户频道的话就是用户昵称, 订阅频道就是标签名, 系统频道就是固定的那几个类别名称;
    + channelImage (string, nullable) - 频道图片
    + description (string, nullable) - 前端用不到;
    + watched (int) - 订阅数/关注数
    + cancellable (int) - 0否, 1是 : 是否可取消订阅/关注;
    + subscribed (boolean, nullable) - false否, true是 : 用户是否订阅/关注了该频道, 只有在用户登录时增加该字段; 
    + displayOrder (int) - 排序, 用户登录时增加该显示字段
    + host (string) - 只有channelType=4的数据有该字段, 表示来源网站的host

### 查询频道集合 [GET]

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
                "sort": [
                    {
                        "direction": "ASC",
                        "property": "users_channels_watch.display_order",
                        "ignoreCase": false,
                        "nullHandling": "NATIVE",
                        "ascending": true,
                        "descending": false
                    }
                ]
            },
            "links": {
                "self": "/channels?filter[UsersChannelsWatch.userId]=11&fields[UsersChannelsWatch]=displayOrder&fields[channels]=id,channelName,channelType,channelImage,description,watched,cancellable&include=UsersChannelsWatch&sort=UsersChannelsWatch.displayOrder&page[number]=1&page[size]=10",
                "first": "/channels?filter[UsersChannelsWatch.userId]=11&fields[UsersChannelsWatch]=displayOrder&fields[channels]=id,channelName,channelType,channelImage,description,watched,cancellable&include=UsersChannelsWatch&sort=UsersChannelsWatch.displayOrder&page[number]=1&page[size]=10",
                "last": "/channels?filter[UsersChannelsWatch.userId]=11&fields[UsersChannelsWatch]=displayOrder&fields[channels]=id,channelName,channelType,channelImage,description,watched,cancellable&include=UsersChannelsWatch&sort=UsersChannelsWatch.displayOrder&page[number]=1&page[size]=10"
            },
            "data": [
                {
                    "id": 2,
                    "channelType": 1,
                    "channelTypeValue": "版面频道",
                    "channelName": "产品",
                    "channelImage": "http://i1.go2yd.com/image.php?url=http://s.go2yd.com/b/idgxqp5n_df00d1d1.jpg&amp;type=thumbnail_300x300",
                    "description": "频道描述",
                    "watched": 7,
                    "cancellable": 0,
                    "displayOrder": 272,
                    "subscribed": true,
                    "host": null
                }
            ]
        }
        
## (GET)频道详细信息 [/channels/{channelId}]

+ Description 
    + 获得用户频道 /channels/users/{userId}
        + 返回结构同直接获取频道API
    
+ Parameters
    + channelId (long) - 频道ID

### 查询频道详细 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 2,
                "channelType": 1,
                "channelTypeValue": "版面频道",
                "channelName": "产品",
                "channelImage": "http://i1.go2yd.com/image.php?url=http://s.go2yd.com/b/idgxqp5n_df00d1d1.jpg&amp;type=thumbnail_300x300",
                "description": "频道描述",
                "watched": 7,
                "cancellable": 0,
                "displayOrder": 272,
                "subscribed": true
            }
        }

        
## (GET)链接集合 [/links?filter[type]={typeId}]

+ Description
    + 各种链接
    + typeId 1,2,3不变, 还是之前的类型, 增加了友情链接与广告类型

+ Data
    + image (string) - image url
    + description (string) - 描述
    + href (string) - 链接
    + blank (int) - 1:新窗口, 0:当前窗口
    
+ Parameters
    + typeId (int) - 1:PC导航?, 2:手机?, 3:手机导航?, 4:友情链接, 5:广告

### 查询评论集合 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "number": 1,
                "size": 2,
                "numberOfElements": 2,
                "last": false,
                "totalPages": 2,
                "sort": null,
                "first": true,
                "totalElements": 4
            },
            "links": {
                "self": "/api/links?page[number]=1&page[size]=2",
                "first": "/api/links?page[number]=1&page[size]=2",
                "next": "/api/links?page[number]=2&page[size]=2",
                "last": "/api/links?page[number]=2&page[size]=2"
            },
            "data": [
                {
                    "image": "http://static.budee.com/yyren/image/banner/banner-01.jpg",
                    "creator": 1,
                    "created": 1480909654000,
                    "modifier": 1,
                    "description": "desc",
                    "sort": 1,
                    "title": "概述",
                    "type": 1,
                    "blank": 1,
                    "enabled": 1,
                    "parentId": 0,
                    "modified": 1480909654000,
                    "href": "/",
                    "id": 1
                },
                {
                    "image": "http://static.budee.com/yyren/image/banner/banner-02.jpg",
                    "creator": 1,
                    "created": 1480909654000,
                    "modifier": 1,
                    "description": "desc",
                    "sort": 2,
                    "title": "品牌",
                    "type": 1,
                    "blank": 0,
                    "enabled": 1,
                    "parentId": 0,
                    "modified": 1480909654000,
                    "href": "/filter-brand",
                    "id": 2
                }
            ]
        }
        
## (GET)产品主题颜色集合 [/topics/colors]

+ Description
    + 

+ Data

### 查询颜色集合 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "number": 1,
                "size": 0,
                "numberOfElements": 8,
                "last": true,
                "totalPages": 1,
                "sort": null,
                "first": true,
                "totalElements": 8
            },
            "data": [
                "000000",
                "0000FF",
                "00FF00",
                "00FFFF",
                "FF0000",
                "FF00FF",
                "FFFF00",
                "FFFFFF"
            ]
        }

## (GET)主题 [/topics/{id}]

+ Description
    + topic类型 : [产品|新闻|评测|原创]
    + post 类型 : [原创|爬取|精翻|机翻]
    + forumId = [1], 有product数据;
    + forumId = [3|4], 有document数据;

+ Data
    + forumId (long) - 版块ID, 1:产品, 2:品牌, 3:新闻, 4:评测, 5:视频. @see forumName
    + forumName (string) - 版块名称, @see forumId
    + topicType (int) - 主题类型, 0:正常, 1:置顶, 2:公告. @see topicTypeValue
    + topicTypeValue (String) - 主题类型值, @see topicType
    + reviews (int) - 检阅数/浏览数
    + replies (int) - 评论数
    + thumbsUp (int) - 喜欢数
    + thumbsDown (int) - 不喜欢数
    + thumbs (int) - thumbsUp 减去 thumbsDown
    + locked (int) - _保留字段_, 是否锁定, 0:否,1:是, 被锁定主题不能被评论
    + moderated (int) - _保留字段_, 是否审核此主题, 0:否, 1:是
    + imageSingle (boolean) - 是否是单图
    + imageRotation (boolean) - 是否有旋转图
    + liked (int) - -1:踩, 0:无, 1:赞
    + favorite (int) - 0:否, 1:已加入收藏
    + rotations (array) - 旋转图集合
    + images (array) - 图片集合
    + audios (array) - 音频集合
    + videos (array) - 视频集合
    + from (object, nullable) - 来源, 只有爬取的数据才有该数据
    + from.seedId (int) - 种子ID
    + from.origin (int) - 数据来源URL
    + from.rating (double, nullable) - 爬取的评分 区间[0, 1]
    + from.reviews (int, nullable) - 爬取的浏览数 区间[0, 1]
    + from.host (string) - 网站地址
    + from.source (string) - 来源名称
    + from.image (string) - 来源名称
    + from.type (string) - users|channels, 类型
    + from.targetId (string) - 目标ID, 类型为users时为用户ID, 类型为channels时为频道ID
    + document (object, nullable) - 新闻和评测相关数据; 
    + document.postDate (date, nullable) - 文章发表时间
    + document.author (string, nullable) - 文章作者
    + document.brands (array) - 文章关联的品牌
    + product (object, nullable) - 产品相关数据;
    + product.price (decimal, nullable) - [MUST]Authenticated, 价格
    + product.priceUnit (string, nullable) - [MUST]Authenticated, 价格单位编码 e.g : USD
    + product.priceSign (string, nullable) - [MUST]Authenticated, 价格单位符号 e.g : $
    + product.brand (string, nullable) - 品牌名称
    + product.brandInfo (object, nullable) - 品牌详细
    + product.brandInfo.name (string) - 品牌名称
    + product.brandInfo.logo (string) - 品牌LOGO
    + product.brandInfo.rating (int) - 品牌评分 区间[0, 1]
    + product.brandInfo.reviews (int) - 品牌浏览数 区间[0, 1]
    + product.brandInfo.top (boolean) - true: 品牌有详情, false: 无详情
    + product.histories (array) - [MUST]Authenticated, 品牌历史价格
    + product.histories.price (decimal) - 价格
    + product.histories.effectiveDate (date) - 生效时间
    + post (object) - 内容文本数据; 标题, 摘要, 参数, 内容等.
        + 优先级:精翻|原创 > 机翻 > 爬取
    + post.postType (int) - 1:爬取, 2:机翻, 3:精翻, 4:原创
    + post.postTypeValue (string) - 类型:精翻, 原创, 机翻, 爬取
    + post.priority (int) - 机器翻译默认为50, 按翻译质量降序排序
    + post.title (string) - 主题标题
    + post.description (string, nullable) - 摘要/描述
    + post.content (string， nullable) - 内容富文本
    + post.categories (array) - 分类, 有可能是机器学习出来的
    + post.tags (array) - 标签/关键词/等等
    + post.features (array) - 特性集合
    + post.parent (object) - post的parent节点
        + 如果post是[机翻|精翻], 那么post.parent指向其原文节点
    + similarities (array) - 排重后的相同主题数据;
        + 如果该topic是clustering数据, 会返回相同数据来源不同的topics

+ Parameters
    + id (long) - topic id

### 查询主题 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 49226,
                "creator": 0,
                "modifier": 0,
                "created": "2016-10-18 07:10:16",
                "modified": "2017-04-05 13:55:33",
                "forumId": 1,
                "topicType": 0,
                "reviews": 0,
                "replies": 0,
                "thumbsUp": 0,
                "thumbsDown": 0,
                "thumbs": 0,
                "locked": 0,
                "moderated": 0,
                "forumName": "产品",
                "topicTypeValue": "正常",
                "imageSingle": false,
                "imageRotation": true,
                "liked": 1,
                "favorite": 1,
                "rotations": [
                    {
                        "mime": "image/jpeg",
                        "filename": "http://static.budee.com/yyren/image/79/18/1199986.jpg"
                    },
                    {
                        "mime": "image/jpeg",
                        "filename": "http://static.budee.com/yyren/image/79/18/1199987.jpg"
                    }
                ],
                "images": [
                    {
                        "mime": "image/jpeg",
                        "filename": "http://static.budee.com/yyren/image/176/4/307350.jpg"
                    },
                    {
                        "mime": "image/jpeg",
                        "filename": "http://static.budee.com/yyren/image/176/4/307351.jpg"
                    }
                ],
                "audios": [],
                "videos": [],
                "from": {
                    "id": 49226,
                    "seedId": 11,
                    "origin": "https://www.thomann.de/gb/rode_nt2a_studio_solution_set.htm",
                    "rating": 0.94,
                    "reviews": 0,
                    "host": "https://www.thomann.de/gb/cat_brands.html",
                    "source": "Thomann"
                },
                "document": {
                    "postDate": "2017-07-07",
                    "author": "作者",
                    "brands": [
                        "2Box",
                        "EV",
                        "Yamaha"
                    ]
                },
                "product": {
                    "id": 49226,
                    "topicId": 49226,
                    "price": 329,
                    "priceUnit": "USD",
                    "priceSign": "$"
                    "brand": "Rode",
                    "histories": [
                        {
                            "price": 329,
                            "effectiveDate": "2017-03-23"
                        },
                        {
                            "price": 299,
                            "effectiveDate": "2016-12-22"
                        },
                        {
                            "price": 299,
                            "effectiveDate": "2016-11-04"
                        },
                        {
                            "price": 339,
                            "effectiveDate": "2016-10-19"
                        },
                        {
                            "price": 339,
                            "effectiveDate": "2016-10-18"
                        }
                    ]
                },
                "post": {
                    "id": 49226,
                    "parent": null,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2016-10-18 07:10:16",
                    "modified": "2017-04-05 13:55:33",
                    "parentId": 0,
                    "postTypeValue": "爬取",
                    "topicId": 49226,
                    "priority": 0,
                    "title": "NT2-A Studio Solution Set",
                    "description": "",
                    "content": "",
                    "categories": [
                        "Microphones",
                        "Large-diaphragm Mics"
                    ],
                    "tags": [
                        "256707"
                    ],
                    "features": [
                        {
                            "_name": "Media",
                            "_value": "<div></div>"
                        },
                        {
                            "_name": "Category",
                            "_value": "<a href=\"https://www.thomann.de/gb/large_diaphragm_mics.html\">Large-diaphragm Microphones</a>"
                        }
                    ]
                }
            }
        }
        
## (GET)搜索主题集合 [/topics/search]

+ Description
    + topic类型 : [产品|新闻|评测|原创]
    + post 类型 : [原创|爬取|精翻|机翻]
    + forumId = [1], 有product数据;
    + forumId = [3|4], 有document数据;
    + 结构同/topics, 是其子集（字段更少一些）
    
+ Meta
    + aggregations (object, nullable) - 原聚合数据, 结构不变, 所有一级对象为每个聚类的名称;
    + clusters (array, nullable) - 【保留】只有在聚类查询时才返回数据或空数组, 原聚类数据, 结构不变;

+ Data
    + meta.highlight (object, nullable) - 只有在请求中有filter[q]参数时才会有. 高亮对象, 结构较之前发生变化
    + meta.highlight.content (string) - 根据原数组推荐的一条高亮内容
    
+ 过滤参数
    + filter[q] (string, nullable) - 按关键词过滤
    + filter[category] (string, nullable) - 按类别过滤
    + filter[categoryId] (int, nullable) - 按类ID别过滤
    + filter[brand] (string, nullable) - 按品牌过滤, e.g : EV,Yamaha 
    + filter[tag] (string, nullable) - 按标签过滤
    + filter[from] (int, nullable) - 按来源过滤, 抓取的数据都有一个来源seedId
    + filter[forum] (int, nullable) - 按版块过滤;
    + filter[topicType] (int, nullable) - 主题类型过滤
    + filter[postType] (int, nullable) - 内容类型过滤
    + filter[items.price:range] - 按价格范围过滤, e.g=[20,30]; =[20,\*]; 两个数值不能同时为\*, 不同于数据库过滤, 搜索API由于部分实现问题, 必须使用闭区间过滤, 内部实现为(20,30] 
    + filter[created:gt] - 查找大于指定时间的新数据, 时间格式yyyy-MM-dd HH:mm:ss
    + filter[channel] (int, nullable) - 按频道过滤, 过滤参数可以是 channel 或 channelId

+ 排序参数
    + sort=-images.distance.FFFFFF - 按颜色降序排序, [000000|0000FF|00FF00|00FFFF|FF0000|FF00FF|FFFF00|FFFFFF]
        + alias : 暂无待定
    + sort=-items.price - 按价格降序排序
        + alias : 暂无待定
    + sort=-created - 按主题创建时间降序排序
    + sort=-modified - 按主题修改时间降序排序
    + 特殊排序
        + 可用过滤参数
            + filter[forumId] 
            + filter[topicId:ne] - not equel (单个值)
            + filter[topicId:nin] - not in (多个值)
        + 排序类型
            + sort=-hot - 按热度降序排序
            + sort=-new - 按新品降序排序
            + sort=-best - 按精选降序排序
+ 建议器
    + suggest[size] (int) - 控制返回的建议结果条数, 缺省值为0, 即不显示建议器
    + suggest[type] (string) - 建议类型, 缺省值 phrase
        + phrase - 拼写检查 (目前对中文支持不太友好)
        + completion - 自动完成 (未完成)

+ 聚合参数
    + agg[mask] (int, nullable) - 缺省为15, 即显示所有聚合. (filter中明确指定的字段, 不会作为聚合项返回 e.g filter[brand]=Yamaha, 结果将不包含brand聚合)
        + 0001 - (1) category 聚合
        + 0010 - (2) brand 聚合
        + 0100 - (4) price 聚合
        + 1000 - (8) from 聚合
    + agg[size] (int, nullable) - 该参数控制返回聚合数的大小, 不写该参数返回所有聚合数据. 
        + 增加了对小于0的参数判断, 小于0时不返回聚合数据
    + agg[sort] (string, nullable) - 聚合排序, 只能在[term|count]中选择, term按字母排序, count按统计数排序
    + agg[image] (boolean, nullable) - 聚合的数据是否需要查找一个代表性图片. 缺省false.
    + agg[group] (boolean, nullable) - 是否按首字母分组显示. 
    + aggregation[{name}] (string, nullable) - 如果有该请求参数, 其他聚合参数才有效, {name}为聚合的名称, 其值为聚合的字段 e.g aggregation[category]=categories.en.raw
    
+ 给定某个品牌, 按所有类别聚合
    + /topics/search?page[size]=0&filter[forumId]=1&filter[brand.name]=A-Gift-Republic&agg[group]=false&agg[image]=true&aggregation[category]=categories.en.raw
    + categories.en.raw | categories.cn.raw
    
+ 给定某个品牌, 按一级类别聚合
    + /topics/search?page[size]=0&filter[forumId]=1&filter[brand.name]=A-Gift-Republic&agg[group]=false&agg[image]=true&aggregation[category]=level_0.en.raw
    + level\_0.en.raw | level\_0.cn.raw
    
+ 给定某个品牌与一级类别, 按二级类别聚合
    + /topics/search?page[size]=0&filter[forumId]=1&filter[brand.name]=A-Gift-Republic&filter[level\_0.en.raw]=Accessories&agg[group]=false&agg[image]=true&aggregation[category]=level\_0.en.raw-level\_1.en.raw

+ 按一级类别聚合
    + /topics/search?page[size]=0&filter[forumId]=1&agg[group]=false&agg[image]=true&aggregation[category]=level_0.en.raw

+ 给定一级类别, 按二级类别聚合
    + /topics/search?page[size]=0&filter[forumId]=1&filter[level\_0.en.raw]=Guitars%20and%20Basses&agg[group]=false&agg[image]=true&aggregation[category]=level_1.en.raw

+ 给定二级类别, 按三级类别聚合
    + /topics/search?page[size]=0&filter[forumId]=1&filter[level\_1.en.raw]=Accessories&agg[group]=false&agg[image]=true&aggregation[category]=level_2.en.raw

+ 按品牌聚合, 显示所有品牌集合, 按count降序
    + /topics/search?page[size]=0&filter[forumId]=1&agg[group]=true&aggregation[brand]=brand.name
    + 增加 agg[size]=10 参数控制聚合结果大小
    
+ 给定分类，查找流行度排行前{n}的品牌 (该API暂时数据有问题, 待修复)
    + http://localhost/topics/search?page[size]=0&filter[forumId]=1&filter[category]=Guitars and Basses&aggregation[brand]=brand.name

### 搜索主题 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "number": 1,
                "size": 1,
                "numberOfElements": 1,
                "last": false,
                "totalPages": 88800,
                "sort": null,
                "first": true,
                "totalElements": 88800,
                "suggest": {
                    "phrase": [
                        {
                            "offset": 0,
                            "length": 13,
                            "options": [
                                {
                                    "score": 0.0000012245733,
                                    "highlighted": "<em>guitar</em> and bas",
                                    "text": "guitar and bas"
                                },
                                {
                                    "score": 1.1775544e-7,
                                    "highlighted": "<em>guild</em> and bas",
                                    "text": "guild and bas"
                                },
                                {
                                    "score": 7.7354095e-8,
                                    "highlighted": "<em>guiro</em> and bas",
                                    "text": "guiro and bas"
                                },
                                {
                                    "score": 4.4551182e-8,
                                    "highlighted": "<em>guira</em> and bas",
                                    "text": "guira and bas"
                                },
                                {
                                    "score": 3.3881292e-8,
                                    "highlighted": "<em>guitari</em> and bas",
                                    "text": "guitari and bas"
                                },
                                {
                                    "score": 3.2059063e-8,
                                    "highlighted": "<em>guit</em> and bas",
                                    "text": "guit and bas"
                                }
                            ],
                            "text": "guita and bas"
                        }
                    ]
                },
                "aggregations": {
                    "price": {
                        "buckets": [
                            {
                                "doc_count": 40744,
                                "from": "-Infinity",
                                "to": 100,
                                "key": "*-100.0"
                            },
                            {
                                "doc_count": 13366,
                                "from": 100,
                                "to": 200,
                                "key": "100.0-200.0"
                            }
                        ]
                    },
                    "from": {
                        "buckets": [
                            {
                                "doc_count": 26979,
                                "from": {
                                    "host": "http://www.sweetwater.com",
                                    "source": "Sweetwater"
                                },
                                "key": "2"
                            },
                            {
                                "doc_count": 61821,
                                "from": {
                                    "host": "https://www.thomann.de",
                                    "source": "Thomann"
                                },
                                "key": "11"
                            }
                        ]
                    },
                    "category": {
                        "buckets": [
                            {
                                "image": "http://static.budee.com/yyren/image/203/52038.jpg",
                                "doc_count": 53773,
                                "key": "吉他/贝斯"
                            }
                        ]
                    },
                    "brand": {
                        "buckets": {
                            "#": [
                                {
                                    "doc_count": 1,
                                    "reviews": 0,
                                    "top": true,
                                    "rating": 0,
                                    "logo": "http://static.budee.com/yyren/image/188/21/1424546.jpg",
                                    "key": "1stClassRock"
                                }
                            ],
                            "A": [
                                {
                                    "doc_count": 100,
                                    "reviews": 320,
                                    "top": true,
                                    "rating": 0.9,
                                    "logo": "http://static.budee.com/yyren/image/17.jpg",
                                    "key": "A-Gift-Republic"
                                },
                                {
                                    "doc_count": 2,
                                    "reviews": 0,
                                    "top": true,
                                    "rating": 0,
                                    "logo": "http://static.budee.com/yyren/image/188/21/1424547.jpg",
                                    "key": "AA Craaft"
                                }
                            ]
                        }
                    }
                },
                "clusters": [
                    {
                        "score": 37.7698711496348,
                        "subgroups": [],
                        "otherTopics": false,
                        "documentReferences": [
                            "97189",
                            "105539",
                            "105455",
                            "105516",
                            "56437"
                        ],
                        "id": 0,
                        "label": "String Set for E- Span Class",
                        "phrases": [
                            "String Set for E- Span Class"
                        ]
                    },
                    {
                        "score": 29.360514351279697,
                        "subgroups": [],
                        "otherTopics": false,
                        "documentReferences": [
                            "41945",
                            "100401",
                            "43061",
                            "110672",
                            "76835",
                            "41897"
                        ],
                        "id": 1,
                        "label": "Acoustic Span Class",
                        "phrases": [
                            "Acoustic Span Class"
                        ]
                    }
                ]
            },
            "links": {
                "self": "/topics/search?page[number]=1&page[size]=1",
                "first": "/topics/search?page[number]=1&page[size]=1",
                "next": "/topics/search?page[number]=2&page[size]=1",
                "last": "/topics/search?page[number]=88800&page[size]=1"
            },
            "data": [
                {
                    "id": 31839,
                    "meta": {
                        "score": 1.282559,
                        "highlight": {
                            "content": "Ueberschall Urbanic <span class='highlight'>Guitars</span> (ESD); Elastik player instrument; <span class='highlight'>guitar</span> library for Urban, RnB und Hip Hop music consists of 125 themes with 780 loops and samples between 60 and 130 BPM; an acoustical and two different electrical <span class='highlight'>guitars</span> play various melodies, riffs and licks; Ueberschall Elastik Player with loopeye functions offers timestretch and pitchshift in best quality; browser features multiple filter search and tagging; prelisten in sync; sequence mode with loads of edtiting features per slice; realtime sync to host; multiple outs; supported formats: standalone/VST2/AU/AAXnative/RTAS; system requirements: Win7 (64-Bit) min., Mac OSX 10.9 min., 1.8 GB HD, internet connection"
                        }
                    },
                    "forumId": 1,
                    "forumName": "产品",
                    "topicType": 0,
                    "topicTypeValue": "正常",
                    "imageSingle": false,
                    "imageRotation": true,
                    "creator": 0,
                    "created": "2016-10-17 21:08:35",
                    "modified": "2017-04-05 12:33:54",
                    "liked": 1,
                    "favorite": 1,
                    "reviews": 22,
                    "replies": 21,
                    "thumbsUp": 44,
                    "thumbsDown": 56,
                    "thumbs": -12,
                    "brand": {
                        "reviews": 0,
                        "name": "Tractel",
                        "rating": 0,
                        "logo": null
                    },
                    "rotations": [
                        {
                            "filename": "http://static.budee.com/yyren/image/88/19/1267942.jpg"
                        },
                        {
                            "filename": "http://static.budee.com/yyren/image/88/19/1267943.jpg"
                        }
                    ],
                    "images": [
                        {
                            "filename": "http://static.budee.com/yyren/image/15/3/200569.jpg"
                        },
                        {
                            "filename": "http://static.budee.com/yyren/image/15/3/200570.jpg"
                        }
                    ],
                    "from": {
                        "seedId": 11,
                        "origin": "https://www.thomann.de/gb/jbl_c26c.htm",
                        "rating": 0.95,
                        "host": "https://www.thomann.de",
                        "source": "Thomann",
                        "image": "logo"
                    },
                    "post": {
                        "postType": 1,
                        "postTypeValue": "爬取",
                        "title": "C 26 C",
                        "description": "",
                        "categories": [
                            "PA Equipment",
                            "Loudspeakers",
                            "Speakers for Installation",
                            "Ceiling Speakers"
                        ],
                        "tags": [
                            "131786"
                        ]
                    },
                    "product": {
                        "price": 358,
                        "priceUnit": "EUR"
                    },
                    "similarities": []
                }
            ]
        }
        
## (GET) 查询与特定主题相关的数据集合 [/topics/{id}/relates?filter[forumId]={forumId}&filter[tag]={tag}]

+ Description
    + 结构同搜索API

+ Parameters
    + id (long) - 主题ID
    + forumId (long, nullable) - forum ID, 多个id使用逗号分隔
    + tag (string, nullable) - 按标签/型号/关键词过滤, 多个tag使用逗号分隔
    
### 搜索主题 [GET]

+ Response 200 (application/json)

        {
        }
        
## (GET) 产品比较集合 [/topics/compare?art[]={ids}]

+ Description
    + 
    
+ Meta 
    + features (array) - 原产品参数比较数组, 结构不变
    + prices (array) - 原产品价格比较数组, 结构不变
    
+ Data
    + 结构同主题详情

+ Parameters
    + ids (long) - 主题ID e.g: 123,125,126
    
### 产品比较集合 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "totalPages": 1,
                "totalElements": 2,
                "size": 2,
                "number": 1,
                "numberOfElements": 2,
                "first": true,
                "last": true,
                "sort": null,
                "features": [
                    {
                        "columns": [
                            "Analog Inputs",
                            "2 x TS",
                            "-"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Analog Outputs",
                            "6 x TS, 1 x TRS (Headphones)",
                            "-"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Arpeggiator",
                            "Yes",
                            "Yes"
                        ],
                        "collapsible": true
                    },
                    {
                        "columns": [
                            "Audio Inputs",
                            "-",
                            "2 x TS"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Audio Outputs",
                            "-",
                            "6 x TS, 1 x TRS (Headphones)"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Depth",
                            "7.4\"",
                            "14.6\""
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Digital Inputs",
                            "1 x S/PDIF",
                            "1 x S/PDIF"
                        ],
                        "collapsible": true
                    },
                    {
                        "columns": [
                            "Digital Outputs",
                            "1 x S/PDIF",
                            "1 x S/PDIF"
                        ],
                        "collapsible": true
                    },
                    {
                        "columns": [
                            "Effects Types",
                            "Reverb, Chorus, Delay, Phaser, EQ, Ring Mod",
                            "Reverb, Chorus, Delay, Phaser, EQ, Ring Mod"
                        ],
                        "collapsible": true
                    },
                    {
                        "columns": [
                            "Height",
                            "3.2\"",
                            "4.6\""
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "MIDI I/O",
                            "In/Out/Thru/USB",
                            "In/Out/Thru/USB"
                        ],
                        "collapsible": true
                    },
                    {
                        "columns": [
                            "Manufacturer Part Number",
                            "Virus TI2 Desk",
                            "Virus TI2 Key"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Number of Effects",
                            "192",
                            "192"
                        ],
                        "collapsible": true
                    },
                    {
                        "columns": [
                            "Number of Keys",
                            "-",
                            "61"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Number of Presets",
                            "512 RAM Patches, 3328 ROM Sounds",
                            "512 RAM Patches, 3328 ROM Sounds"
                        ],
                        "collapsible": true
                    },
                    {
                        "columns": [
                            "Other Controllers",
                            "-",
                            "Pitchbend, Mod Wheel"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Pedal Inputs",
                            "-",
                            "1 x Hold, 1 x Control"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Polyphony",
                            "20 Notes-90 Notes (Depending On the Patch)",
                            "20 Notes-90 Notes (Depending On the Patch)"
                        ],
                        "collapsible": true
                    },
                    {
                        "columns": [
                            "Power Supply",
                            "Power Supply Included",
                            "Included"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Sound Engine Type(s)",
                            "Analog Modeling",
                            "Analog Modeling"
                        ],
                        "collapsible": true
                    },
                    {
                        "columns": [
                            "Type of Keys",
                            "-",
                            "Semi-weighted"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "USB",
                            "1 x Type B",
                            "1 x Type B"
                        ],
                        "collapsible": true
                    },
                    {
                        "columns": [
                            "Weight",
                            "7.4 lbs.",
                            "30.6 lbs."
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Width",
                            "18.5\"",
                            "39.2\""
                        ],
                        "collapsible": false
                    }
                ],
                "prices": [
                    {
                        "columns": [
                            "Sweetwater",
                            "USD 2185.00",
                            "USD 2915.00"
                        ],
                        "collapsible": false
                    }
                ]
            },
            "data": [
                {
                    "forumId": 1,
                    "forumName": "产品",
                    "topicType": 0,
                    "topicTypeValue": "正常",
                    "imageSingle": true,
                    "imageRotation": false,
                    "creator": 0,
                    "created": "2016-10-12 09:55:37",
                    "modified": "2017-04-05 22:46:40",
                    "liked": 1,
                    "favorite": 1,
                    "reviews": 22,
                    "replies": 21,
                    "thumbsUp": 44,
                    "thumbsDown": 56,
                    "thumbs": -12,
                    "brand": null,
                    "rotations": [],
                    "images": [
                        {
                            "filename": "http://static.budee.com/yyren/image/111/24/1601492.jpg"
                        }
                    ],
                    "from": {
                        "seedId": 2,
                        "origin": "http://www.sweetwater.com/store/detail/VirusTI2Desk",
                        "rating": 0.9,
                        "host": "http://www.sweetwater.com",
                        "source": "Sweetwater"
                    },
                    "post": {
                        "postType": 1,
                        "postTypeValue": "爬取",
                        "title": "Virus TI2 Desktop",
                        "description": "Analog Modeling Desktop Synthesizer and 24-bit/192kHz Audio/MIDI Interface",
                        "categories": [
                            "Keyboards & MIDI",
                            "Synths / Modules"
                        ],
                        "tags": [
                            "VirusTI2Desk"
                        ]
                    },
                    "similarities": [],
                    "id": 1
                },
                {
                    "forumId": 1,
                    "forumName": "产品",
                    "topicType": 0,
                    "topicTypeValue": "正常",
                    "imageSingle": true,
                    "imageRotation": false,
                    "creator": 0,
                    "created": "2016-10-12 09:55:38",
                    "modified": "2017-04-05 22:46:38",
                    "liked": 0,
                    "reviews": 22,
                    "replies": 21,
                    "thumbsUp": 44,
                    "thumbsDown": 56,
                    "thumbs": -12,
                    "brand": null,
                    "rotations": [],
                    "images": [
                        {
                            "filename": "http://static.budee.com/yyren/image/111/24/1601490.jpg"
                        }
                    ],
                    "from": {
                        "seedId": 2,
                        "origin": "http://www.sweetwater.com/store/detail/VirusTI2Key",
                        "rating": 1,
                        "host": "http://www.sweetwater.com",
                        "source": "Sweetwater"
                    },
                    "post": {
                        "postType": 1,
                        "postTypeValue": "爬取",
                        "title": "Virus TI2 Keyboard",
                        "description": "61-key Analog Modeling Synthesizer and 24-bit/192kHz Audio/MIDI Interface",
                        "categories": [
                            "Keyboards & MIDI",
                            "Synths / Modules"
                        ],
                        "tags": [
                            "VirusTI2Key"
                        ]
                    },
                    "similarities": [],
                    "id": 2
                }
            ]
        }
        
## (GET) 查询品牌详情 [/brands/{name}]

+ Description
    + 结构同主题详情API

+ Parameters
    + name (string) - 品牌名称, 区分大小写
    
### 搜索主题 [GET]

+ Response 200 (application/json)

        {
        }
        
## (GET) 查询浏览主题历史记录列表 [/topics/histories?page[size]=10&page[number]=1]

+ Description
    + 如果登陆使用登陆用户信息
    + 如果没有登录, 使用header
    + 结构同主题搜索API
    
+ Parameters
    + page[size] (int) - 默认10
    + page[number]=1 (int) - 默认1
    
### 查询浏览主题历史记录 [GET]

+ Response 200 (application/json)

        {
        }
        
## (GET) 查询主题搜索热词列表 [/topics/keywords?page[size]=10]

+ Description
    + 统计某一个时间段内（当前时间往前推 7 * 24 小时）的搜索指数高的词组或短语等

+ Data
    + rank (int) - 排名
    + keyword (string) - 热词/词组/短语等等
    + value (int) - 对关键词在过去一段时间内搜索频度的度量, 会随着时间慢慢降低;
    + score (double) - 归一化(normalization)处理后的值, 取值范围[0, 1], 会随着时间慢慢降低;
    
+ Parameters
    + page[size]=10 (int) - 默认10
    
### 查询主题搜索热词列表 [GET]

+ Response 200 (application/json)

        {
            "data": [
                {
                    "rank": 1,
                    "keyword": "guitars",
                    "value": 337,
                    "score": 1
                },
                {
                    "rank": 2,
                    "keyword": "Wagner",
                    "value": 322,
                    "score": 0.9549665333484131
                },
                {
                    "rank": 3,
                    "keyword": "Yamaha",
                    "value": 318,
                    "score": 0.9419774284314028
                },
                {
                    "rank": 4,
                    "keyword": "EV",
                    "value": 317,
                    "score": 0.9404728856718862
                },
                {
                    "rank": 5,
                    "keyword": "bass",
                    "value": 258,
                    "score": 0.7664617198397439
                }
            ]
        }
