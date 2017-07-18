FORMAT: 1A
HOST: http://192.168.1.138/

# topics

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
    + 不加过滤条件查询所有频道集合
    + filter[channelType]=1,2 : 只查询系统频道与订阅频道, 不查询用户频道(查询所有频道的时候用此过滤参数)
    + filter[users\_channels\_watch.user_id]={userId} : 查询用户订阅的频道
        + 只要参数中有该过滤条件, 如果用户没有登录, 抛出未认证异常;
        + 只要参数中有该过滤条件, 如果不是当前登录用户ID, 抛出未认证异常;
        + 只要参数中有该过滤条件, 后端需要进行联表查询;
        + 超级管理员ROLE_ADMIN, 可以查询任意过滤参数;
    + 后端强制添加enabled=1过滤条件
    + 排序:sort=users\_channels\_watch.displayOrder, 按用户设置的顺序排序(需要配合用户过滤条件使用);
    + 排序:sort=-watched, 按关注热度降序排序;

+ Data
    + channelType (int) - 频道类型, 1:系统频道, 2:订阅频道, 3:用户频道
    + channelName (string) - 频道名称, 用户频道的话就是用户昵称, 订阅频道就是标签名, 系统频道就是固定的那几个类别名称;
    + tag (string, nullable) - 前端用不到;
    + description (string, nullable) - 前端用不到;
    + watched (int) - 订阅数/关注数
    + subscribed (boolean, nullable) - 用户是否订阅/关注了该频道, 只有在用户登录时增加该字段;
    + displayOrder (int) - 排序, 用户登录时增加该显示字段

### 查询频道集合 [GET]

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
                "self": "/api/channels?page[number]=1&page[size]=2",
                "first": "/api/channels?page[number]=1&page[size]=2",
                "next": "/api/channels?page[number]=2&page[size]=2",
                "last": "/api/channels?page[number]=2&page[size]=2"
            },
            "data": [
                {
                    "id": "1",
                    "userId": "22",
                    "channelType": 2,
                    "channelName": "新闻",
                    "tag": "tag",
                    "description": "description",
                    "watched": 11889,
                    "subscribed": "true",
                    "displayOrder": 1
                }
            ]
        }
        
## (GET)链接集合 [/links?filter[type]={typeId}]

+ Description
    + 各种链接

+ Data
    + image (string) - image url
    + description (string) - 描述
    + href (string) - 链接
    + blank (int) - 1:新窗口, 0:当前窗口
    
+ Parameters
    + typeId (int) - 1:PC导航, 2:手机???, 3:手机导航 4:友情链接

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
    
+ Meta
    + aggregations (object) - 原聚合数据, 结构不变;
    + clusters (array) - 原聚类数据, 结构不变;

+ Data
    + meta.highlight (object, nullable) - 原高亮数据, 结构不变
    + forumId (long) - 版块ID, 1:产品, 2:品牌, 3:新闻, 4:评测. @see forumName
    + forumName (string) - 版块名称, @see forumId
    + topicType (int) - 主题类型, 0:正常, 1:置顶, 2:公告. @see topicTypeValue
    + topicTypeValue (String) - 主题类型值, @see topicType
    + reviews (int) - 检阅数/浏览数
    + replies (int) - 评论数
    + thumbsUp (int) - 喜欢数
    + thumbsDown (int) - 不喜欢数
    + thumbs (int) - thumbsUp 减去 thumbsDown
    + locked (int) - 是否锁定, 0:否,1:是, 被锁定主题不能被评论
    + moderated (int) - _保留字段_, 是否审核此主题, 0:否, 1:是
    + imageSingle (boolean) - 是否是单图
    + imageRotation (boolean) - 是否有旋转图
    + userLike (boolean) - 当前登录用户是否喜欢过
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
    + from.source (string) - 网站简称
    + document (object, nullable) - 新闻和评测相关数据; 
    + document.postDate (date, nullable) - 文章发表时间
    + document.author (string, nullable) - 文章作者
    + document.brands (array) - 文章关联的品牌
    + product (object, nullable) - 产品相关数据;
    + product.price (decimal, nullable) - [MUST]Authenticated, 价格
    + product.priceUnit (string, nullable) - [MUST]Authenticated, 价格单位
    + product.brand (string, nullable) - 品牌名称
    + product.brandInfo (object, nullable) - 品牌详细
    + product.brandInfo.name (string) - 品牌名称
    + product.brandInfo.logo (string) - 品牌LOGO
    + product.brandInfo.rating (int) - 品牌评分 区间[0, 1]
    + product.brandInfo.reviews (int) - 品牌浏览数 区间[0, 1]
    + product.histories (array) - [MUST]Authenticated, 品牌历史价格
    + product.histories.price (decimal) - 价格
    + product.histories.effectiveDate (date) - 生效时间
    + post (object) - 内容文本数据; 标题, 摘要, 参数, 内容等.
        + 优先级:精翻|原创 > 机翻 > 爬取
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
    + items (object, 此结构待定???) - 排重后的相同主题数据;
        + 如果该topic是clustering数据, 会返回相同数据来源不同的topics
        + 如果存在这个对象, 那么post对象里存放的是翻译合并的数

+ Parameters
    + id (long) - topic id

### 查询主题 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "aggregations": {
                    "image": "http://static.budee.com/yyren/image/202/51840.jpg",
                    "category": {
                        "buckets": [
                            {
                                "image": "http://static.budee.com/yyren/image/203/52038.jpg",
                                "doc_count": 53773,
                                "key": "吉他/贝斯"
                            }
                        ]
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
            "data": {
                "meta": {
                    "_score": 1,
                    "highlight": {
                        "items.contents": [
                            "RockcaseRC20809B柔光钢琴<spanclass='highlight'>吉他</span>"
                        ]
                    }
                },
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
                "userLike": true,
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
                    "priceUnit": "EUR",
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
