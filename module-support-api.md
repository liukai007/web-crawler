FORMAT: 1A
HOST: http://polls.apiblueprint.org/

# support

+ 2017年7月12日
    + API初始化

## 评论模块

## 评论 [/comments]
    
+ Data
    + confId (long) - 评论配置标识，必填
    + themeId (long) - 主题标识，必填
    + topId (long) - 一级评论标识，必填
    + replayId (long) - 被回复的评论标识
    + reUserId (long) - 被回复的用户标识
    + content (string) - 评论内容
    + isAnonymous (int) - 是否匿名
    + tagIds (array) - 评论时填写的标签
    + praiseCount (long) - 点赞数
    + _praiseCount (long) - 踩数
    + replayCount (long) - 回复数
    + currentScore (int) - 当前用户赞踩状态，1：赞，-1：踩
    + nickName (String) - 昵称
    + userAvatar (String) - 头像
    + reUserNickName (String) - 被回复的用户昵称

### 增加评论 [POST]

+ Description
    + [MUST] Authenticated

+ Request (application/json)

        {
            "data": {
                "confId": 1,
                "themeId": 1,
                "topId": 1,
                "replayId": 1,
                "reUserId": 1031,
                "content": "回复内容, 啊啊啊",
                "isAnonymous": 0,
                "tagIds": [
                    1,
                    2
                ]
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /comments/2

    + Body

            {
                "data": {
                    "id": 2,
                    "type": "comments"
                }
            }
            
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

## (GET)评论集合 [/comments?page[number]=1&page[size]=5&filter[confId]=1&filter[themeId]=1&filter[topId]=1]

### 查询评论集合 [GET]

+ Response 200 (application/json)

        {
          "meta": {
            "number": 1,
            "size": 5,
            "numberOfElements": 5,
            "last": false,
            "totalPages": 2,
            "sort": [
              {
                "direction": "DESC",
                "property": "created",
                "ignoreCase": false,
                "nullHandling": "NATIVE",
                "ascending": false,
                "descending": true
              }
            ],
            "first": true,
            "totalElements": 7
          },
          "links": {
            "self": "/comments?filter[confId]=1&filter[themeId]=1&filter[topId]=1&page[number]=1&page[size]=5",
            "first": "/comments?filter[confId]=1&filter[themeId]=1&filter[topId]=1&page[number]=1&page[size]=5",
            "next": "/comments?filter[confId]=1&filter[themeId]=1&filter[topId]=1&page[number]=2&page[size]=5",
            "last": "/comments?filter[confId]=1&filter[themeId]=1&filter[topId]=1&page[number]=2&page[size]=5"
          },
          "data": [
            {
              "id": 38,
              "created": "2017-06-05 17:39:27",
              "themeId": 1,
              "confId": 1,
              "topId": 1,
              "reUserId": 222,
              "replayId": 1,
              "content": "hello wwwww",
              "isAnonymous": 1,
              "praiseCount": 0,
              "_praiseCount": 0,
              "reUserNickName": "quzile0.763264954859064"
            },
            {
              "id": 37,
              "creator": 11,
              "created": "2017-06-05 17:39:18",
              "themeId": 1,
              "confId": 1,
              "topId": 1,
              "reUserId": 222,
              "replayId": 1,
              "content": "hello wwwww",
              "isAnonymous": 0,
              "praiseCount": 0,
              "_praiseCount": 0,
              "nickName": "e",
              "userAvatar": "http://static.budee.com/iyyren/image/201610/21/1452/132943086757232640.jpg",
              "reUserNickName": "quzile0.763264954859064"
            },
            {
              "id": 36,
              "creator": 11,
              "created": "2017-06-05 17:18:48",
              "themeId": 1,
              "confId": 1,
              "topId": 1,
              "reUserId": 222,
              "replayId": 1,
              "content": "hello wwwww",
              "isAnonymous": 0,
              "praiseCount": 0,
              "_praiseCount": 0,
              "nickName": "e",
              "userAvatar": "http://static.budee.com/iyyren/image/201610/21/1452/132943086757232640.jpg",
              "reUserNickName": "quzile0.763264954859064"
            },
            {
              "id": 35,
              "creator": 11,
              "created": "2017-06-05 17:17:30",
              "themeId": 1,
              "confId": 1,
              "topId": 1,
              "reUserId": 222,
              "replayId": 1,
              "content": "hello wwwww",
              "isAnonymous": 0,
              "praiseCount": 0,
              "_praiseCount": 0,
              "nickName": "e",
              "userAvatar": "http://static.budee.com/iyyren/image/201610/21/1452/132943086757232640.jpg",
              "reUserNickName": "quzile0.763264954859064"
            },
            {
              "id": 16,
              "creator": 1031,
              "created": "2017-05-31 14:28:20",
              "themeId": 1,
              "confId": 1,
              "topId": 1,
              "reUserId": 1030,
              "replayId": 14,
              "content": "5.31 5.31 5.31",
              "isAnonymous": 0,
              "praiseCount": 0,
              "_praiseCount": 1,
              "currentScore": -1,
              "nickName": "张永伟",
              "userAvatar": "http://static.budee.com/o2o/image/201605/25/1303/78919913401630720.jpg",
              "reUserNickName": "赵洪阳"
            }
          ]
        }
        
## (DELETE)评论 [/comments/{id}]

+ Description
    + [MUST] Authenticated
    + [MUST] 用户只能操作自己的资源

+ Parameters
    + id (long) - 评论ID

### 删除地址信息 [DELETE]

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
        
## (GET)评论标签集合 [/commentConfs/{id}]

+ Description
    + 

+ Parameters
    + id (long) - 

+ Response 200 (application/json)

        {
            "data": {
                "id": 1,
                "confName": "产品详情评论配置",
                "tags": [
                    {
                        "id": 1,
                        "tagName": "质量不错"
                    },
                    {
                        "id": 2,
                        "tagName": "222222222"
                    }
                ]
            }
        }



## 日志模块
 
 ## 自定义header
 >头的名称暂为ssid，值类型为String，长度最大100，长度不固定。
 
     {
         "X-User-ssid" : "dghpsdhipoiwtehgdfsgfasjdsklgjs"
     }
     
## 日志 [/eventLogs]
  
  + Data
    + source (String) - 来源
    + sourceTitle (String) - 来源标题
    + eventCode (String) - 自定义编码，定义在event_dic中
    + urlLog (String) - 请求的url
    + methodType (String) - 请求类型
    + params (json) - 参数，json格式的字符串
    + isSuccess (int) - 是否成功

### 增加日志 [POST]

   + Request (application/json)

    {
        "data": {
            "source": "http://www.mifanfan.cn/",
            "sourceTitle": "米饭-首页",
            "eventCode": "open_page",
            "urlLog": "/topics/1",
            "methodType": "get",
            "params": "{}",
            "isSuccess": 1
        }
    }

   + Response 201 (application/json)

        + Headers

            Location: /eventLogs/1

        + Body

            {
                "data": {
                    "id": 1,
                    "type": "eventLogs"
                }
            }
            
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
    
### 埋点位置
    
   + 进入、关闭产品、文章、评测、视频详情的操作
   + 进入、关闭图库详情操作
   + 进入、关闭品牌详情的操作
   + 进入、关闭分类详情的操作
   + 频道点击、收藏、取消收藏操作
   + 不喜欢操作
   + 刷新频道列表操作 
   + 搜索关键字
