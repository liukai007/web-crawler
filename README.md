FORMAT: 1A
HOST: http://192.168.1.228

# web-crawler

音乐人产品搜索库 [产品搜索](http://192.168.1.228)

## 产品详细 [/product/profile/{doc_id}]

+ hits.hits._source.priceUnit - 价格单位
+ hits.hits._source.price - 价格
+ hits.hits._source.category - 产品分类
+ hits.hits._source.brand - 品牌
+ hits.hits._source.tags - （保留）标签
+ hits.hits._source.name - 产品名称
+ hits.hits._source.description - 产品描述
+ hits.hits._source.rawContent - 富文本内容
+ hits.hits._source.origin - 原始链接
+ hits.hits._source.created - 抓取时间
+ hits.hits._source.images - 图片集合dddd

+ Parameters
    + doc_id (required, number) - 产品文档ID

### 产品详细 [GET]

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

## 产品搜索 [/profile/_search]

### 产品搜索 [GET]

+ Request (application/json)

        {
            "q": "bass",
            "c": "0_0_0_*-200.0_0"
            "page": 1,
            "size": 20,
            "cluster": "false",
            "aggregation": "0_0_0_50"
        }

+ Response 200 (application/json)

        {
          "hits": {
            "hits": [
              {
                "highlight": {
                  "content": [
                    "Overview Pops' <span class='highlight'>Bass</span> Rosin is packaged in a convenient resealable red tub."
                  ]
                },
                "_source": {
                  "priceUnit": "CNY",
                  "images": [
                    {
                      "image": "http://static.budee.com/yyren/image/201608/10/32/F9/E4/53/A0/6A/3E/8B/5A/F7/9A/4A/EB/54/0D/12/32F9E453A06A3E8B5AF79A4AEB540D12.jpg",
                      "000000": 0.001625050938040724,
                      "0000FF": 0.0017121483481907281,
                      "FF0000": 0.002108896256451791,
                      "FF00FF": 0.002219442528339659,
                      "alt": "Pops' Bass Rosin Bass Rosin",
                      "00FF00": 0.0018475207605581057,
                      "00FFFF": 0.002146904123589382,
                      "FFFF00": 0.0026933843973544424,
                      "FFFFFF": 0.005582458482860849,
                      "width": 180,
                      "id": 1389,
                      "height": 180
                    }
                  ],
                  "created": "2016-08-10T04:53:06.000Z",
                  "price": 79.89,
                  "origin": "http://www.guitarcenter.com/Pops-Bass-Rosin/Bass-Rosin.gc",
                  "seedId": 3,
                  "name": "Pops' Bass Rosin Bass Rosin  ",
                  "from": {
                    "host": "http://www.guitarcenter.com",
                    "source": "guitarcenter"
                  },
                  "category": [
                    "Orchestral Strings",
                    "Accessories for Orchestral Strings",
                    "Bows & Rosin",
                    "Rosin",
                    "Double Bass Rosin"
                  ],
                  "brand": null,
                  "tags": []
                },
                "_id": "1200",
                "_score": 2.2920809
              }
            ],
            "total": 16794
          },
          "aggregations": {
            "price": {
              "buckets": [
                {
                  "doc_count": 5898,
                  "from": "-Infinity",
                  "to": 100,
                  "key": "*-100.0"
                },
                {
                  "doc_count": 2138,
                  "from": 100,
                  "to": 200,
                  "key": "100.0-200.0"
                }
              ]
            },
            "category": {
              "buckets": [
                {
                  "doc_count": 3692,
                  "key": "Accessories"
                },
                {
                  "doc_count": 2344,
                  "key": "Guitars"
                }
              ]
            },
            "brand": {
              "buckets": [
                {
                  "doc_count": 702,
                  "key": "Hal Leonard"
                },
                {
                  "doc_count": 549,
                  "key": "Gator"
                },
                {
                  "doc_count": 59,
                  "key": "Epiphone"
                }
              ]
            }
          }
        }
        
## 产品相似查询(More Like This) [/product/profile/_search/mlt]

### 产品搜索 [GET]

## 产品聚合 [/product/profile/_search/aggregation]

### 产品搜索 [GET]

## 产品颜色集合 [/product/colors]

### 产品搜索 [GET]
