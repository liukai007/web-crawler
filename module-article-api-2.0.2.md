FORMAT: 1A
HOST: http://192.168.1.138/

# topics

+ 2017年9月7日
    + 新增收藏主题列表API 
        + /topics/favorites

+ 2017年9月5日
    + 部分新API增加了模块前缀名称，未来老接口会逐渐过渡到这种格式
        + /article
    + 增加用户收藏主题API
    + 增加留言板API
    + 增加反馈信息API
    + 增加获取举报选项的API

文章主题模块2.0.2

## (POST|DELETE|PATCH)用户与主题关联关系(收藏) [/users/{userId}/relationships/topics]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + [OPTIONAL] 不写meta的话:/users/{userId}/relationships/topics/favorite
    
+ Meta
    + relationships (string) - favorite:表明这是用户与主题资源多对多关系中的_收藏关联_
    
+ Data
    + id (long) - 主题资源唯一标识符

+ Parameters
    + userId (long) - 用户ID

### 增加关联 [POST]

+ Request (application/json)

        {
            "meta": {
                "relationships": "favorite"
            },
            "data": [
                {
                    "id": "88"
                },
                {
                    "id": "90"
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
                "relationships": "favorite"
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
                "relationships": "favorite"
            },
            "data": [
                {
                    "id": "88"
                },
                {
                    "id": "90"
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
        
## (POST)用户与主题关联关系(举报) [/users/{userId}/relationships/topics]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + [OPTIONAL] 不写meta的话:/users/{userId}/relationships/topics/report
    + 非unique关联
    
+ Meta
    + relationships (string) - report:表明这是用户与主题资源多对多关系中的_举报关联_
    
+ Data
    + id (long) - 主题资源唯一标识符
    + attributes (object, nullable) - 资源属性
    + attributes.remark (string, nullable) - 举报备注信息
    + attributes.options (array, nullable) - 选项的ID数组

+ Parameters
    + userId (long) - 用户ID

### 增加关联 [POST]

+ Request (application/json)

        {
            "meta": {
                "relationships": "report"
            },
            "data": [
                {
                    "id": "92",
                    "attributes": {
                        "remark": "我要举报你",
                        "options": [
                            1,
                            2,
                            4
                        ]
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
        
## (GET) 收藏主题列表 [/topics/favorites]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源
    + 结构同搜索API

### 查询收藏主题列表 [GET]

+ Response 200 (application/json)

        {
        }

        
## (GET) 投票信息 [/article/votes/{id}]

+ Description
    + 用户举报主题时的复选/单选等详情
    + 获取举报相关的投票信息, 如下两种方式, ID=1的固定为举报投票的相关信息, 或者使用report
        + /article/votes/1
        + /article/votes/report
    + 获取精翻选项的投票信息,如下两种方式,ID=2的固定为精翻选项的相关信息, 或者使用translate
        + /article/votes/2
        + /article/votes/translate
    
+ Data
    + id (long) - vote ID
    + voteType (enum) - [CHECKBOX|RADIO]
    + voteText (string) - 投票描述
    + voteStart (date) - _保留字段_
    + voteLength (int) - _保留字段_
    + options (array) - 选项数组
    + options.id - 选项 ID
    + options.voteOptionText - 选项描述
    + options.voteOptionCount - 选项投票计数

### 查询投票信息 [GET]

+ Response 400 (application/json)

        {
            "data": {
                "id": 1,
                "voteType": "CHECKBOX",
                "voteText": "举报原因",
                "voteStart": "2017-09-05 11:37:48",
                "voteLength": 0,
                "options": [
                    {
                        "id": 1,
                        "voteOptionText": "反动言论",
                        "voteOptionCount": 6
                    },
                    {
                        "id": 2,
                        "voteOptionText": "文题不符",
                        "voteOptionCount": 6
                    },
                    {
                        "id": 3,
                        "voteOptionText": "错别字",
                        "voteOptionCount": 1
                    },
                    {
                        "id": 4,
                        "voteOptionText": "其他",
                        "voteOptionCount": 5
                    }
                ]
            }
        }
        
## (POST|GET) 留言板 [/article/messages]

+ Description
    + sort=-created 按创建时间降序排序查询列表
    
+ Data
    + message (string) - 用户留言

### 新增留言 [POST]

+ Request (application/json)

        {
            "data": {
                "message": "米饭网挺好"
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /article/messages/1

    + Body

            {
                "data": {
                    "id": 1,
                    "type": "messages"
                }
            }
            
+ Response 204 (application/json)
        
+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "Pattern.Messages.message",
                    "title": "Bad Request",
                    "detail": "留言字符长度应在2~140之间",
                    "source": {
                        "pointer": "Messages -> message"
                    }
                }
            ]
        }
        
### 查询留言板集合 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "totalPages": 1,
                "totalElements": 3,
                "size": 10,
                "number": 1,
                "numberOfElements": 3,
                "first": true,
                "last": true,
                "sort": [
                    {
                        "direction": "DESC",
                        "property": "created",
                        "ignoreCase": false,
                        "nullHandling": "NATIVE",
                        "ascending": false,
                        "descending": true
                    }
                ]
            },
            "links": {
                "self": "/article/messages?sort=-created&page[number]=1&page[size]=10",
                "first": "/article/messages?sort=-created&page[number]=1&page[size]=10",
                "last": "/article/messages?sort=-created&page[number]=1&page[size]=10"
            },
            "data": [
                {
                    "id": 3,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-09-02 15:09:29",
                    "modified": "2017-09-02 15:09:29",
                    "message": "messagemessagemessagemessage"
                },
                {
                    "id": 2,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-09-02 15:09:27",
                    "modified": "2017-09-02 15:09:27",
                    "message": "messagemessagemessagemessage"
                },
                {
                    "id": 1,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-09-02 15:09:24",
                    "modified": "2017-09-02 15:09:24",
                    "message": "messagemessagemessagemessage"
                }
            ]
        }   
        
## (POST|GET) 反馈 [/article/feedback]

+ Description
    + [MUST] Authenticated
    + <del>feedback没有单独的复数表述形式</del>
    
+ Data
    + username (string) - 姓名
    + mobile (string) - 手机号, 联系方式
    + email (string) - 邮箱
    + suggestion (string) - 反馈建议信息

### 新增反馈 [POST]

+ Request (application/json)

        {
            "data": {
                "username": "张三",
                "mobile": "13523456434",
                "email": "12323@qq.com",
                "suggestion": "反馈建议信息"
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /article/feedback/1

    + Body

            {
                "data": {
                    "id": 1,
                    "type": "feedback"
                }
            }
            
+ Response 204 (application/json)
        
+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "Pattern.Messages.message",
                    "title": "Bad Request",
                    "detail": "留言字符长度应在2~140之间",
                    "source": {
                        "pointer": "Messages -> message"
                    }
                }
            ]
        }
        
### 查询反馈列表集合 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "totalPages": 1,
                "totalElements": 4,
                "size": 10,
                "number": 1,
                "numberOfElements": 4,
                "first": true,
                "last": true,
                "sort": [
                    {
                        "direction": "DESC",
                        "property": "created",
                        "ignoreCase": false,
                        "nullHandling": "NATIVE",
                        "ascending": false,
                        "descending": true
                    }
                ]
            },
            "links": {
                "self": "/article/feedback?sort=-created&page[number]=1&page[size]=10",
                "first": "/article/feedback?sort=-created&page[number]=1&page[size]=10",
                "last": "/article/feedback?sort=-created&page[number]=1&page[size]=10"
            },
            "data": [
                {
                    "id": 4,
                    "enabled": 1,
                    "creator": 11,
                    "modifier": 11,
                    "created": "2017-09-02 15:07:14",
                    "modified": "2017-09-02 15:07:14",
                    "username": "quzile",
                    "mobile": "17611019209",
                    "email": "1929593@qq.com",
                    "suggestion": "建议22333233"
                },
                {
                    "id": 3,
                    "enabled": 1,
                    "creator": 11,
                    "modifier": 11,
                    "created": "2017-09-02 15:06:51",
                    "modified": "2017-09-02 15:06:51",
                    "username": "quzile",
                    "mobile": "17611019209",
                    "email": "1929593@qq.com",
                    "suggestion": "建议22333233"
                }
            ]
        }
        
+ 2017年9月9日
    + API初始化

## 精翻

+ Data
    + id (long) - 文档id
    + parentId (long) - 父文档id
    + postId (long) - 文档id
    + topicId (long) - 主题id
    + language (int) - 语言 0:缺省, 1:中文, 2英文
    + features (数组) - 属性
    + title (String) - 标题
    + description (String) - 描述
    + content (String) - 内容
    + state (int) - 状态 1:草稿箱, 2:待审核, 3:审核通过, 4:审核失败, 5:被覆盖, 9:已删除
    + isAdmin (boolean) - 是否是管理员
    + parentTitle (String) - 父文档标题
    + remark (String) - 审核说明

### 精翻列表 [GET] /article/humanTranslates?filter[parentTitle]=雅马哈&filter[topicId]=23373

+ Description
    + [MUST] Authenticated

    
+ Parameters
    + parentTitle (String) 父文档标题
    + topicId (long) 主题id
    + title (String) 文档标题
    + state (int) 

    
    {
        "meta": {
            "totalPages": 1,
            "totalElements": 3,
            "size": 10,
            "number": 1,
            "numberOfElements": 3,
            "first": true,
            "last": true,
            "sort": [
              {
                "direction": "DESC",
                "property": "created",
                "ignoreCase": false,
                "nullHandling": "NATIVE",
                "descending": true,
                "ascending": false
              }
            ]
          },
          "links": {
            "self": "/article/humanTranslates?page[number]=1&page[size]=10",
            "first": "/article/humanTranslates?page[number]=1&page[size]=10",
            "last": "/article/humanTranslates?page[number]=1&page[size]=10"
          },
          "data": [
            {
              "id": 852804,
              "creator": 1031,
              "modifier": 1031,
              "created": "2017-09-08 14:11:24",
              "modified": "2017-09-08 14:11:24",
              "topicId": 413,
              "title": "ETY Plugs 翻译High Fidelity Earplugs - Large Fit",
              "state": 1,
              "parentTitle": "ETY Plugs High Fidelity Earplugs - Large Fit"
            },
            {
              "id": 852390,
              "creator": 1031,
              "modifier": 1031,
              "created": "2017-09-07 16:56:56",
              "modified": "2017-09-07 16:56:56",
              "topicId": 36176,
              "title": "Tuba翻译Strap",
              "state": 1,
              "parentTitle": "Tuba Strap"
            },
            {
              "id": 852377,
              "creator": 1031,
              "modifier": 1031,
              "created": "2017-09-07 15:11:46",
              "modified": "2017-09-07 15:11:46",
              "topicId": 457999,
              "title": "刘凯产品的翻译",
              "state": 1,
              "parentTitle": "刘凯222产品"
            }
          ]
    }


### 精翻详情 [GET] /article/humanTranslates/{id}

+ Description
    + [MUST] Authenticated
    

    { 
        "data": {
            "id": 853974,
            "enabled": 0,
            "creator": 1031,
            "modifier": 1031,
            "created": "2017-09-11 16:37:00",
            "modified": "2017-09-11 17:31:10",
            "parentId": 23373,
            "topicId": 23373,
            "priority": 0,
            "language": 0,
            "categories": [],
            "tags": [],
            "features": [
              {
                "_name": "12123342属性2",
                "_value": "中国"
              }
            ],
            "title": "翻译Taylor T5z Custom, Sweetwater Exclusive - Cocobolo",
            "moderation": {
              "id": 5,
              "enabled": 1,
              "creator": 1031,
              "modifier": 1031,
              "created": "2017-09-11 16:37:00",
              "modified": "2017-09-11 17:34:00",
              "postId": 853974,
              "state": 9
            },
            "parent": {
              "id": 23373,
              "enabled": 1,
              "creator": 0,
              "modifier": 0,
              "created": "2016-10-12 15:25:02",
              "modified": "2016-10-17 14:31:47",
              "parentId": 0,
              "topicId": 23373,
              "priority": 0,
              "language": 2,
              "categories": [
                "Guitars and Basses",
                "Acoustic Guitars",
                "Acoustic / Electric Guitars"
              ],
              "tags": [
                "T5z6CustomCB"
              ],
              "features": [
                {
                  "_name": "Body Type",
                  "_value": "Hollowbody"
                },
                {
                  "_name": "Body Shape",
                  "_value": "T5"
                }
              ],
              "title": "Taylor T5z Custom, Sweetwater Exclusive - Cocobolo",
              "description": "6-string Thinline Acoustic-electric Guitar with Cocobolo Top, Sapele Body, Acoustic Body Sensor System, 2 Humbucking Pickups, and Hard Case - Natural",
              "content": "<h2>This Cocobolo T5z Custom is Exclusive to Sweetwater!</h2> "
              },
              "postTypeValue": "爬取",
              "postType": 1
            },
            "postsText": {
              "id": 853974,
              "category": "",
              "title": "翻译Taylor T5z Custom, Sweetwater Exclusive - Cocobolo",
              "feature": "[{\"_name\":\"12123342属性2\",\"_value\":\"中国\"}]"
            },
            "postTypeValue": "精翻",
            "postType": 3
          }
    }

 
### 增加精翻 [POST] /article/humanTranslates

+ Description
    + [MUST] Authenticated
    
+ Parameters
    + parentId  - 必填
    + topicId - 必填
    + language
    + features
    + title - 必填
    + description
    + content
    + state - 范围1~2

+ 新增Request (application/json)
    
        "data":{
                "topicId":23373,
                "parentId":23373,
                "title":"fan'yi翻译Taylor T5z Custom, Sweetwater Exclusive - Cocobolo",
                "state":1,
                "features":[
                            {
                                "_name":"属性2",
                                "_value":"中国"
                            }
                        ]
            }

+ Response 201 (application/json)

    + Headers

            Location: /article/humanTranslates/2
    + Body

            {
                "data": {
                    "id": 2,
                    "type": "humanTranslates"
                }
            }

### 修改精翻 [PATCH] /article/humanTranslates/{id}

+ Description
    + [MUST] Authenticated
+ Parameters
    + title - 必填
    + description
    + content
    + state - 范围1~2
    + language
    + features

+ 修改Request (application/json)

        "data":{
        "title":"翻译Taylor T5z Custom, Sweetwater Exclusive - Cocobolo",
        "state":2,
        "features":[
                    {
                        "_name":"12123342属性2",
                        "_value":"中国"
                    }
                ]
    }
    
+ Response 200 (application/json)
    
### 删除精翻 [DELETE] /article/humanTranslates/{id}

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


## 希望精翻

+ Data
    + id (long) - 品牌名称
    + userId (long) - 用户id
    + extendId (long) - 扩展id
    + why (String) - 对我有什么帮助
    + votesOptionIds (String) - 要求翻译的部分
    + topicId (long) - 主题id
    + state (int) - 状态，0：待翻 1：已翻 2：驳回
    + feedBack (String) - 管理员反馈
    + who (long) - 谁翻译的
    + userCount (int) - 多少人希望精翻


### 用户查询希望精翻列表 [GET] /article/hopeTranslates

+ Description
    + [MUST] Authenticated
    + 本接口供前台普通用户查询使用
+ Response 200 (application/json)
        
          "meta": {
            "totalPages": 1,
            "totalElements": 2,
            "size": 10,
            "number": 1,
            "numberOfElements": 2,
            "first": true,
            "last": true,
            "sort": null
          },
          "links": {
            "self": "/article/hopeTranslates?page[number]=1&page[size]=10",
            "first": "/article/hopeTranslates?page[number]=1&page[size]=10",
            "last": "/article/hopeTranslates?page[number]=1&page[size]=10"
          },
          "data": [
            {
              "id": 5,
              "enabled": 1,
              "created": "2017-09-07 14:13:59",
              "modified": "2017-09-07 14:13:59",
              "userId": 1031,
              "extendId": 9,
              "why": "dui'wo对我 hen'you很有 bang帮 zhu住 ",
              "extend": {
                "id": 9,
                "topicId": 1,
                "state": 1,
                "title": "Virus TI2 Desktop"
              }
            },
            {
              "id": 6,
              "enabled": 1,
              "created": "2017-09-07 14:38:27",
              "modified": "2017-09-07 14:38:27",
              "userId": 1031,
              "extendId": 10,
              "why": "222222",
              "extend": {
                "id": 10,
                "topicId": 2,
                "state": 0,
                "title": "Virus TI2 Keyboard"
              }
            }
          ]

### 增加希望精翻 [POST] /article/hopeTranslates/

+ Description
    + [MUST] Authenticated
    + 本接口供前台普通用户使用

+ Parameters
    + extend.topicId  - 必填
    + why  - 必填
    + votesOptionIds

+ 新增Request (application/json)

        "data":{
            "why":"这篇文章对我很有用，希望能有人翻译一下。",
            "extend":{
                "topicId":36176
            }
        }
+ Response 201 (application/json)

    + Headers

            Location: /article/hopeTranslates/2
    + Body

            {
                "data": {
                    "id": 2,
                    "type": "hopeTranslates"
                }
            }

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

### 删除希望精翻 [DELETE] /article/hopeTranslates/{id}

+ Description
    + [MUST] Authenticated
    + 本接口供前台普通使用

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
        

## 审核精翻

+ Data
    + remark (String) - 审核说明
    + state (int) - 状态 1:草稿箱, 2:待审核, 3:审核通过, 4:审核失败, 5:被覆盖, 9:已删除
    + riceNum (int) - 奖励的米粒个数

+ Parameters
    + topicId
    + state

### 待审核列表 [GET] /article/hopeTranslates?filter[state]=1

    
    {
      "meta": {
        "totalPages": 1,
        "totalElements": 4,
        "size": 10,
        "number": 1,
        "numberOfElements": 4,
        "first": true,
        "last": true,
        "sort": null
      },
      "links": {
        "self": "/article/moderations?page[number]=1&page[size]=10",
        "first": "/article/moderations?page[number]=1&page[size]=10",
        "last": "/article/moderations?page[number]=1&page[size]=10"
      },
      "data": [
        {
          "id": 2,
          "enabled": 1,
          "creator": 1031,
          "modifier": 1031,
          "created": "2017-09-07 15:11:46",
          "modified": "2017-09-07 15:11:46",
          "postId": 852377,
          "state": 1,
          "title": "刘凯产品的翻译"
        },
        {
          "id": 3,
          "enabled": 1,
          "creator": 1031,
          "modifier": 1031,
          "created": "2017-09-07 16:56:56",
          "modified": "2017-09-07 16:56:56",
          "postId": 852390,
          "state": 1,
          "title": "Tuba翻译Strap"
        },
        {
          "id": 4,
          "enabled": 1,
          "creator": 1031,
          "modifier": 1031,
          "created": "2017-09-08 14:11:24",
          "modified": "2017-09-08 15:33:24",
          "postId": 852804,
          "state": 1,
          "title": "ETY Plugs 翻译High Fidelity Earplugs - Large Fit"
        }
      ]
    }

### 审核 [PATCH] /article/hopeTranslates/audit/{id}

+ Description
    + [MUST] Authenticated
    + [MUST] administrator

+ Parameters
    + remark - 必填
    + state - 必填 范围限制3~4
    + riceNum 0-100

+ 审核Request (application/json)

        {
            "data":{
                "state":3,
                "remark":"111翻译的不错 "
            }
        }
        
+ Response 200 (application/json)
