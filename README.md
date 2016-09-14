FORMAT: 1A
HOST: http://192.168.1.228

# web-crawler

音乐人产品搜索库 [产品搜索](http://192.168.1.228)

## update
+ 2016年9月18日
    + 增加 retry 配置参数

+ 2016年9月14日
    + 爬虫架构重构（Fetcher模块、Parser模块、Image模块、SFTP上传模块、Filter等） 
    + 加入图片排重功能，相同的图片只下载一次
    + 多线程同步模式改为单线程异步模式
    + 在爬取网页内容时，增加了Retry功能，保证数据完整性。
    + [BUG FIXED] 修改了一个资源竞争导致的无限等待的bug
    + 爬虫客户端重构
    + ElasticSearch 产品mapping结构变更, 提供更丰富的facet查询。

+ 2016年9月9日
    + [BUG FIXED] 修改了URI中特殊字符导致的链接处理失败的问题
    + 产品搜索的[brand]数据结构变化, [brand]聚合增加logo字段
    + 分类减少了, 现在只返回第一级分类数据

+ 2016年9月8日
    + 优化了过滤器链的处理
    + 增加了对深度优先策略的支持，避免了广度优先策略导致URL队列过度膨胀与元数据长久占据内存的问题

+ 2016年9月7日
    + 索引mapping修改（brand由字符串类型变为一个Object类型）
    + Image调度任务修改, ImageDownloadHandler的流程优化

+ 2016年9月6日
    + 修改了爬虫种子表的数据结构
    + 修改了LinkFilter链接过滤器接口, 增加了一个参数来对链接类型进行分组
    + 修改了Brands表结构
    + [BUG FIXED] 修复了一个由于类加载器类型不一致导致instanceof比较失败的问题

+ 2016年9月5日
    + 爬虫配置增加OBJECT枚举类型用以处理动态数据属性的网页结构，并增加了新的元数据将其输出为JSON格式
    + 增加了对爬取网页内容中产品特性内容结构的的配置参数
    + Node增加了转移的特性, 控制Node配置是否参与到抓取的网页的下一页的处理流程中
    + 增加了对网页内容数据一对多关系的处理，品牌信息增加了logo字段

+ 2016年9月2日
    + 爬虫系统重构Node数据结构，删除了冗余的属性
    + 爬虫系统增加了元数据
    + 删除了NodeType, 现在通过元数据进行逻辑判断
    + 修改了持久化接口，增加了一些参数。BeanWrapper替换为DataBinder了
    + 抓取配置中可以配置表达式了，以完成更复杂的网页抓取规则

+ 2016年9月1日
    + 图片下载器现在优先处理尺寸最清晰的图片，并根据THUMBNAIL,SMALL,MEDIUM,LARGE进行压缩处理
    + 图片队列增加了等待通知模式，避免队列过大消耗太多内存。
    + 抓取网页产品相关的图片由线性结构改为树形结构，用以图片分组与分类。产品索引关联的图片减少，只对所关心的分组图片建立索引。
    + 修改了产品索引mapping
    + 网页中的image图片（img标签的）和链接图片（a标签的）现在合并了，通过图片模式字段model加以区分
    + 修改了在抓取某些网站分页内容时的爬虫黑洞问题

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
                  "brand": {
                    "name": "ADG Productions",
                    "logo": "http://static.budee.com/yyren/image/201609/08/EA/D9/C2/74/9A/08/91/17/C6/99/AC/0E/E5/7C/05/B1/EAD9C2749A089117C699AC0EE57C05B1.jpg"
                  },
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
                  "brand": {
                    "name": "ADG Productions",
                    "logo": "http://static.budee.com/yyren/image/201609/08/EA/D9/C2/74/9A/08/91/17/C6/99/AC/0E/E5/7C/05/B1/EAD9C2749A089117C699AC0EE57C05B1.jpg"
                  },
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
                  "logo":"http://static.budee.com/yyren/image/201609/08/EA/D9/C2/74/9A/08/91/17/C6/99/AC/0E/E5/7C/05/B1/EAD9C2749A089117C699AC0EE57C05B1.jpg",
                  "key": "Hal Leonard"
                },
                {
                  "doc_count": 549,
                  "logo":"http://static.budee.com/yyren/image/201609/08/EA/D9/C2/74/9A/08/91/17/C6/99/AC/0E/E5/7C/05/B1/EAD9C2749A089117C699AC0EE57C05B1.jpg",
                  "key": "Gator"
                },
                {
                  "doc_count": 59,
                  "logo":"http://static.budee.com/yyren/image/201609/08/EA/D9/C2/74/9A/08/91/17/C6/99/AC/0E/E5/7C/05/B1/EAD9C2749A089117C699AC0EE57C05B1.jpg",
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
                  "brand": {
                    "name": "ADG Productions",
                    "logo": "http://static.budee.com/yyren/image/201609/08/EA/D9/C2/74/9A/08/91/17/C6/99/AC/0E/E5/7C/05/B1/EAD9C2749A089117C699AC0EE57C05B1.jpg"
                  },
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
