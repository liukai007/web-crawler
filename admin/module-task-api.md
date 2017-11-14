## 管理员接口
### 增加任务 [POST] /article/translateTasks
+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN

+ Parameters
    + topicId - 必填
    + wordsNum
    + bonus

+ 新增Request (application/json)
    
        {
            "data":{
                "topicId":1,
                "wordsNum":1000,
                "bonus":50
            }
        }
+ Response 201 (application/json)

        {
            "data": {
                "id": 1,
                "type": "translateTasks"
            }
        }
### 修改任务 [PATCH] /article/translateTasks/{id}

+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN

+ Parameters
    + state
    + wordsNum
    + wordsNumCn
    + bonus

+ 修改Request (application/json)
    
        {
            "data":{
                "wordsNum":2000,
                "wordsNumCn":2500,
                "bonus":100
            }
        }
+ Response 200 (application/json)

### 删除任务 [DELETE] /article/translateTasks/{id}

+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN
    + [MUST] 只有state=1的才能被删除

+ Response 204 (application/json)
+ Response 400 (application/json)

### 任务集合 [GET] [/article/translateTasks?page[number]=1&page[size]=5&filter[state]=1&filter[enabled]=1

+ Parameters
    + filter[enabled] - 筛选条件，是否可用
    + filter[state] - 筛选条件，任务状态

+ Response 200 (application/json)

        {
          "meta": {
            "totalPages": 1,
            "totalElements": 2,
            "size": 5,
            "number": 1,
            "numberOfElements": 2,
            "first": true,
            "last": true,
            "sort": null
          },
          "links": {
            "self": "/article/translateTask?filter[state]=1&filter[enabled]=1&page[number]=1&page[size]=5",
            "first": "/article/translateTask?filter[state]=1&filter[enabled]=1&page[number]=1&page[size]=5",
            "last": "/article/translateTask?filter[state]=1&filter[enabled]=1&page[number]=1&page[size]=5"
          },
          "data": [
            {
              "id": 1,
              "enabled": 1,
              "creator": 1031,
              "modifier": 1031,
              "created": "2017-11-07 10:41:56",
              "modified": "2017-11-07 11:09:06",
              "topicId": 1,
              "postId": 0,
              "state": 1,
              "wordsNum": 2000,
              "bonus": 100,
              "translator": 0,
              "auditor": 0,
              "title": "Virus TI2 Desktop",
              "topicType": "产品"
            },
            {
              "id": 2,
              "enabled": 1,
              "creator": 1031,
              "modifier": 1031,
              "created": "2017-11-07 11:07:30",
              "modified": "2017-11-07 11:07:30",
              "topicId": 2,
              "postId": 0,
              "state": 1,
              "wordsNum": 500,
              "bonus": 25,
              "translator": 0,
              "auditor": 0,
              "title": "Virus TI2 Keyboard",
              "topicType": "产品"
            }
          ]
        }
### 任务详情 [GET] [/article/translateTasks/{id}]
+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN

+ Response 200 (application/json)   

        {
          "data": {
            "id": 1,
            "enabled": 1,
            "creator": 1031,
            "modifier": 1031,
            "created": "2017-11-07 10:41:56",
            "modified": "2017-11-07 15:09:33",
            "topicId": 1,
            "postId": 1013367,
            "state": 2,
            "wordsNum": 2000,
            "bonus": 100,
            "translator": 1031,
            "auditor": 0,
            "post": {
              "id": 1013367,
              "enabled": 0,
              "creator": 1031,
              "modifier": 1031,
              "created": "2017-11-07 15:03:23",
              "modified": "2017-11-07 15:09:33",
              "parentId": 1,
              "topicId": 1,
              "priority": 0,
              "language": 2,
              "categories": [],
              "tags": [],
              "features": [
                {
                  "_name": "属性2",
                  "_value": "属性2的值"
                }
              ],
              "title": "fan'yi'ren'wu翻译任务222222222 ",
              "description": "fan'yi翻译 miao'shu描述2222222 ",
              "content": "wo'shi'fan'yi'nei'rong我是翻译内容2222222",
              "parent": {
                "id": 1,
                "enabled": 1,
                "creator": 0,
                "modifier": 0,
                "created": "2016-10-12 09:55:37",
                "modified": "2017-10-28 13:33:38",
                "parentId": 0,
                "topicId": 1,
                "priority": 0,
                "language": 2,
                "categories": [
                  "Keyboards & Synthesizers",
                  "Synthesizers"
                ],
                "tags": [
                  "VirusTI2Desk"
                ],
                "features": [
                  {
                    "_name": "Sound Engine Type(s)",
                    "_value": "Analog Modeling"
                  }
                ],
                "title": "Virus TI2 Desktop",
                "description": "Analog Modeling Desktop Synthesizer and 24-bit/192kHz Audio/MIDI Interface",
                "content": "<div> \n <h2>Access Steps Up The Renowned Virus!</h2> \n ",
                "postsText": {
                  "id": 1,
                  "category": "Keyboards & Synthesizers,Synthesizers",
                  "tag": "VirusTI2Desk",
                  "title": "Virus TI2 Desktop",
                  "feature": "[{\"_name\":\"Sound Engine Type(s)\",\"_value\":\"Analog Modeling\"}]",
                  "description": "Analog Modeling Desktop Synthesizer and 24-bit/192kHz Audio/MIDI Interface",
                  "content": "<div> \n <h2>Access Steps Up The Renowned Virus!</h2> \n 
                },
                "postType": 1,
                "postTypeValue": "爬取"
              },
              "postsText": {
                "id": 1013367,
                "category": "",
                "tag": "",
                "title": "fan'yi'ren'wu翻译任务222222222 ",
                "feature": "[{\"_name\":\"属性2\",\"_value\":\"属性2的值\"}]",
                "description": "fan'yi翻译 miao'shu描述2222222 ",
                "content": "wo'shi'fan'yi'nei'rong我是翻译内容2222222"
              },
              "postType": 3,
              "postTypeValue": "精翻"
            },
            "title": "Virus TI2 Desktop",
            "topicType": "产品"
          }
        }