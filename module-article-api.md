FORMAT: 1A
HOST: http://polls.apiblueprint.org/

# topics

+ 2017年6月30日
    + 新增上传文件验证 API
    + 新增上传文件文档

文章主题模块

## (POST|DELETE|PATCH)用户与文章主题关联关系(HIDE) [/api/users/{userId}/relationships/topics]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    
+ Fields
    
+ Data
    + id (long) - 文章主题ID
    + remark (string) - 原因描述

+ Meta
    + relationships (string) - [hide|like] hide:隐藏关联, like:喜欢关联

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
        
## (POST|DELETE|PATCH)用户与文章主题关联关系(LIKE) [/api/users/{userId}/relationships/topics]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    
+ Fields
    
+ Data
    + id (long) - 文章主题ID
    + remark (string) - 原因描述

+ Meta
    + relationships (string) - [hide|like] hide:隐藏关联, like:喜欢关联

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
        
## (POST|GET)精翻文章 [/api/posts]

+ Description
    + [MUST] Authenticated
    + [MUST] has ROLE_TRANSLATOR (拥有翻译角色权限)
    + 只有当parentId不为null并且不为0时, 才是精翻
    + 翻译字段：标题、参数、描述、内容;
    + 只能翻译爬取的文章, 即parent_id=0 AND creator=0的文档;
    + 翻译格式：富文本格式、key-value格式、list格式等、json格式等;
    + 当新增了一条精翻时, 需要新增一行moderations,翻译审核记录;

+ Data
    + parentId (long) - 原文ID, 翻译要指向原文ID
    + topicId (long) - 原文所属主题ID
    + postsText.title (string) - 标题
    + postsText.feature (string) - 特性
    + postsText.description (string) - 描述
    + postsText.content (string) - 内容

### 增加主题回复(精翻) [POST]

+ Request (application/json)

        {
            "data": {
                "parentId": "2",
                "topicId": "102",
                "postsText": {
                    "title": "Access Virus TI2 Desktop",
                    "feature": "features",
                    "description": "Analog Modeling Desktop Synthesizer and 24-bit/192kHz Audio/MIDI Interface",
                    "content": "<h2>Access Steps Up The Renowned Virus!</h2>"
                }
            }
        }
        
+ Response 201 (application/json)

    + Headers

            Location: /api/posts/3

    + Body

            {
                "data": {
                    "id": 3,
                    "type": "posts"
                }
            }
            
+ Response 202 (application/json)

+ Response 204 (application/json)

### 查询主题回复集合(精翻) [GET]

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
                "self": "/api/posts?page[number]=1&page[size]=2",
                "first": "/api/posts?page[number]=1&page[size]=2",
                "next": "/api/posts?page[number]=2&page[size]=2",
                "last": "/api/posts?page[number]=2&page[size]=2"
            },
            "data": [
                {
                    "id": 1,
                    "creator": 20,
                    "created": "2017-01-01 12:34:45",
                    "modified": "2017-01-01 12:34:45",
                    "priority": 88,
                    "postsText": {
                        "title": "AccessVirusTI2Desktop",
                        "feature": "features",
                        "description": "AnalogModelingDesktopSynthesizerand24-bit/192kHzAudio/MIDIInterface",
                        "content": "<h2>AccessStepsUpTheRenownedVirus!</h2>"
                    }
                },
                {
                    "id": 1,
                    "creator": 20,
                    "created": "2017-01-01 12:34:45",
                    "modified": "2017-01-01 12:34:45",
                    "priority": 88,
                    "postsText": {
                        "title": "AccessVirusTI2Desktop",
                        "feature": "features",
                        "description": "AnalogModelingDesktopSynthesizerand24-bit/192kHzAudio/MIDIInterface",
                        "content": "<h2>AccessStepsUpTheRenownedVirus!</h2>"
                    }
                }
            ]
        }


## (DELETE|PATCH|GET)精翻文章 [/api/posts/{id}]

+ Description
    + [MUST] Authenticated
    + [MUST] has ROLE_TRANSLATOR (拥有翻译角色权限)
    + [MUST] 用户只能操作自己的资源

+ Data
    + id (long) - post id
    + priority (int) - 优先级
    + postsText.title (string) - 标题
    + postsText.feature (string) - 特性
    + postsText.description (string) - 描述
    + postsText.content (string) - 内容

+ Parameters
    + id (long) - post id

### 删除主题回复(精翻) [DELETE]

+ Response 204 (application/json)

### 修改主题回复(精翻) [PATCH]

+ Request (application/json)

        {
            "data": {
                "id": 1,
                "postsText": {
                    "title": "Access Virus TI2 Desktop",
                    "feature": "features",
                    "description": "Analog Modeling Desktop Synthesizer and 24-bit/192kHz Audio/MIDI Interface",
                    "content": "<h2>Access Steps Up The Renowned Virus!</h2>"
                }
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)

### 查询主题回复(精翻) [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 1,
                "creator": 20,
                "created": "2017-01-01 12:34:45",
                "modified": "2017-01-01 12:34:45",
                "priority": 88,
                "postsText": {
                    "title": "AccessVirusTI2Desktop",
                    "feature": "features",
                    "description": "AnalogModelingDesktopSynthesizerand24-bit/192kHzAudio/MIDIInterface",
                    "content": "<h2>AccessStepsUpTheRenownedVirus!</h2>"
                }
            }
        }
        
## (POST|GET)原创文章 [/api/topics]

+ Description
    + [MUST] Authenticated

+ Data
    + forumId (long) - (可选, 默认原创) 新闻, 视频, 评测, 产品, 原创
    + topicStatus (int) - (可选, 默认0) 是否锁定, 0:否, 1:是, 锁定主题不能回复
    + topicType (int) - (可选, 默认0) 主题类型, 0:正常, 1:置顶, 2:公告
    + postsText.category (string) - 类别
    + postsText.tag (string) - 标签
    + postsText.title (string) - 标题
    + postsText.feature (string) - 特性
    + postsText.description (string) - 描述
    + postsText.content (string) - 内容
    + relationships (object) - (可选) 资源关联关系对象, 其成员必须为资源对象
    + relationships.attachments (object) - 附件资源对象
    + relationships.attachments.data (array) - 关联数组
    + relationships.attachments.data.id (int) - 附件资源ID

### 增加主题(原创) [POST]

+ Request (application/json)

        {
            "data": {
                "forumId": 4,
                "topicStatus": 0,
                "topicType": 0,
                "postsText": {
                    "category": "product, 2box, yamaha",
                    "tag": "guitars, bass, nice",
                    "title": "Access Virus TI2 Desktop",
                    "feature": "features",
                    "description": "Analog Modeling Desktop Synthesizer and 24-bit/192kHz Audio/MIDI Interface",
                    "content": "<h2>Access Steps Up The Renowned Virus!</h2>"
                },
                "relationships": {
                    "attachments": {
                        "data": [
                            {
                                "id": 1
                            },
                            {
                                "id": 2
                            }
                        ]
                    }
                }
            }
        }
        
+ Response 201 (application/json)

    + Headers

            Location: /api/topics/33

    + Body

            {
                "data": {
                    "id": 3,
                    "type": "topics"
                }
            }
            
+ Response 202 (application/json)

+ Response 204 (application/json)

### 查询主题(原创) [GET]

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
                "self": "/api/topics?page[number]=1&page[size]=2",
                "first": "/api/topics?page[number]=1&page[size]=2",
                "next": "/api/topics?page[number]=2&page[size]=2",
                "last": "/api/topics?page[number]=2&page[size]=2"
            },
            "data": [
                {
                    "forumId": 4,
                    "topicStatus": 0,
                    "topicType": 0,
                    "postsText": {
                        "category": "product, 2box, yamaha",
                        "tag": "guitars, bass, nice",
                        "title": "Access Virus TI2 Desktop",
                        "feature": "features",
                        "description": "Analog Modeling Desktop Synthesizer and 24-bit/192kHz Audio/MIDI Interface",
                        "content": "<h2>Access Steps Up The Renowned Virus!</h2>"
                    },
                    "relationships": {
                        "images": {
                            "links": {
                                "self": "/api/topics/1/relationships/attachments?filter[mime]=jpg",
                                "related": "/api/topics/1/attachments"
                            },
                            "data": [
                                {
                                    "id": 1,
                                    "filename": "http://www.mifan.cn/12/1.jpg",
                                    "title": "title"
                                },
                                {
                                    "id": 2,
                                    "filename": "http://www.mifan.cn/12/2.jpg",
                                    "title": "title"
                                }
                            ]
                        },
                        "audios": {
                            "data": [
                                {
                                    "id": 11,
                                    "filename": "DJixzjio6KE"
                                },
                                {
                                    "id": 12
                                }
                            ]
                        },
                        "videos": {
                            "data": [
                                {
                                    "id": 101
                                },
                                {
                                    "id": 201
                                }
                            ]
                        }
                    }
                }
            ]
        }
        
## (DELETE|PATCH|GET)原创文章 [/api/topics/{id}]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源

+ Data
    + id (long) - post id
    + priority (int) - 优先级
    + postsText.title (string) - 标题
    + postsText.feature (string) - 特性
    + postsText.description (string) - 描述
    + postsText.content (string) - 内容

+ Parameters
    + id (long) - topic id

### 删除主题(原创) [DELETE]

+ Response 204 (application/json)

### 修改主题(原创) [PATCH]

+ Request (application/json)

        {
            "data": {
                "id": 1,
                "forumId": 4,
                "topicStatus": 0,
                "topicType": 0,
                "postsText": {
                    "category": "product, 2box, yamaha",
                    "tag": "guitars, bass, nice",
                    "title": "Access Virus TI2 Desktop",
                    "feature": "features",
                    "description": "Analog Modeling Desktop Synthesizer and 24-bit/192kHz Audio/MIDI Interface",
                    "content": "<h2>Access Steps Up The Renowned Virus!</h2>"
                },
                "relationships": {
                    "images": {
                        "links": {
                            "self": "/api/topics/1/relationships/attachments?filter[mime]=jpg",
                            "related": "/api/topics/1/attachments"
                        },
                        "data": [
                            {
                                "id": 1,
                                "filename": "http://www.mifan.cn/12/1.jpg",
                                "title": "title"
                            },
                            {
                                "id": 2,
                                "filename": "http://www.mifan.cn/12/2.jpg",
                                "title": "title"
                            }
                        ]
                    },
                    "audios": {
                        "data": [
                            {
                                "id": 11,
                                "filename": "DJixzjio6KE"
                            },
                            {
                                "id": 12
                            }
                        ]
                    },
                    "videos": {
                        "data": [
                            {
                                "id": 101
                            },
                            {
                                "id": 201
                            }
                        ]
                    }
                }
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)

### 查询主题(原创) [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 1,
                "forumId": 4,
                "topicStatus": 0,
                "topicType": 0,
                "topicViews": 999,
                "topicReplies": 99,
                "topicLikes": 33
                "postsText": {
                    "category": "product, 2box, yamaha",
                    "tag": "guitars, bass, nice",
                    "title": "Access Virus TI2 Desktop",
                    "feature": "features",
                    "description": "Analog Modeling Desktop Synthesizer and 24-bit/192kHz Audio/MIDI Interface",
                    "content": "<h2>Access Steps Up The Renowned Virus!</h2>"
                },
                "relationships": {
                    "images": {
                        "links": {
                            "self": "/api/topics/1/relationships/attachments?filter[mime]=jpg",
                            "related": "/api/topics/1/attachments"
                        },
                        "data": [
                            {
                                "id": 1,
                                "filename": "http://www.mifan.cn/12/1.jpg",
                                "title": "title"
                            },
                            {
                                "id": 2,
                                "filename": "http://www.mifan.cn/12/2.jpg",
                                "title": "title"
                            }
                        ]
                    },
                    "audios": {
                        "data": [
                            {
                                "id": 11,
                                "filename": "DJixzjio6KE"
                            },
                            {
                                "id": 12
                            }
                        ]
                    },
                    "videos": {
                        "data": [
                            {
                                "id": 101
                            },
                            {
                                "id": 201
                            }
                        ]
                    }
                }
            }
        }

## (GET)翻译审核集合 [/api/moderations]

+ Description
    + [MUST] Authenticated
    + [MUST] has ROLE_TRANSLATOR (拥有翻译审核角色权限)

+ Data
    + state (int) - 审核状态
    + remark (string) - 备注

### 查询翻译审核 [GET]

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
                "self": "/api/moderations?page[number]=1&page[size]=2",
                "first": "/api/moderations?page[number]=1&page[size]=2",
                "next": "/api/moderations?page[number]=2&page[size]=2",
                "last": "/api/moderations?page[number]=2&page[size]=2"
            },
            "data": [
                {
                    "id": 1,
                    "postId": "22",
                    "remark": "remark",
                    "state": 2,
                    "stateVal": "待审核",
                    "creator": 1,
                    "creatorVal": "张三",
                    "modifier": "2",
                    "modifierVal": "管理员",
                    "postsText": {
                        "title": "AccessVirusTI2Desktop",
                        "feature": "features",
                        "description": "AnalogModelingDesktopSynthesizerand24-bit/192kHzAudio/MIDIInterface",
                        "content": "<h2>AccessStepsUpTheRenownedVirus!</h2>"
                    }
                }
            ]
        }
        
## (DELETE|PATCH|GET)翻译审核 [/api/moderations/{id}]

+ Description
    + [MUST] Authenticated
    + [MUST] has ROLE_TRANSLATOR (拥有翻译审核角色权限)

+ Data
    + 

+ Parameters
    + id (long) - moderation id

### 删除翻译审核 [DELETE]

+ Response 204 (application/json)

### 修改翻译审核 [PATCH]

+ Request (application/json)

        {
            "data": {
                "id": 1,
                "remark": "通过了, 翻译的不错, 小伙子",
                "state": 8
            }
        }        
        
+ Response 200 (application/json)

+ Response 204 (application/json)

### 查询翻译审核 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 1,
                "postId": "22",
                "remark": "remark",
                "state": 2,
                "stateVal": "待审核",
                "creator": 1,
                "creatorVal": "张三",
                "modifier": "2",
                "modifierVal": "管理员",
                "postsText": {
                    "title": "AccessVirusTI2Desktop",
                    "feature": "features",
                    "description": "AnalogModelingDesktopSynthesizerand24-bit/192kHzAudio/MIDIInterface",
                    "content": "<h2>AccessStepsUpTheRenownedVirus!</h2>"
                }
            }
        }
        
## (POST|DELETE|PATCH)用户与频道关联关系(WATCH) [/api/users/{userId}/relationships/channels]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + 当type=users, 要先update[user_profile]表的用户是否开通频道字段（条件中需要带上 该字段=false）
        + 如果返回1, 说明是首次开通频道, 需要创建一个用户频道;
        + 如果返回0, 说明已开通, 直接根据userId查找用户频道;
            + 如果没有找到用户频道, 抛出异常, 稍后在请求, 原因可能是并发请求时, 创建用户频道的事务还没有提交;
            + 如果找到用户频道, 就建立关联关系;
    + NOTE: 这是典型的SELECT FOR UPDATE应用场景, 转换为效率更高的先UPDATE, 再执行其他逻辑;
    + NOTE: 这是分布式事务, 先更新用户限界上下文, 再处理文章限界上下文, 先UPDATE可以确保用户频道的唯一性;
    + 对于频道来说是【订阅/取消】, 对于用户频道来说是【关注/取消关注】,关注用户API整合
    + 资源与资源的多对多关联中, 可以存在多个多对多关联, 通过meta元数据区分他们;
    + meta中的relationships表明的是在用户(users)与频道(channels)的多对多关联中，这是一个watch多对多关联;
    
+ Data
    + id (long) - 关联资源ID
    + type (string) - (默认channels) [users|channels] 资源类型, 关注用户或订阅频道
    + displayOrder (int) - 排序

+ Meta
    + relationships (string) - [watch] watch:订阅/关注关联

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
        
## (GET)频道集合 [/api/channels]

+ Description
    + 不加过滤条件查询所有频道集合
    + filter[channelType]=1,2 : 只查询系统频道与订阅频道, 不查询用户频道(查询所有频道的时候用此过滤参数)
    + filter[users\_channels\_watch.user_id]=1 : 查询用户1订阅的频道
        + 普通用户角色ROLE_USER, 只要参数中有该过滤条件, 不管其值是什么, 后端都会强制设置为当前登录用户;
        + 超级管理员ROLE_ADMIN, 可以查询任意过滤参数;
    + 后端强制添加enabled=1过滤条件
    + 排序:sort=users\_channels\_watch.displayOrder, 按用户设置的顺序排序(需要配合用户过滤条件使用);
    + 排序:sort=-watched, 按关注热度降序排序;

+ Data
    + subscribed (int) : 用户是否订阅, 用户登录时增加该显示字段
    + displayOrder (int) : 排序, 用户登录时增加该显示字段

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
        
## (GET)标签统计集合 [/api/statistics/tags]

+ Description
    + filter[between|current_hour]=[2017-07-01 09:00:00,2017-07-02 09:00:00)&group[]=tag

+ Data

### 查询标签统计集合 [GET]

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
                "self": "/api/statistics/tags?page[number]=1&page[size]=2",
                "first": "/api/statistics/tags?page[number]=1&page[size]=2",
                "next": "/api/statistics/tags?page[number]=2&page[size]=2",
                "last": "/api/statistics/tags?page[number]=2&page[size]=2"
            },
            "data": [
                {
                    "tag": "guitars",
                    "totalCount": "1100"
                },
                {
                    "tag": "bass",
                    "totalCount": "112"
                },
                {
                    "tag": "2box",
                    "totalCount": "11"
                }
            ]
        }
        
## (POST|GET)评论 [/api/topics/{topicId}/comments]

+ Description
    + [MUST] Authenticated (添加评论时)
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
        
## (DELETE)翻译评论 [/api/comments/{id}]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + 逻辑删除?

+ Parameters
    + id (long) - comment id

### 删除翻译审核 [DELETE]

+ Response 204 (application/json)

## (GET)评论标签集合 [/api/comments/tags/]

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
        
## (GET)友情链接 [/api/links?filter[type]={typeId}]

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
