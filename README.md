FORMAT: 1A
HOST: http://192.168.1.228

# web-crawler

音乐人产品搜索库 [产品搜索](http://192.168.1.228)

## update

+ 2016年8月19日
    + 增加facet
    + /product/profile/_search API 增加排序参数[s] sort

+ 2016年8月18日
    + 关于[Deep Pagination]问题，有两种解决方案, 第一种修改索引setting, 将max_result_window数变大(性能消耗会成指数级增长); 第二种使用scroll API(只能一页一页进行翻页), 两种方案都有限制，建议搜索引擎只返回前n条数据, https://www.elastic.co/guide/en/elasticsearch/guide/current/_fetch_phase.html, https://www.elastic.co/guide/en/elasticsearch/guide/current/pagination.html
    + 搜索API增加按来源条件[from]过滤的功能, 参数c增加了第6位来表示网站来源的ID

+ 2016年8月17日
    + 增加颜色的接口
    + 增加价格聚合
    + 增加来源聚合
    + 聚合类的接口进行了整合, 并且返回的格式进行了稍微的调整, 请查看/product/profile/_search/aggregation
    + 对聚合数据增加缓存
    + 返回的搜索数据列表中增加了关联来源的对象信息(from), 具体查看/product/profile/_search

+ 2016年8月16日
    + 修改了products表结构, 重新建立的索引, 搜索结果中增加产盘来源信息
    + 搜索中的通过聚合参数查询出来的结果增加价格range的统计信息

+ 2016年8月15日
    + 修复了URL CRC越界的一个bug
    + 搜索增加了产品所属网站的字段（from）, 目前可以根据网站来源进行搜索, 索引中保存的是ID
    + MORE LIKE THIS 现在如果是根据docId搜索, 会过滤掉docId本身的文档; docId修改为String类型

+ 2016年8月12日
    + 增加了聚合搜索功能
    + 增加查找产品分类[category], 产品品牌[brand]的API
    + 调整了搜索API, 修复了价格查找的bug, delimiter符号变化
    + MORE LIKE THIS API 增加了按doc id查找详细文档的参数


## 产品详情 [/product/profile/{doc_id}]

+ Response
    + hits.total - 记录总数
    + hits.hits[]._source.category[] - 产品分类
    + hits.hits[]._source.brand - 品牌
    + hits.hits[]._source.tags[] - （保留）标签
    + hits.hits[]._source.name - 产品名称
    + hits.hits[]._source.description - 产品描述
    + hits.hits[]._source.rawContent - 富文本内容
    + hits.hits[]._source.origin - 原始链接
    + hits.hits[]._source.priceUnit - 价格单位
    + hits.hits[]._source.price - 价格
    + hits.hits[]._source.seedId - 产品所属种子网站的ID
    + hits.hits[]._source.created - 抓取时间
    + hits.hits[]._source.images[] - 图片集合
    + hits.hits[]._source.images[].id - 图片标识符
    + hits.hits[]._source.images[].image - 本地图片链接
    + hits.hits[]._source.images[].width - 宽度
    + hits.hits[]._source.images[].height - 高度
    + hits.hits[]._source.images[].title - 图片标题
    + hits.hits[]._source.images[].alt - 图片alt

+ Parameters
    + doc_id (required, number) - 产品文档ID

### 查询产品详细信息 [GET]

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


## 产品搜索 [/product/profile/_search?q={q}&c={c}&s={s}&page={page}&size={size}&cluster={cluster}&aggregation={aggregation}]

+ Description
    + Condition [0_0_0_0_0_0] e.g: bass_0_0_50.0-500.0_FF0000_1, 查询关键词是bass价格在50.0-500.0之间的来源于ID=1的网站产品并按图片颜色[FF0000]红色降序排序, '-'为多选或区间, '_'为不同字段的delimiter符号
    + category:支持多选
    + brand:支持多选
    + tag:(保留字段)数据与category合并
    + price:格式一[50000.0-\*] 格式二[100.0-200.0] 格式三[\*-100.0]
    + color:单选 可用数据集合['000000','0000FF','00FF00','00FFFF','FF0000','FF00FF','FFFF00','FFFFFF']
    + from:来源网站的ID 支持多选

+ Response
    + hits.total - 记录总数
    + hits.hits[]._source.category[] - 产品分类
    + hits.hits[]._source.brand - 品牌
    + hits.hits[]._source.tags[] - （保留）标签
    + hits.hits[]._source.name - 产品名称
    + hits.hits[]._source.origin - 原始链接
    + hits.hits[]._source.priceUnit - 价格单位
    + hits.hits[]._source.price - 价格
    + hits.hits[]._source.seedId - 产品所属种子网站的ID
    + hits.hits[]._source.created - 抓取时间
    + hits.hits[]._source.images[] - 图片集合
    + hits.hits[]._source.images[].id - 图片标识符
    + hits.hits[]._source.images[].image - 本地图片链接
    + hits.hits[]._source.images[].width - 宽度
    + hits.hits[]._source.images[].height - 高度
    + hits.hits[]._source.images[].title - 图片标题
    + hits.hits[]._source.images[].alt - 图片alt
    + hits.hits[]._source.from.host - 网站主页
    + hits.hits[]._source.from.source - 网站简称
    + hits.hits[].highlight.content[] - 根据q选择相关度最高的n段句子, 并使用<span class='highlight'>{key}</span>包围关键词
    + aggregations.{name}.buckets - 根据搜索结果实时聚合的数据集合或实体对象
    + aggregations.{name}.buckets[].key - label名称, 会按key名称进行分组[A, B, C..., Z, #]
    + aggregations.{name}.buckets[].doc_count - count统计
    + clusters[] - 聚类数据
    + clusters[].documentReferences[] - 对搜索结果前n条记录聚类的文档ID集合
    + clusters[].label - 描述聚类的标签名称
    + clusters[].score - 保留
    + clusters[].subgroups - 保留
    + clusters[].otherTopics - 保留
    + clusters[].id - 保留
    + clusters[].phrases - 保留

+ Parameters
    + q (string) - Query 查询关键词 
    + c (string) - Condition 搜索条件[0_0_0_0_0_0], [category_brand_tag_price_color_from]
    + s (string) - 排序字段目前是排他的, 排序优先级比颜色高, 可选参数, 没有的话按关键词相关度排序. 格式:[price|created]-[0|1], e.g: price-0 (0:ASC, 1:DESC)
    + page (number) - 页码
    + size (number) - 大小
    + cluster (boolean) - 是非返回搜索结果聚类数据
    + aggregation (string) - 聚合条件[0_0_0_50], (0|1:关闭|打开 聚合搜索, 0|1:asc|desc, 0|1:term|count 排序字段, 50:返回聚合结果的size)

### 产品搜索 [GET]

+ Request (application/json)

        {
            "q": "bass",
            "c": "0_0_0_*-200.0_0_0"
	    "s": "price-0"
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
    

## 产品相似查询More Like This [/profile/_search/mlt?text={text}&docId={docId}&size={size}]

+ Parameters
    + text (string) - 根据该文本内容找相似文档(与docId二选一)
    + docId (number) - 根据指定的文档ID找相似文档(与text二选一)
    + size (number) - 显示相似文档的数量

### MLT [GET]

+ Request (application/json)

        {
            "text": "The Simmons Piezo Drum Trigger enables you to trigger electronic drum sounds",
            "docId": 1
            "size": 20
        }

+ Response 200 (application/json)

        {
          "hits": {
            "hits": [
              {
                "_source": {
                  "priceUnit": "$",
                  "images": [
                    {
                      "image": "http://static.budee.com/yyren/image/201608/12/11/B5/B1/4F/0E/AF/AA/7B/4B/DB/0D/98/53/E2/2B/15/11B5B14F0EAFAA7B4BDB0D9853E22B15.jpg",
                      "000000": 0.0015284483987619338,
                      "0000FF": 0.0016725935114987162,
                      "FF0000": 0.0017398052281613968,
                      "FF00FF": 0.002004197277862656,
                      "alt": "Simmons Piezo Drum Trigger",
                      "00FF00": 0.0018473284671215245,
                      "00FFFF": 0.002361876797803322,
                      "FFFF00": 0.0025789561502864537,
                      "FFFFFF": 0.008876127604525032,
                      "width": 120,
                      "id": 328592,
                      "height": 120
                    }
                  ],
                  "created": "2016-08-12T10:23:21.000Z",
                  "price": 19.99,
                  "origin": "http://www.music123.com/drums-percussion/simmons-piezo-drum-trigger",
                  "seedId": 1,
                  "name": "  Simmons Piezo Drum Trigger",
                  "category": [
                    "Drums & Percussion",
                    "Electronic Drums",
                    "Acoustic Triggers"
                  ],
                  "brand": "Simmons",
                  "tags": []
                },
                "_id": "59297",
                "_score": 4.1201496
              }
            ],
            "total": 3142
          },
          "aggregations": null
        }


## 产品聚合统计搜索 [/product/profile/_search/aggregation?type={type}]

+ Parameters
    + type (string) - [category|brand|price|from] 分类|品牌|价格|来源 可选参数,没有的话返回所有数据

### 聚合搜索 [GET]

+ Request (application/json)

        {
            "type": "price"
        }

+ Response 200 (application/json)

        {
          "hits": {
            "hits": [],
            "total": 97857
          },
          "aggregations": {
            "price": {
              "buckets": [
                {
                  "doc_count": 49346,
                  "from": "-Infinity",
                  "to": 100,
                  "key": "*-100.0"
                },
                {
                  "doc_count": 10829,
                  "from": 100,
                  "to": 200,
                  "key": "100.0-200.0"
                },
                {
                  "doc_count": 5994,
                  "from": 200,
                  "to": 300,
                  "key": "200.0-300.0"
                },
                {
                  "doc_count": 6605,
                  "from": 300,
                  "to": 500,
                  "key": "300.0-500.0"
                },
                {
                  "doc_count": 8267,
                  "from": 500,
                  "to": 1000,
                  "key": "500.0-1000.0"
                },
                {
                  "doc_count": 5124,
                  "from": 1000,
                  "to": 2000,
                  "key": "1000.0-2000.0"
                },
                {
                  "doc_count": 3918,
                  "from": 2000,
                  "to": 5000,
                  "key": "2000.0-5000.0"
                },
                {
                  "doc_count": 965,
                  "from": 5000,
                  "to": 10000,
                  "key": "5000.0-10000.0"
                },
                {
                  "doc_count": 78,
                  "from": 50000,
                  "to": "Infinity",
                  "key": "50000.0-*"
                }
              ]
            }
          }
        }


## 查询产品颜色列表 [/product/colors]

### 颜色列表 [GET]

+ Response 200 (application/json)

        [
          "000000",
          "0000FF",
          "00FF00",
          "00FFFF",
          "FF0000",
          "FF00FF",
          "FFFF00",
          "FFFFFF"
        ]
