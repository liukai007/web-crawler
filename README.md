FORMAT: 1A
HOST: http://192.168.1.228

# web-crawler

音乐人产品搜索库 [产品搜索](http://192.168.1.228)

## 产品 [/product/profile/{doc_id}]

+ hits.hits._source.priceUnit - 价格单位
+ hits.hits._source.price - 价格
+ hits.hits._source.images - 图片集合
+ hits.hits._source.category - 产品分类

+ Parameters
    + doc_id (required, number) - 产品文档ID

### 产品详细页 [GET]

+ Response 200 (application/json)

        {
          "hits": {
            "hits": [
              {
                "_source": {
                  "priceUnit": "CNY",
                  "images": [
                    {
                      "image": "http://static.budee.com/yyren/image/201608/10/C5/42/00/F6/87/FE/D9/0F/D5/DA/8F/6A/E8/E4/98/2E/C54200F687FED90FD5DA8F6AE8E4982E.jpg",
                      "000000": 0.0015232958813356824,
                      "0000FF": 0.0016691663195955856,
                      "FF0000": 0.0017360185242277426,
                      "FF00FF": 0.002001311678124089,
                      "alt": "Simmons Piezo Drum Trigger",
                      "00FF00": 0.0018438470871000882,
                      "00FFFF": 0.002360421052461741,
                      "FFFF00": 0.0025782791325445493,
                      "FFFFFF": 0.009053163686147867,
                      "width": 180,
                      "id": 1,
                      "height": 180
                    }
                  ],
                  "created": "2016-08-10T04:41:12.000Z",
                  "price": 133.19,
                  "origin": "http://www.guitarcenter.com/Simmons/Piezo-Drum-Trigger.gc",
                  "seedId": 3,
                  "name": "Simmons Piezo Drum Trigger  ",
                  "description": null,
                  "category": [
                    "Drums & Percussion",
                    "Electronic Drums",
                    "Acoustic Triggers"
                  ],
                  "brand": null,
                  "tags": [],
                  "rawContent": "<div>  \n <div>\n   Overview \n </div> \n <div>  \n  <p>The Simmons Piezo Drum Trigger enables you to trigger electronic drum sounds, effects, and loops with an acoustic drum. It easily mounts to any drumhead to trigger any electronic drum module. A trigger hat and insulated rim jack mount clip come included. (Electronic drum module not included)<br></p> \n </div>  \n</div>"
                },
                "_id": "1",
                "_score": 1
              }
            ],
            "total": 1
          },
          "aggregations": null
        }

## 搜索 [/profile/_search]

### Create a New Question [POST]

You may create your own question using this action. It takes a JSON
object containing a question and a collection of answers in the
form of choices.

+ Request (application/json)

        {
            "question": "Favourite programming language?",
            "choices": [
                "Swift",
                "Python",
                "Objective-C",
                "Ruby"
            ]
        }

+ Response 201 (application/json)

    + Headers

            Location: /questions/2

    + Body

            {
                "question": "Favourite programming language?",
                "published_at": "2015-08-05T08:40:51.620Z",
                "choices": [
                    {
                        "choice": "Swift",
                        "votes": 0
                    }, {
                        "choice": "Python",
                        "votes": 0
                    }, {
                        "choice": "Objective-C",
                        "votes": 0
                    }, {
                        "choice": "Ruby",
                        "votes": 0
                    }
                ]
            }
