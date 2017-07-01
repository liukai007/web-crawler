FORMAT: 1A
HOST: http://www.mifan.com/

# topics

+ 2017年6月30日
    + API初始化

文章主题模块

## (POST|DELETE|PATCH)用户与主题关联关系(隐藏) [/users/{userId}/relationships/topics]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + 不写meta的话:/users/{userId}/relationships/topics/hide
    
+ Data
    + id (long) - 主题资源唯一标识符
    + remark (string, nullable) - 隐藏原因

+ Meta
    + relationships (string) - hide:表明这是用户与主题资源多对多关系中的_隐藏关联_

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
                    "id": "2",
                    "remark": "文章好烂!"
                },
                {
                    "id": "3",
                    "remark": "文章抄袭!"
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

+ Request (application/json)

        {
            "meta": {
                "relationships": "hide"
            },
            "data": [
                {
                    "id": "2",
                    "remark": "文章好烂!"
                },
                {
                    "id": "3",
                    "remark": "文章抄袭!"
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
    + 不写meta的话:/users/{userId}/relationships/topics/like

+ Data
    + id (long) - 主题资源唯一标识符
    + up (boolean) - true:喜欢, false:不喜欢

+ Meta
    + relationships (string) - hide:表明这是用户与主题资源多对多关系中的_喜欢关联_

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
                    "id": "2",
                    "up": true
                },
                {
                    "id": "3",
                    "up": false
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

+ Request (application/json)

        {
            "meta": {
                "relationships": "like"
            },
            "data": [
                {
                    "id": "2",
                    "up": true
                },
                {
                    "id": "3",
                    "up": false
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
    + 不写meta:/users/{userId}/relationships/channels/watch
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
    
+ Data
    + id (long) - 关联资源ID. type=channels时, id为频道id; type=users时, id为用户标识符, 后端会为该用户创建频道再进行关联; 
    + type (string, nullable) - 缺省值:channels, 可选值:[users|channels]资源类型, 关注用户或订阅频道;
    + displayOrder (int, nullable) - 缺省值:0, 排序字段;

+ Meta
    + relationships (string) - hide:表明这是用户与频道资源多对多关系中的_关注关联_

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
                    "type": "users"
                    "id": "2",
                    "displayOrder": 88
                },
                {
                    "type": "channels"
                    "id": "3",
                    "displayOrder": 89
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
                    "type": "users"
                    "id": "2",
                    "displayOrder": 88
                },
                {
                    "type": "channels"
                    "id": "3",
                    "displayOrder": 89
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
        + 只要参数中有该过滤条件, 不管其值是什么, 后端都会强制设置为当前登录用户;
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
        
## (POST|GET)评论(待定接口???) [/topics/{topicId}/comments]

+ Description
    + [MUST] Authenticated (添加评论时)
    + 或直接访问资源并添加过滤参数/comments?filter[themeId]=1
    + 添加品论时, 后端需要检查topicId是否被锁定, 是否允许回复
    + 根节点按热度(点赞数与评论数), 创建时间降序排序
    + 各种过滤条件与排序组合：
        + 【sort】按热度排序
        + 【sort】是创建时间排序
        + 【filter】是否是根节点
    + 点赞数?
    + 回复数?
    + parameters 通过设置此参数控制返回数据, 也可实现默认行为
        + include=tags 返回的结果中有标签
        + include=comments 返回的结果中有非根节点
        + include=tags,comments 都返回

+ Data
    + themeId (long) - topicId
    + topId (long) - root id
    + replayId (long) - parent id

### 增加评论 [POST]

+ Request (application/json)

        {
            "data": {
                "themeId": 1,
                "topId": 0,
                "replayId": 0,
                "content": "回复内容, 啊啊啊",
                "relationships": {
                    "tags": {
                        "data": [
                            {
                                "type": "tags",
                                "id": 1
                            },
                            {
                                "type": "tags",
                                "id": 2
                            }
                        ]
                    }
                }
            }
        }
        
+ Response 201 (application/json)

    + Headers

            Location: /api/comments/33

    + Body

            {
                "data": {
                    "id": 3,
                    "type": "comments"
                }
            }
            
+ Response 202 (application/json)

+ Response 204 (application/json)

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
                "self": "/api/comments?page[number]=1&page[size]=2",
                "first": "/api/comments?page[number]=1&page[size]=2",
                "next": "/api/comments?page[number]=2&page[size]=2",
                "last": "/api/comments?page[number]=2&page[size]=2"
            },
            "data": [
                {
                    "id": 444,
                    "themeId": 1,
                    "topId": 0,
                    "content": "回复内容, 啊啊啊",
                    "relationships": {
                        "tags": {
                            "data": [
                                {
                                    "id": 1,
                                    "tag": "好吃"
                                },
                                {
                                    "id": 2,
                                    "tag": "真不错"
                                }
                            ]
                        },
                        "comments": {
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
                                "self": "/api/comments?page[number]=1&page[size]=2&filter[replayId]=22",
                                "first": "/api/comments?page[number]=1&page[size]=2&filter[replayId]=22",
                                "next": "/api/comments?page[number]=2&page[size]=2&filter[replayId]=22",
                                "last": "/api/comments?page[number]=2&page[size]=2&filter[replayId]=22"
                            },
                            "data": [
                                {
                                    "id": 1,
                                    "themeId": 1,
                                    "topId": 10,
                                    "replayId": 444,
                                    "content": "回复的内容1",
                                    "relationships": {
                                        "tags": {
                                            "data": []
                                        },
                                        "comments": {
                                            "data": []
                                        }
                                    }
                                },
                                {
                                    "id": 2,
                                    "themeId": 1,
                                    "topId": 10,
                                    "replayId": 444,
                                    "content": "回复的内容2",
                                    "relationships": {
                                        "tags": {
                                            "data": []
                                        },
                                        "comments": {
                                            "data": []
                                        }
                                    }
                                }
                            ]
                        }
                    }
                },
                {
                    "id": 4,
                    "themeId": 1,
                    "topId": 10,
                    "content": "根节点内容, 啊啊啊",
                    "relationships": {
                        "tags": {
                            "data": []
                        },
                        "comments": {
                            "data": []
                        }
                    }
                }
            ]
        }
        
## (DELETE)删除评论 [/comments/{id}]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + 逻辑删除?

+ Parameters
    + id (long) - comment id

### 删除评论 [DELETE]

+ Response 204 (application/json)

## (GET)评论标签集合 [/comments/tags/]

+ Description
    + 

+ Data
    + 

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
                "self": "/api/tags?page[number]=1&page[size]=2",
                "first": "/api/tags?page[number]=1&page[size]=2",
                "next": "/api/tags?page[number]=2&page[size]=2",
                "last": "/api/tags?page[number]=2&page[size]=2"
            },
            "data": [
                {
                    "id": 1,
                    "tagName": "标签1",
                    "description": "标签描述1"
                },
                {
                    "id": 2,
                    "tagName": "标签2",
                    "description": "标签描述2"
                }
            ]
        }
        
## (GET)友情链接 [/links?filter[type]={typeId}]

+ Description
    + 

+ Data
    + 
    
+ Parameters
    + typeId (int) - 友情链接类型ID

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

## (GET)主题 [/api/topics/1]

+ Description
    + topic类型 : [产品|新闻|评测|原创]
    + post 类型 : [原创|爬取|精翻|机翻]
    
+ Meta
    + aggregations (object) - 原聚合数据, 结构不变;
    + clusters (array) - 原聚类数据, 结构不变;

+ Data
    + meta.highlight - 原高亮数据, 结构不变
    + post (object) - 页面主要显示数据。可翻译的内容文本数据， 标题 摘要 参数 内容等
        + 精翻|原创 > 机翻 > 爬取
    + post.parent (object) - post的parent节点
        + 如果post是翻译数据, 这里指向其parent节点, 即原文
    + relationships (object) - 关系数据对象
    + relationships.attachments (object) - 附件对象
    + relationships.topics (object) - 排重后的相同主题数据;
        + 如果该topic是clustering数据, 会返回相同数据来源不同的topics
        + 如果存在这个对象, 那么post对象里存放的是翻译合并的数据
    + relationships.products (object) - 产品相关数据;
    + relationships.documents (object) - 新闻和评测相关数据;
    + relationships.brands (object) - 品牌相关数据;
    + relationships.comments (object) - 评论相关数据;

+ Parameters
    + id (long) - comment id

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
                "type": "topics",
                "id": 1,
                "views": 99,
                "replies": 88,
                "thumbsUp": 90,
                "thumbsDown": 10,
                "thumbs": 80,
                "rating": 1,
                "forum": "产品|新闻|评测|视频",
                "topicType": "正常|置顶|公告",
                "locked": false,
                "moderated": false,
                "clustered": false,
                "created": "2016-10-12T01: 55: 37.000Z",
                "from": {
                    "seedId": 0,
                    "origin": "http: //product.mifanfan.cn/product/1",
                    "host": "http: //product.mifanfan.cn/",
                    "source": "MusicFans"
                },
                "post": {
                    "postType": "原创|爬取|精翻|机翻",
                    "category": [
                        "Keys"
                    ],
                    "tags": [
                        "VirusTI2Desk"
                    ],
                    "title": "（精翻, 如果有的话, 否则这里放原文）AccessVirusTI2Desktop",
                    "features": [
                        {
                            "_name": "SoundEngineType(s)",
                            "_value": "AnalogModeling"
                        }
                    ],
                    "descriptions": "AnalogModelingDesktopSynthesizerand24-bit/192kHzAudio/MIDIInterface",
                    "contents": "AnalogModelingDesktopSynthesizerand24-bit/192kHzAudio/MIDIInterface",
                    "parent": {
                        "postType": "原创|爬取",
                        "category": [
                            "Keys"
                        ],
                        "tags": [
                            "VirusTI2Desk"
                        ],
                        "title": "（精翻, 如果有的话, 否则这里放原文）AccessVirusTI2Desktop",
                        "features": [
                            {
                                "_name": "SoundEngineType(s)",
                                "_value": "AnalogModeling"
                            }
                        ],
                        "descriptions": "AnalogModelingDesktopSynthesizerand24-bit/192kHzAudio/MIDIInterface",
                        "contents": "AnalogModelingDesktopSynthesizerand24-bit/192kHzAudio/MIDIInterface"
                    }
                }
            },
            "relationships": {
                "attachments": {
                    "data": [
                        {
                            "type": "images",
                            "id": 1745,
                            "image": "http: //static.budee.com/yyren/image/6/1745.jpg",
                            "width": 1800,
                            "height": 500
                        },
                        {
                            "type": "images",
                            "id": 1746,
                            "image": "http: //static.budee.com/yyren/image/6/1746.jpg",
                            "width": 1600,
                            "height": 794
                        }
                    ]
                },
                "topics": {
                    "data": [
                        {
                            "meta": {
                                "_score": 0.987653
                            },
                            "type": "topics",
                            "id": 1,
                            "views": 99,
                            "replies": 88,
                            "thumbsUp": 90,
                            "thumbsDown": 10,
                            "thumbs": 80,
                            "rating": 1,
                            "created": "2016-10-12T01: 55: 37.000Z",
                            "from": {
                                "seedId": 0,
                                "origin": "http: //product.mifanfan.cn/product/1",
                                "host": "http: //product.mifanfan.cn/",
                                "source": "MusicFans"
                            },
                            "post": {
                                "category": [
                                    "Keys"
                                ],
                                "tags": [
                                    "VirusTI2Desk"
                                ],
                                "title": "AccessVirusTI2Desktop",
                                "features": [
                                    {
                                        "_name": "SoundEngineType(s)",
                                        "_value": "AnalogModeling"
                                    }
                                ],
                                "descriptions": "AnalogModelingDesktopSynthesizerand24-bit/192kHzAudio/MIDIInterface",
                                "contents": "AnalogModelingDesktopSynthesizerand24-bit/192kHzAudio/MIDIInterface"
                            }
                        },
                        {
                            "meta": {
                                "_score": 0.8876643
                            },
                            "type": "posts",
                            "id": 2
                        }
                    ]
                },
                "product": {
                    "data": {
                        "type": "products",
                        "id": "11"
                    }
                },
                "document": {
                    "data": {
                        "type": "documents",
                        "id": "21"
                    }
                },
                "brand": {
                    "data": {
                        "type": "brands",
                        "id": "Yamaha"
                    }
                },
                "comments": {
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
                        "self": "/api/comments?page[number]=1&page[size]=2",
                        "first": "/api/comments?page[number]=1&page[size]=2",
                        "next": "/api/comments?page[number]=2&page[size]=2",
                        "last": "/api/comments?page[number]=2&page[size]=2"
                    },
                    "data": [
                        {
                            "id": 444,
                            "themeId": 1,
                            "topId": 0,
                            "content": "回复内容, 啊啊啊",
                            "relationships": {
                                "tags": {
                                    "data": [
                                        {
                                            "id": 1,
                                            "tag": "好吃"
                                        },
                                        {
                                            "id": 2,
                                            "tag": "真不错"
                                        }
                                    ]
                                },
                                "comments": {
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
                                        "self": "/api/comments?page[number]=1&page[size]=2&filter[replayId]=22",
                                        "first": "/api/comments?page[number]=1&page[size]=2&filter[replayId]=22",
                                        "next": "/api/comments?page[number]=2&page[size]=2&filter[replayId]=22",
                                        "last": "/api/comments?page[number]=2&page[size]=2&filter[replayId]=22"
                                    },
                                    "data": [
                                        {
                                            "id": 1,
                                            "themeId": 1,
                                            "topId": 10,
                                            "replayId": 444,
                                            "content": "回复的内容1",
                                            "relationships": {
                                                "tags": {
                                                    "data": []
                                                },
                                                "comments": {
                                                    "data": []
                                                }
                                            }
                                        },
                                        {
                                            "id": 2,
                                            "themeId": 1,
                                            "topId": 10,
                                            "replayId": 444,
                                            "content": "回复的内容2",
                                            "relationships": {
                                                "tags": {
                                                    "data": []
                                                },
                                                "comments": {
                                                    "data": []
                                                }
                                            }
                                        }
                                    ]
                                }
                            }
                        },
                        {
                            "id": 4,
                            "themeId": 1,
                            "topId": 10,
                            "content": "根节点内容, 啊啊啊",
                            "relationships": {
                                "tags": {
                                    "data": []
                                },
                                "comments": {
                                    "data": []
                                }
                            }
                        }
                    ]
                }
            },
            "included": [
                {
                    "type": "brands",
                    "id": "Yamaha",
                    "attributes": {
                        "reviews": 0,
                        "top": false,
                        "name": "Access",
                        "rating": 0,
                        "logo": "http: //static.budee.com/yyren/image/220/14/974041.jpg"
                    }
                },
                {
                    "type": "documents",
                    "id": "21",
                    "attributes": {
                        "postDate": "2017-07-07 12:12:11",
                        "author": "作者",
                        "brand": [
                            "2Box",
                            "EV",
                            "Yamaha"
                        ]
                    }
                },
                {
                    "type": "products",
                    "id": "31",
                    "attributes": {
                        "price": 2185.02,
                        "priceUnit": "USD",
                        "range": [
                            2185.02,
                            2193.11,
                            2198.98
                        ],
                        "history": [
                            {
                                "date": "2017-01-01",
                                "price": 2000.01
                            },
                            {
                                "date": "2017-04-01",
                                "price": 2050.02
                            },
                            {
                                "date": "2017-07-01",
                                "price": 2198.98
                            }
                        ]
                    }
                }
            ]
        }
