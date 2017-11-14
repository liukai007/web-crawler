## 审核精翻

+ Data
    + remark (String) - 审核说明
    + state (int) - 状态 1:草稿箱, 2:待审核, 3:审核通过, 4:审核失败, 5:被覆盖, 9:已删除
    + riceNum (int) - 奖励的米粒个数


### 待审核列表 [GET] /article/moderations?filter[state]=2

+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_AUDITOR

+ Parameters
    + state
    

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

### 审核 [PATCH] /article/moderations/audit/{id}

+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_AUDITOR

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

### 精翻详情 [GET] /article/humanTranslates/{id}

+ Description
    + [MUST] Authenticated
    + 只有本人、管理员、审核员可以查看
    

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
