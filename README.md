FORMAT: 1A
HOST: http://192.168.1.228

# web-crawler

音乐人产品搜索库 [产品搜索](http://192.168.1.228)

## update

+ 基于Mahout的产品分类
    + 第一阶段：将原始数据转换为可分类数据的预处理过程
        + 检查产品数据（确定目标变量：16个产品大类别，['Guitars and Basses','Drums + Percussion', ...]）
        + 产品库可能为空的字段：category(99.71%), brand(99.65%), tag(?), description(), content(99.87), price(99.50%), price_unit, rating(84.75), feature()
        + 产品库不可能为空的字段：seed_id, origin, name, time
        + 保留字段：origin, name, brand, description, content, content-length, price, rating, feature
    + 第二阶段：将可分类数据转化为向量（连续型、类别型、单词型、文本型）
    
+ 2017年1月
    + 流程自动化：爬取数据 -> 自动分类（包括垃圾数据过滤）-> 自动判断重复 -> 建立索引
        + 分类：先执行分类任务进行产品一级分类划分与垃圾数据过滤
        + 排重：基于过滤后的结果再进行产品排重与合并
        + 索引：对排重后的数据再进行索引
    + 垃圾数据过滤（建立索引时，特定分类不予索引）
    + 搜索把最相关的排在最前面
    + 产品合并新API
    + 优化一级分类与其他分类
    + 对比时的参数过滤
    + 对比接口结构变化
        + 现在按参数顺序返回对比结果集
        + 增加价格对比
        + 增加一个属性 来判断是否都相同
    + 考虑产品数据动态合并解决方案
    
+ 2017年2月13日 （产品索引中文翻译后的相关改动）
    + [category, descriptions, contents, features]增加多语言数据, 结构变化, 相关API如下
    + 产品详情API[response]变化 [/product/profile/{doc_id}]
        + 原[description, rawContent]名称改动为[descriptions, contents]. (与brand字段结构不一致导致的命名冲突)
        + 部分字段增加中英文用于数据检索，改动字段: [category, descriptions, contents, features]
        + 增加数据合并后的相关统计对象*:info, 包括平均值、最小值、最大值等(price:info, rating:info)
    + 产品搜索API[response]变化 [/product/profile/_search]
        + 原[description, content]名称改动为[descriptions, contents]
        + 部分字段增加中英文用于数据检索，改动字段: [category, descriptions] (contents, features不返回)
        + highlight路径变化, 原[items.content]变更为[items.contents.en, items.contents.cn]. 根据不同的关键词可能会都出现, 也可能只出现其中一个
    + 产品相似查询API[response]变化 [/profile/_search/mlt] (More Like This).
        + 原[description, content]名称改动为[descriptions, contents].
        + 部分字段增加中英文用于数据检索，改动字段: [category, descriptions].
    + 产品参数对比API[response]变化 [/product/compare?art[]={ids}]
        + 原[description, content]名称改动为[descriptions, contents]
        + 部分字段增加中英文用于数据检索，改动字段: [category, descriptions, features]
        + 上述变化只在header中, 对body无变化

+ 2017年1月9日
    + 修改/product/summary索引结构，增加feature数组，并重建索引数据。
    + 对比API现在查询summary，不再查询profile数据。
     
+ 2017年1月6日
    + 修改对比API返回的参数数据结构，并增加价格对比数据等。
    + 修改elasticsearch索引分类错误
    
+ 2016年12月
    + [OK] 首页接口: 热词接口, 记录与查找(简单实现, 并按没7天的搜索词的频度进行排序)
    + [OK] 记录搜索关键词
    + [OK] 增加记录词频的数据库表(先使用缓存记录)
    + [OK] 首页接口: 热度商品API
    + [OK] 首页接口: 新品商品API
    + [OK] 首页接口: 最近浏览商品接口（用户）产品浏览记录
    + [OK] 首页轮播图 API
    + [OK] 品牌接口的图片显示的是产品图而不是品牌图
    + [OK] 整合分类分析数据（刘凯）（数据已经整合）
    + [OK] 轮播图/links?keys=type&values=1&properties=sort&asc=true&fields=id,title,description,image,href
    + [OK] 导航栏/links?keys=type&values=2&properties=sort&asc=true&fields=id,title,description,image,href
    + 统一所有返回的列表数据只限定在两种格式内:一种是搜索引擎的hits格式, 另一个是PageImpl格式（包括从cache中查找, 数据库查找等, 都要封装成这种格式）.

+ 2016年12月30日
    + 修改MLT BUG
    + [TASK]考虑自动分类任务与排重任务与线上数据的整合问题
    + 增加产品对比相关API实现
    
+ 2016年12月29日
    + 和列表相关的部分API变化，如下：
    + 搜索列表API结构变化 （/product/profile/_search）
        + a. 【参数】价格排序路径变化了：由price-0 变化为 items.price-0
        + b. 【response】highlight路径变化了：由 content 变化为 items.content
        + c. 【response】部分字段移动到items[]属性下了 （seedId，origin，from，price，priceUnit，description，rating）
    + More Like This API 相似产品搜索列表结构同上 （/product/profile/_search/mlt）
    + 搜索详细API结构无变化，但是会返回多条数据。
        + 第1条为主要产品数据，第2至n条为相同产品来自不同网站的其他数据。（产品ID e.g : 130、872、874）
    + 其他API结构不变
    
+ 2016年12月28日
    + 因为产品排重需求, 搜索列表API结构变化. [seedId, origin, description, price, priceUnit, rating]字段移动到items属性下
    + 产品详细API结构没有变化，但是现在返回的数据可能有多条（相同产品的数据）
    
+ 2016年12月27日
    + 实现Elastic Search索引排重功能相关代码
    
+ 2016年12月26日
    + 索引结构调整，采用数组保存相似的产品集合；相关API也需要调整，搜索路径变动。(https://www.elastic.co/guide/en/elasticsearch/reference/2.3/array.html)
    + [OK]360度图片加入索引中。
    + [brand]品牌合并 [category]类别合并 [name]名称合并 删除:image.size image.model image.enabled
    + 方案1: top hit aggregation(无法分页)（https://www.elastic.co/guide/en/elasticsearch/reference/2.3/search-aggregations-metrics-top-hits-aggregation.html）
    + 方案2: 双mapping, 一个用于列表 一个用于详情
    + 方案3: 产品详细使用top hit aggregation查询相似产品数组
    + 方案4: 详情: 扩展API返回相似产品集合, 数据结构不变
    + 方案5: 详情使用现有API, 条件中根据URL传递的id查询相同产品id集合，在条件查询elasticsearch
    
+ 2016年12月22日
    + 修改thomann爬虫模板，生产环境开始抓取360度图片，thomann数据全量更新；
    + rest接口安全性问题考虑，
    
+ 2016年12月20日
    + thomann360度图片抓取，需要修改爬虫项目来处理JSON格式数据，需要增加一个link queue来延迟处理特定的LINK；
    + 爬虫系统修改初期设计，主要看在哪里处理节点加入逻辑比较合适；
    
+ 2016年12月15日
    + 产品中文翻译与中文分词
    + 数据排重与整合, Elastic搜索引擎产品索引调整
    + 其他多媒体格式抓取：360度图片抓取；视频抓取；
    + 略 淘宝API（只有天猫商家发布商品时才需要用到，并非商品搜索api，当前暂不提供商品搜索api。）
    + 略 京东API（不支持JD自营店）
     
+ 2016年12月2日
    + [FIXED] 修改品牌图片查询时关联错误的BUG

+ 2016年12月1日
    + [OK] 增加记录搜索关键词的API，redis 数据库
    + [OK] 增加推荐关键词的API

+ 2016年11月
    + [OK] 增加分类列表API （名称，图片）
    + [OK] 增加品牌列表API （名称，图片）
    + [OK] mahout : 系统过滤推荐（user based, item based）
    + [NEW] mahout : 聚类
    + [OK] 100% mahout : 分类
    + [OK] 修改 fetcher 时间
    + [NEW] 增加产品比较（Compare）API
    + [OK] 建立索引时增加了货币的汇率转换
    + [OK] 品牌详细信息抓取
    + [FIXED] upload图片失败时需要提供一个票据(Future)以告知下载器
    + [OK] 写一个图片校验器, 来检查enabled = 1但是没有上传成功的图片
    + [OK] 增加品牌聚合API
    + [FIXED] 搜索接口BUG修改
    
+ 2016年11月17日
    + 增加【查找某个品牌下的子分类】API

+ 2016年11月2日
    + [FIXED] 建立索引时没有图片的品牌被忽略掉了， 需要修改下

+ 2016年10月31日
    + 修改thomann模板, 增加了对后补图片的抓取 （当产品没有图片列表时, 抓取其他元素中的图片作为产品图）
    + [FIXED] 修改下载图片时流没有关闭导致的内存溢出问题
    
+ 2016年10月28日
    + [FIXED] images数据一致性问题, image字段
    + [BUG] 断网时visited redis数据需要重置
    + 生产环境部署

+ 2016年10月24日
    + 增加了对AJAX请求的处理逻辑，如产品图片链接的AJAX加载。

+ 2016年10月20日
    + [FIXED] 修复了异步HTTP（New I/O worker）与信号量（Semaphore）资源竞争导致的死锁问题

+ 2016年10月17日
    + [FIXED]修复错误图片链接数据记录
    + 修改模板script部分，增加判断图片链接的前缀是否包含/^http(s)/gi的逻辑
    + 爬虫架构增加whenEmpty流程来处理特殊情况：当没有找到图片列表时，采用备用元素节点作为产品图片。（iterate逻辑修改）

+ 2016年10月13日
    + [FIXED]修改了图片不能正常分组的问题，该问题会导致产品关联的图片中出现富文本中包含的图片
    + [FIXED]UnknownHostException:该异常将使调度程序不能从异常中恢复
    + 先对Element中图片进行的排重处理，然后再进行数据库级的排重
    + 更新图片时, 会同时更新对引用此图片的所有产品的最后更新时间，用以告诉搜索引擎重建索引来加入新的图片
    + 修改了甜水模板，部分产品找不到图片

+ 2016年10月12日
    + [BUG]不同二级域名相同图片暂时没有进行排重，需要修改模板（link中使用replace将随机域名替换为固定）
    + 修改产品搜索, 产品详细API文档
    + visited表废除, 现在只采用redis进行已访问的url验证, 清除redis缓存将全量更新产品数据
    + [FIXED]修改了RedisFilterAdapter的实现, 现在不会产生URL MD5碰撞问题了
    + 产品搜索详情加入来源数据 from

+ 2016年10月11日
    + 图片下载采用更新策略并对相同地址的图片进行的排重，避免在更新产品信息时重新下载已有的图片
    + 产品索引增加【rating】属性（产品评级）
    + 产品索引增加【features】属性（产品特性）
    + 产品mapping修改，增加动态模板来处理【颜色距离】属性集合，并将集合提取到images属性的嵌套属性中

+ 2016年10月10日
    + 图片生成策略修改，避免原先MD5碰撞问题
    + 使用延迟生成图片缩略图的策略
    + 增加价格浮动记录表
    + 现在爬虫支持对相同URL的修改功能了
    + 创建索引时增加了图片sizes属性

+ 2016年9月23日
    + 所有主题网站模板的优化，改为采用builder方式构建，层次更加清晰
    + 数据重新爬取

+ 2016年9月20日
    + 增加了在处理elements前对element的过滤功能，用来过滤掉对指定选择器选择的元素
    + 现在可以采用建造模式构建网站mapping，简化构建流程。
    + 增加了新网站的mapping

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
    
## 产品参数对比接口 [/product/compare?art={ids}]

+ Response
    + header - 头部信息, 包含产品详细的信息
    + body.features - 产品参数
    + body.prices - 产品价格
    
+ Parameters
    + ids (integer) - 数组, 待对比的产品ID集合 e.g : 1,2,3

### 查找产品对比信息 [GET]

+ Response 200 (application/json)

        {
            "header": {
                "models": [
                    {
                        "_source": {
                            "features": [
                                {
                                    "en": {
                                        "_name": "Channels",
                                        "_value": "2 + 2 Channel"
                                    },
                                    "cn": {
                                        "_name": "频道",
                                        "_value": "2 + 2 Channel"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Analog Inputs",
                                        "_value": "4 x stereo RCA line, 1 x XLR"
                                    },
                                    "cn": {
                                        "_name": "模拟输入",
                                        "_value": "4 x stereo RCA line, 1 x XLR"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Analog Outputs",
                                        "_value": "2 x XLR, 2 x stereo RCA (Monitor, Record)"
                                    },
                                    "cn": {
                                        "_name": "模拟输出",
                                        "_value": "2 x XLR, 2 x stereo RCA (Monitor, Record)"
                                    }
                                }
                            ],
                            "images": [
                                {
                                    "image": "http://static.budee.com/yyren/image/6/1751.jpg",
                                    "width": 1385,
                                    "id": 1751,
                                    "height": 1800
                                },
                                {
                                    "image": "http://static.budee.com/yyren/image/6/1752.jpg",
                                    "width": 1600,
                                    "id": 1752,
                                    "height": 724
                                }
                            ],
                            "created": "2016-10-12T01:55:39.000Z",
                            "name": "Allen & Heath Xone:23",
                            "category": [
                                {
                                    "en": "DJ Equipment",
                                    "cn": null
                                },
                                {
                                    "en": "Mixer",
                                    "cn": null
                                },
                                {
                                    "en": "DJ Mixers",
                                    "cn": null
                                }
                            ],
                            "brand": {
                                "reviews": 1861,
                                "top": true,
                                "name": "Allen & Heath",
                                "rating": 0.93,
                                "logo": "http://static.budee.com/yyren/image/220/14/974003.jpg"
                            },
                            "items": [
                                {
                                    "priceUnit": "$",
                                    "price": 299,
                                    "seedId": 2,
                                    "origin": "http://www.sweetwater.com/store/detail/Xone23",
                                    "rating": null,
                                    "from": {
                                        "source": "Sweetwater",
                                        "host": "http://www.sweetwater.com"
                                    },
                                    "descriptions": {
                                        "en": "2-channel/4-deck DJ Mixer with VCA Fading, 3-band \"Total Kill\" EQs, Assignable VCF, and Adjustable/Replaceable Crossfader",
                                        "cn": "2通道/ 4层DJ混音器，具有VCA衰落，3段“总杀伤”均衡器，可分配VCF和可调/可替换交叉衰减器"
                                    }
                                }
                            ],
                            "tags": [
                                "Xone23"
                            ]
                        },
                        "_id": "3"
                    },
                    {
                        "_source": {
                            "features": [
                                {
                                    "en": {
                                        "_name": "Sound Engine Type(s)",
                                        "_value": "Analog Modeling"
                                    },
                                    "cn": {
                                        "_name": "声音引擎类型",
                                        "_value": "Analog Modeling"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Number of Keys",
                                        "_value": "37"
                                    },
                                    "cn": {
                                        "_name": "键数",
                                        "_value": "37"
                                    }
                                }
                            ],
                            "images": [
                                {
                                    "image": "http://static.budee.com/yyren/image/6/1768.jpg",
                                    "width": 1800,
                                    "id": 1768,
                                    "height": 1214
                                },
                                {
                                    "image": "http://static.budee.com/yyren/image/6/1769.jpg",
                                    "width": 1600,
                                    "id": 1769,
                                    "height": 1010
                                }
                            ],
                            "created": "2016-10-12T01:55:39.000Z",
                            "name": "Access Limited Edition Virus TI2 Dark Star",
                            "category": [
                                {
                                    "en": "Keys",
                                    "cn": null
                                },
                                {
                                    "en": "Keyboards & MIDI",
                                    "cn": null
                                },
                                {
                                    "en": "Synths / Modules",
                                    "cn": null
                                }
                            ],
                            "brand": {
                                "reviews": 0,
                                "top": false,
                                "name": "Access",
                                "rating": 0,
                                "logo": "http://static.budee.com/yyren/image/220/14/974041.jpg"
                            },
                            "items": [
                                {
                                    "priceUnit": "$",
                                    "price": 2915,
                                    "seedId": 2,
                                    "origin": "http://www.sweetwater.com/store/detail/VirusTI2PDS",
                                    "rating": 1,
                                    "from": {
                                        "source": "Sweetwater",
                                        "host": "http://www.sweetwater.com"
                                    },
                                    "descriptions": {
                                        "en": "37-key Analog Modeling Synthesizer and 24-bit/192kHz USB Audio/MIDI Interface - LTD Edition \"Dark Star\" Color",
                                        "cn": "37键模拟合成器和24位/ 192kHz USB音频/ MIDI接口 -  LTD版“暗星”颜色"
                                    }
                                }
                            ],
                            "tags": [
                                "VirusTI2PDS"
                            ]
                        },
                        "_id": "4"
                    }
                ],
                "size": 2
            },
            "body": {
                "features": [
                    {
                        "columns": [
                            "Channels",
                            "2 + 2 Channel",
                            "-"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Analog Inputs",
                            "4 x stereo RCA line, 1 x XLR",
                            "-"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Analog Outputs",
                            "2 x XLR, 2 x stereo RCA (Monitor, Record)",
                            "-"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Headphones",
                            "1 x 1/4\", 1 x 1/8\"",
                            "-"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Other I/O",
                            "2 x Stereo RCA (FX Loop I/O)",
                            "-"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Faders",
                            "2",
                            "-"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Crossfader",
                            "Adjustable",
                            "-"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "EQs",
                            "3-band Full Cut",
                            "-"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Effects",
                            "Resonance (Sweepable Frequency)",
                            "-"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Rackmountable",
                            "Yes",
                            "-"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Width",
                            "9.4\"",
                            "22.3\""
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Depth",
                            "12.4\"",
                            "13.2\""
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Height",
                            "4.2\"",
                            "4.4\""
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Weight",
                            "6 lbs.",
                            "18.5 lbs."
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Manufacturer Part Number",
                            "AH-XONE:23",
                            "Virus TI2 Polar Darkstar"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Sound Engine Type(s)",
                            "-",
                            "Analog Modeling"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Number of Keys",
                            "-",
                            "37"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Type of Keys",
                            "-",
                            "Semi-weighted"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Other Controllers",
                            "-",
                            "Pitchbend, Mod Wheel"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Polyphony",
                            "-",
                            "20 Notes-90 Notes (Depending On the Patch)"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Number of Presets",
                            "-",
                            "512 RAM Patches, 3328 ROM Sounds"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Number of Effects",
                            "-",
                            "192"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Effects Types",
                            "-",
                            "Reverb, Chorus, Delay, Phaser, EQ, Ring Mod"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Arpeggiator",
                            "-",
                            "Yes"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Audio Inputs",
                            "-",
                            "2 x TS"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Audio Outputs",
                            "-",
                            "6 x TS, 1 x TRS (Headphones)"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Digital Inputs",
                            "-",
                            "1 x S/PDIF"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Digital Outputs",
                            "-",
                            "1 x S/PDIF"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "USB",
                            "-",
                            "1 x Type B"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "MIDI I/O",
                            "-",
                            "In/Out/Thru/USB"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Pedal Inputs",
                            "-",
                            "1 x Hold, 1 x Control"
                        ],
                        "collapsible": false
                    },
                    {
                        "columns": [
                            "Power Supply",
                            "-",
                            "Included"
                        ],
                        "collapsible": false
                    }
                ],
                "prices": [
                    {
                        "columns": [
                            "Sweetwater",
                            "$ 299.00",
                            "$ 2915.00"
                        ],
                        "collapsible": false
                    }
                ]
            }
        }
  
## 产品统计聚合接口 [/product/profile/aggregation?conditions={conditions}&values={values}&names={names}&fields={fields}&size={size}&image={image}]

+ Description
    + conditions[], values[], names[], fields[]是数组类型参数, 可以传递多个值
    + conditions和values的大小要一致, key value对
    + names和fields的大小要一致, 为每个聚合数据(field)提供一个命名(name)
    + fields增加向下钻取功能, 以'-'字符连接, e.g: level_0.raw-level_1.raw 表示先按一级分类聚合数据, 再按每个一级分类的二级分类聚合数据. 聚合深度没有限制, 依据索引如何建立.
    
+ Example
    + /product/profile/aggregation?names[]=category&fields[]=level_0.raw (一级产品类别聚合数据)
    + /product/profile/aggregation?conditions[]=level_0.raw&values[]=Guitars and Basses&names[]=category&fields[]=level_1.raw (查找一级类别(level_0)是【Guitars and Basses】的, 并按二级类别(level_1)聚合数据)

+ Response
    + aggregations.{name}.buckets[].image - 子分类图片
    + aggregations.{name}.buckets[].key - 子分类名称

+ Parameters
    + conditions (string) - 数组, 筛选条件名称, e.g[brand.name|category.raw|level_0.raw] = [品牌名称|分类|有层级的分类]
    + values (string) - 数组, 筛选条件值
    + names (required, string) - 数组, name
    + fields (required, string) - 数组, 聚合字段名称
    + size (integer) - default:100, 聚合数据集合大小
    + image (boolean) - default:true, 是否包含image字段

### 查找某个品牌下的子分类 [GET]

+ Response 200 (application/json)

        {
          "hits": {
            "hits": [], 
            "total": 38
          }, 
          "aggregations": {
            "category": {
              "buckets": [
                {
                  "image": "http://static.budee.com/yyren/image/60/13/867371.jpg", 
                  "doc_count": 29, 
                  "category": {
                    "buckets": [
                      {
                        "image": "http://static.budee.com/yyren/image/60/13/867371.jpg", 
                        "doc_count": 20, 
                        "category": {
                          "buckets": [
                            {
                              "image": "http://static.budee.com/yyren/image/60/13/867376.jpg", 
                              "doc_count": 5, 
                              "key": "Accesorios para batería"
                            }, 
                            {
                              "image": "http://static.budee.com/yyren/image/173/2/175374.jpg", 
                              "doc_count": 3, 
                              "key": "EDrum Cymbal Pads"
                            }, 
                            {
                              "image": "http://static.budee.com/yyren/image/173/2/175566.jpg", 
                              "doc_count": 3, 
                              "key": "EDrum Triggers"
                            }, 
                            {
                              "image": "http://static.budee.com/yyren/image/60/13/867371.jpg", 
                              "doc_count": 2, 
                              "key": "EDrum Bass Pads"
                            }, 
                            {
                              "image": "http://static.budee.com/yyren/image/60/13/867381.jpg", 
                              "doc_count": 2, 
                              "key": "EDrum HiHats and Controllers"
                            }, 
                            {
                              "image": "http://static.budee.com/yyren/image/60/13/867370.jpg", 
                              "doc_count": 2, 
                              "key": "EDrum Snare and Tom Pads"
                            }, 
                            {
                              "image": "http://static.budee.com/yyren/image/60/13/867382.jpg", 
                              "doc_count": 2, 
                              "key": "EDrum Sound Modules"
                            }, 
                            {
                              "image": "http://static.budee.com/yyren/image/173/2/175379.jpg", 
                              "doc_count": 1, 
                              "key": "Electronic Drumsets"
                            }
                          ]
                        }, 
                        "key": "E-Drums"
                      }, 
                      {
                        "image": "http://static.budee.com/yyren/image/60/13/867380.jpg", 
                        "doc_count": 6, 
                        "category": {
                          "buckets": [
                            {
                              "image": "http://static.budee.com/yyren/image/60/13/867380.jpg", 
                              "doc_count": 5, 
                              "key": "EDrum Hardware"
                            }, 
                            {
                              "image": "http://static.budee.com/yyren/image/173/2/175371.jpg", 
                              "doc_count": 1, 
                              "key": "Bass Drum Pedals"
                            }
                          ]
                        }, 
                        "key": "Drum Hardware"
                      }, 
                      {
                        "image": "http://static.budee.com/yyren/image/173/2/175500.jpg", 
                        "doc_count": 3, 
                        "category": {
                          "buckets": [
                            {
                              "image": "http://static.budee.com/yyren/image/173/2/175500.jpg", 
                              "doc_count": 3, 
                              "key": "Mesh Heads for E-Drums"
                            }
                          ]
                        }, 
                        "key": "Drum Heads"
                      }
                    ]
                  }, 
                  "key": "Drums + Percussion"
                }, 
                {
                  "image": "http://static.budee.com/yyren/image/241/10/717218.jpg", 
                  "doc_count": 8, 
                  "category": {
                    "buckets": [
                      {
                        "image": "http://static.budee.com/yyren/image/241/10/717218.jpg", 
                        "doc_count": 8, 
                        "category": {
                          "buckets": [
                            {
                              "image": "http://static.budee.com/yyren/image/241/10/717218.jpg", 
                              "doc_count": 6, 
                              "key": "Acoustic Triggers"
                            }, 
                            {
                              "image": "http://static.budee.com/yyren/image/134/14/952058.jpg", 
                              "doc_count": 2, 
                              "key": "Electronic Drum Sets"
                            }
                          ]
                        }, 
                        "key": "Electronic Drums"
                      }
                    ]
                  }, 
                  "key": "Drums & Percussion"
                }, 
                {
                  "image": "http://static.budee.com/yyren/image/60/13/867390.jpg", 
                  "doc_count": 1, 
                  "category": {
                    "buckets": [
                      {
                        "image": "http://static.budee.com/yyren/image/60/13/867390.jpg", 
                        "doc_count": 1, 
                        "category": {
                          "buckets": [
                            {
                              "image": "http://static.budee.com/yyren/image/60/13/867390.jpg", 
                              "doc_count": 1, 
                              "key": "Power Supplies"
                            }
                          ]
                        }, 
                        "key": "Guitar + Bass Effects"
                      }
                    ]
                  }, 
                  "key": "Guitars and Basses"
                }
              ]
            }
          }
        }

## 品牌详细 [/product/brand/{name}]

+ Response
    + hits.hits[]._id - 唯一标识符, 取品牌名称
    + hits.hits[]._source.features[] - 品牌的一些参数
    + hits.hits[]._source.brandName - 品牌名称
    + hits.hits[]._source.reviews - 品牌评论/浏览数
    + hits.hits[]._source.rating - 品牌评分, 取值区间[0,1], 保留2位小数
    + hits.hits[]._source.logo - 品牌logo
    + hits.hits[]._source.description - 品牌描述
    + hits.hits[]._source.rawContent - 品牌HTML富文本

+ Parameters
    + name (string) - 品牌名称(大小写敏感)

### 品牌详细 [GET]

+ Response 200 (application/json)

        {
          "hits": {
            "hits": [
              {
                "_source": {
                  "features": [
                    {
                      "_name": "Manufacturer Ranking", 
                      "_value": "481"
                    }, 
                    {
                      "_name": "In our range since", 
                      "_value": "2009"
                    }, 
                    {
                      "_name": "Product range", 
                      "_value": "30"
                    }, 
                    {
                      "_name": "On Stock", 
                      "_value": "30"
                    }, 
                    {
                      "_name": "Ø Availability (1 Year)", 
                      "_value": "85.81 %"
                    }
                  ],
                  "brandName": "2box", 
                  "reviews": 174, 
                  "rating": 0.85, 
                  "logo": "http://static.budee.com/yyren/image/213/14/972244.jpg", 
                  "description": "Thomann is Germany's most significant 2box dealer. Here you'll find all their latest products at especially low prices and immediately available. If you would like to see a list of all products from 2box, then please click here.", 
                  "rawContent": "<p>You can find 30 2box products at Thomann 30 of them are ready for dispatch . 2box products have been a part of our range for 7 year(s).</p>"
                }, 
                "_id": "2box", 
                "_score": 1
              }
            ],
            "total": 1
          }
        }

## 品牌 [/product/profile/_search/aggregation?type={type}&size={size}]

+ Response
    + aggregations.{type-name}.buckets - 根据搜索结果实时聚合的数据集合或实体对象
    + aggregations.{type-name}.buckets[].key - label名称, 会按key名称进行分组[A, B, C..., Z, #]
    + aggregations.{type-name}.buckets[].doc_count - count统计
    + aggregations.{type-name}.buckets[].id - 品牌ID
    + aggregations.{type-name}.buckets[].ranking - 品牌排名(基于thomann)
    + aggregations.{type-name}.buckets[].top - true为有品牌内容简介的, false为没有品牌介绍的

+ Parameters
    + type (string) - 默认brand, 可选值[brand] 
    + size (number) - 默认3000, 品牌列表的数量

### 品牌列表 [GET]

+ Response 200 (application/json)

        {
            "hits": {
                "hits": [], 
                "total": 117298
            },
            "aggregations": {
                "brand": {
                  "buckets": {
                    "#": [
                      {
                        "doc_count": 43, 
                        "logo": "http://static.budee.com/yyren/image/13.jpg", 
                        "key": "9.solutions",
                        "reviews": 99,
                        "top": true
                      }, 
                      {
                        "doc_count": 34, 
                        "logo": "http://static.budee.com/yyren/image/11.jpg", 
                        "key": "Äolis Klangspiele",
                        "reviews": 97,
                        "top": true
                      }, 
                      {
                        "doc_count": 30, 
                        "logo": "http://static.budee.com/yyren/image/9.jpg", 
                        "key": "2box",
                        "reviews": 33,
                        "top": true
                      }
                    ]
                }
            }
        }

## 产品详情 [/product/profile/{doc_id}]

+ Response
    + hits.total - 记录总数
    + hits.hits[]._source.category[] - 产品分类
    + hits.hits[]._source.brand - 品牌
    + hits.hits[]._source.tags[] - （保留）标签
    + hits.hits[]._source.name - 产品名称
    + hits.hits[]._source.description - 产品描述
    + hits.hits[]._source.contents - 富文本内容
    + hits.hits[]._source.origin - 原始链接
    + hits.hits[]._source.priceUnit - 价格单位
    + hits.hits[]._source.price - 价格
    + hits.hits[]._source.rating - 产品评级, 取值范围[0,1], (0% ~ 100%) 
    + hits.hits[]._source.seedId - 产品所属种子网站的ID
    + hits.hits[]._source.created - 抓取时间
    + hits.hits[]._source.images[] - 图片集合
    + hits.hits[]._source.images[].id - 图片标识符
    + hits.hits[]._source.images[].image - 本地图片链接
    + hits.hits[]._source.images[].width - 宽度
    + hits.hits[]._source.images[].height - 高度
    + hits.hits[]._source.images[].title - 图片标题
    + hits.hits[]._source.images[].alt - 图片alt
    + hits.hits[]._source.images[].sizes[] - 图片大小集合 e.g: ['80','220','400','800','1200'], 小于等于原生图片的最大高度或宽度 
    + hits.hits[]._source.features[]._name - 特性名称
    + hits.hits[]._source.features[]._value - 特性值

+ Parameters
    + doc_id (required, number) - 产品文档ID

### 查询产品详细信息 [GET]

+ Response 200 (application/json)

        {
            "hits": {
                "hits": [
                    {
                        "_source": {
                            "images360": [],
                            "priceUnit": "$",
                            "images": [
                                {
                                    "image": "http://static.budee.com/yyren/image/6/1768.jpg",
                                    "width": 1800,
                                    "id": 1768,
                                    "height": 1214
                                },
                                {
                                    "image": "http://static.budee.com/yyren/image/6/1769.jpg",
                                    "width": 1600,
                                    "id": 1769,
                                    "height": 1010
                                },
                                {
                                    "image": "http://static.budee.com/yyren/image/6/1770.jpg",
                                    "width": 1600,
                                    "id": 1770,
                                    "height": 489
                                },
                                {
                                    "image": "http://static.budee.com/yyren/image/6/1771.jpg",
                                    "width": 1600,
                                    "id": 1771,
                                    "height": 396
                                }
                            ],
                            "created": "2016-10-12T01:55:39.000Z",
                            "origin": "http://product.mifanfan.cn/product/4",
                            "rating": 1,
                            "descriptions": {
                                "en": "37-key Analog Modeling Synthesizer and 24-bit/192kHz USB Audio/MIDI Interface - LTD Edition \"Dark Star\" Color",
                                "cn": "37键模拟合成器和24位/ 192kHz USB音频/ MIDI接口 -  LTD版“暗星”颜色"
                            },
                            "tags": [
                                "VirusTI2PDS"
                            ],
                            "features": [
                                {
                                    "en": {
                                        "_name": "Sound Engine Type(s)",
                                        "_value": "Analog Modeling"
                                    },
                                    "cn": {
                                        "_name": "声音引擎类型",
                                        "_value": "Analog Modeling"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Number of Keys",
                                        "_value": "37"
                                    },
                                    "cn": {
                                        "_name": "键数",
                                        "_value": "37"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Type of Keys",
                                        "_value": "Semi-weighted"
                                    },
                                    "cn": {
                                        "_name": "键类型",
                                        "_value": "Semi-weighted"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Other Controllers",
                                        "_value": "Pitchbend, Mod Wheel"
                                    },
                                    "cn": {
                                        "_name": "其他控制器",
                                        "_value": "Pitchbend, Mod Wheel"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Polyphony",
                                        "_value": "20 Notes-90 Notes (Depending On the Patch)"
                                    },
                                    "cn": {
                                        "_name": "复调",
                                        "_value": "20 Notes-90 Notes (Depending On the Patch)"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Number of Presets",
                                        "_value": "512 RAM Patches, 3328 ROM Sounds"
                                    },
                                    "cn": {
                                        "_name": "预置数",
                                        "_value": "512 RAM Patches, 3328 ROM Sounds"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Number of Effects",
                                        "_value": "192"
                                    },
                                    "cn": {
                                        "_name": "效果数",
                                        "_value": "192"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Effects Types",
                                        "_value": "Reverb, Chorus, Delay, Phaser, EQ, Ring Mod"
                                    },
                                    "cn": {
                                        "_name": "效果类型",
                                        "_value": "Reverb, Chorus, Delay, Phaser, EQ, Ring Mod"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Arpeggiator",
                                        "_value": "Yes"
                                    },
                                    "cn": {
                                        "_name": "琶音",
                                        "_value": "Yes"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Audio Inputs",
                                        "_value": "2 x TS"
                                    },
                                    "cn": {
                                        "_name": "音频输入",
                                        "_value": "2 x TS"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Audio Outputs",
                                        "_value": "6 x TS, 1 x TRS (Headphones)"
                                    },
                                    "cn": {
                                        "_name": "音频输出",
                                        "_value": "6 x TS, 1 x TRS (Headphones)"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Digital Inputs",
                                        "_value": "1 x S/PDIF"
                                    },
                                    "cn": {
                                        "_name": "数字输入",
                                        "_value": "1 x S/PDIF"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Digital Outputs",
                                        "_value": "1 x S/PDIF"
                                    },
                                    "cn": {
                                        "_name": "数字输出",
                                        "_value": "1 x S/PDIF"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "USB",
                                        "_value": "1 x Type B"
                                    },
                                    "cn": {
                                        "_name": "USB",
                                        "_value": "1 x Type B"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "MIDI I/O",
                                        "_value": "In/Out/Thru/USB"
                                    },
                                    "cn": {
                                        "_name": "MIDI I / O",
                                        "_value": "In/Out/Thru/USB"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Pedal Inputs",
                                        "_value": "1 x Hold, 1 x Control"
                                    },
                                    "cn": {
                                        "_name": "踏板输入",
                                        "_value": "1 x Hold, 1 x Control"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Power Supply",
                                        "_value": "Included"
                                    },
                                    "cn": {
                                        "_name": "电源",
                                        "_value": "Included"
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Height",
                                        "_value": "4.4\""
                                    },
                                    "cn": {
                                        "_name": "高度",
                                        "_value": "4.4\""
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Width",
                                        "_value": "22.3\""
                                    },
                                    "cn": {
                                        "_name": "宽度",
                                        "_value": "22.3\""
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Depth",
                                        "_value": "13.2\""
                                    },
                                    "cn": {
                                        "_name": "深度",
                                        "_value": "13.2\""
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Weight",
                                        "_value": "18.5 lbs."
                                    },
                                    "cn": {
                                        "_name": "重量",
                                        "_value": "18.5 lbs."
                                    }
                                },
                                {
                                    "en": {
                                        "_name": "Manufacturer Part Number",
                                        "_value": "Virus TI2 Polar Darkstar"
                                    },
                                    "cn": {
                                        "_name": "制造商零件编号",
                                        "_value": "Virus TI2 Polar Darkstar"
                                    }
                                }
                            ],
                            "contents": {
                                "en": "<h2>Hot Synth in a LTD Edition Color!</h2> \n<p>The Access Virus TI2 Dark Star synthesizer takes the revered Virus TI to the next level, boasting 25% more calculating power, a more robust onboard effects section, a lighter weight, and a completely redesigned housing. At the heart of the Virus TI2 Dark Star is the new OS3 which comes with new effects - Frequency Shifter, Tape Delay, new Distortions, and Character. What's more, it now includes the enhanced Virus Control 3.0 plug-in, giving you even deeper control of the synthesizer and your many presets. Already a known Ferrari of a synth, expect even more from the Access Virus TI2 Dark Star.</p> \n<p>Access somehow managed to improve on the revered Virus TI system, taking the \"Ferrari\" to the next level of performance - and TI computer integration. With a lighter, redesigned enclosure, a bolstered effects section, and an enhanced Virus Control 3.0 plug-in - plus all the staples that made the Virus the famous powerhouse - the Virus TI2 Dark Star is truly the culmination of Access's 12-year triathlon of sound research, distillation of user input, and their simple desire to create an exceptional instrument.</p> \n<p>The new Access Virus TI2 Dark Star features an even more powerful effects section, bringing studio-favorite effects to the live performance realm. In addition to phaser, chorus/flanger, ring modulator/shifter, EQ, and a global vocoder, onboard are a new Tape Delay effect, a Frequency Shifter, and new Distortions. There's also a new Character effect, which lets you shape the timbre of the Virus's sound and its fit in the mix, using Analog Boost, Vintage 1/2/3, Pad Opener, Lead Enhancer, Bass Enhancer, or Stereo Widener \"Characters.\"</p> \n<p>The TI in the Virus TI2 Dark Star stands for Total Integration with your DAW - and Access has heightened that integration, with the Access Virus TI2 line. With the new Virus Control 3.0 plug-in, you have enhanced control over your Virus TI2, right inside your DAW. There's also an even more robust presets manager, so you can organize, sort, search, and tweak - as quickly as you want to work.</p> \n<p>The Access Virus TI2 Dark Star features three main oscillators and one sub-oscillator per voice. Each main oscillator can be made up of various oscillator types, including Hyper Saw (a multi saw-tooth oscillator with up to 9 stacked oscillators, 9 sub oscillators, and a sync oscillator at the same time), Classic Virtual Analog oscillators (saw, variable pulse, sine, triangle, 62 spectral waves with several FM modes), Graintable, Wavetable (with 100 multi-index wavetables), and Format oscillators</p> \n<p>You can use two fully independent filters with the Access Virus TI2 Dark Star - lowpass, highpass, bandpass, and bandstop) while using an optional saturation module between both filter blocks. The saturation module can add one of several distortions and lo-fi effects, or an additional low/highpass filter. There's also optional self-resonating Moog cascade filter simulation, with circuit overload and 1-4 poles. What's more, the Access Virus TI2 Dark Star offers a 2-dimensional modulation matrix, with 6 slots (1 source and 3 modulation targets each), and you can modulate parameters in real-time. </p> \n<p>Every patch onboard the Access Virus TI2 keyboard features its own arpeggiator pattern with 32 programmable steps (length and velocity can be adjusted per step) and a global control for swing/shuffle timing and for note lengths. You can control all of this using the modulation matrix.</p> \n<p>The new Access Virus TI2 Dark Star retains is classy look - all designed, engineered, and built in Germany. It also features a lighter weight, so the Virus TI2 Dark Star is even more prepared for the stage.</p> \n<p><b>Access Virus TI2 Dark Star 37-key Synthesizer Features:</b> </p> \n<ul> \n <li>New Effects, including Tape Delay, Frequency Shifter, new Distortions, and Character </li> \n <li>25% more calculating power than Virus TI, and completely redesigned housing </li> \n <li>Dual DSP system with over 80 stereo voices under average load. (Load depends on which oscillator / filter model has been chosen). </li> \n <li>Virus Control 3.0 VST and Apple Audio Unit Plug-for Mac OS X and Windows XP. The remote seamlessly integrates the Virus TI into your sequencer, making it feel just like a plug-in. </li> \n <li>The Virus TI's Audio and MIDI inputs and outputs can be used by the sequencer application as an audio and MIDI interface. </li> \n <li>The Virus TI is the first hardware synthesizer with sample-accurate timing and delay-compensated connection to your sequencer. </li> \n <li>WaveTable Oscillators for a completely new array of sounds. WaveTable and conventional Virus oscillators and filters can be mixed. </li> \n <li>HyperSaw oscillators with up to 9 sawtooths - each with parallel sub oscillator per voice (that's over 1800 stereo oscillators @ 100 voices!). </li> \n <li>Independent delay and reverb for all 16 multi mode slots. </li> \n <li>129 parallel effects. There is reverb and delay, chorus, phaser, ring modulator, distortion, 3 band EQ and the Analog Boost bass enhancer. </li> \n <li>2 multi-mode filters (HP, LP, BP, BS) and the Analog Filter (modeled after the MiniMoog cascade filter with 6-24 dB Slope and self-oscillation). </li> \n <li>Dedicated remote mode turns the Virus TI into an universal remote control for VST / AU plug-ins and external synthesizers. </li> \n <li>6 balanced outputs with +4 dB level and switchable soft limiting algorithm. Studio grade 192 khz D/A converters with S/PDIF digital I/O. 2x24 bit inputs. Surround sound capabilities </li> \n <li>Tap tempo button. The algorithm is based on Access' Sync Xtreme technology. </li> \n <li>Programmable arpeggiator pattern for every patch. </li> \n <li>Knob quantize for creating stepped controller movements. The stepping automatically syncs to the Virus clock or an incoming MIDI clock. </li> \n <li>3 LFOs with 68 waveforms to choose from. </li> \n <li>2 super fast ADSTR envelopes. </li> \n <li>Extended memory: 512 RAM patches and 2048 ROM patches (rewritable). </li> \n <li>Adaptive control smoothing for jitter-free modulations on all important parameters. </li> \n <li>Multi mode with embedded patches. </li> \n <li>Compatible with USB 2.0 specifications, USB and High-Speed USB devices. </li> \n <li>Great synth-action keyboard with 37 keys, velocity response, and aftertouch. </li> \n <li>2 pedal inputs.</li> \n</ul> \n<p><span>The Access Virus TI2 Dark Star takes the renowned Virus TI synthesizer to the next level!</span></p>",
                                "cn": "<h2>热合成在LTD版颜色！</h2>\n<p> Access Virus TI2 Dark Star合成器将受尊敬的病毒TI提升到更高的水平，拥有25％的计算能力，更强大的板载效果部分，更轻的重量以及完全重新设计的外壳。病毒的核心TI2 Dark Star是新的OS3，它带有新的效果 - 频率移位，磁带延迟，新的失真和字符。此外，它现在包括增强的Virus Control 3.0插件，让您更深入地控制合成器和您的许多预设。已经是一个合成器的已知法拉利，希望更多的从Access病毒TI2暗星。</p>\n<p>以某种方式访问​​可以改善受尊敬的病毒TI系统，将“法拉利”提升到更高水平的性能 - 以及TI计算机集成。通过一个更轻，重新设计的外壳，一个加固的效果部分和一个增强的Virus Control 3.0插件 - 加上所有使病毒成为着名的发电厂的订书钉 - 病毒TI2黑暗星是Access的12年铁人三项赛声音研究，用户输入的蒸馏，以及他们创建特殊乐器的简单愿望。</p>\n<p>新的Access Virus TI2 Dark Star拥有更强大的效果部分，将现场演出最喜欢的效果带到现场表演领域。除了相位器，合唱/镶边器，环形调制器/移位器，EQ和全球声码器外，还有新的磁带延迟效应，频移器和新的失真。还有一个新的字符效果，它可以让你塑造病毒的声音的音色和它的混合，使用模拟升压，复古1/2/3，打开程序，铅增强器，低音增强器或立体声宽度字符的适合。 “</p>\n<p>病毒TI2 Dark Star中的TI代表与您的DAW的总集成 - 并且Access已经提高了与Access病毒TI2线的集成。使用新的Virus Control 3.0插件，您可以在DAW内部对病毒TI2进行增强的控制。还有一个更强大的预设管理器，所以你可以组织，排序，搜索和调整，尽快，你想工作。</p>\n<p> Access Virus TI2 Dark Star每个声音具有三个主振荡器和一个子振荡器。每个主振荡器可以由各种振荡器类型组成，包括超锯（多锯齿振荡器，具有多达9个堆叠振荡器，9个子振荡器和同时的同步振荡器），经典虚拟模拟振荡器（锯，可变脉冲，正弦，三角形，62个具有多个FM模式的光谱波），Graintable，Wavetable（具有100个多折射波表）和格式振荡器</p>\n<p>您可以使用两个完全独立的过滤器与Access Virus TI2 Dark Star  - 低通，高通，带通和带阻），同时在两个滤波器模块之间使用可选的饱和模块。饱和模块可以添加几个失真和低音效果中的一个，或附加的低/高通滤波器。还有可选的自谐振Moog级联滤波器模拟，电路过载和1-4极。此外，Access Virus TI2 Dark Star提供了一个2维调制矩阵，具有6个插槽（每个1个源和3个调制目标），您可以实时调制参数。 </p>\n<p> Access Virus TI2键盘上的每个补丁都具有自己的琶音模式，具有32个可编程步长（每步可以调整长度和速度），以及摇摆/随机定时和音符长度的全局控制。您可以使用调制矩阵来控制所有这些。</p>\n<p>新的Access病毒TI2 Dark Star保留了优雅的外观 - 所有设计，工程和建造在德国。它还具有更轻的重量，所以病毒TI2黑暗星更为准备的舞台。</p>\n<p> <b> Access Virus TI2 Dark Star 37键合成器功能：</b> </p>\n<ul>\n <li>新效果，包括磁带延迟，频率移位，新失真和字符</li>\n <li>计算能力比病毒TI多25％，并彻底重新设计了</li>\n <li>双DSP系统在平均负载下具有超过80个立体声。 （负载取决于选择的振荡器/滤波器模型）。 </li>\n <li> Virus Control 3.0 VST和Apple Audio Unit Plug-for Mac OS X和Windows XP。远程无缝集成病毒TI到您的音序器，使它感觉就像一个插件。 </li>\n <li>病毒TI的音频和MIDI输入和输出可以由音序器应用程序用作音频和MIDI接口。 </li>\n <li> Virus TI是第一款具有精确的定时和延迟补偿连接到您的音序器的硬件合成器。 </li>\n <li> WaveTable振动器用于一个全新的声音阵列。 WaveTable和常规病毒振荡器和过滤器可以混合使用。 </li>\n <li>具有多达9个锯齿的HyperSaw振荡器 - 每个具有每个声音的并行子振荡器（超过1800个立体声振荡器，100个语音！）。 </li>\n <li>所有16个多模式插槽的独立延迟和混响。 </li>\n <li> 129并行效果。有混响和延迟，合唱，移相器，环形调制器，失真，3频段EQ和模拟升压低音增强器。 </li>\n <li> 2多模式滤波器（HP，LP，BP，BS）和模拟滤波器（在MiniMoog级联滤波器之后建模，具有6-24 dB斜率和自振荡）。 </li>\n <li>专用远程模式将病毒TI转换为适用于VST /AU插件和外部合成器的通用遥控器。 </li>\n <li> 6个平衡输出，具有+4 dB电平和可切换软限位算法。工作室级192 khz D /A转换器与S /PDIF数字I /O。 2x24位输入。环绕声功能</li>\n <li>点击节奏按钮。该算法基于Access的同步Xtreme技术。 </li>\n <li>每个补丁都有可编程的琶音模式。 </li>\n <li>旋钮量化用于创建阶梯式控制器移动。步进会自动同步到病毒时钟或传入的MIDI时钟。 </li>\n <li> 3个LFO，可选择68种波形。 </li>\n <li> 2超快速ADSTR信封。 </li>\n <li>扩展内存：512个RAM修补程序和2048个ROM修补程序（可重写）。 </li>\n <li>自适应控制平滑所有重要参数的无抖动调制。 </li>\n <li>带有嵌入式补丁的多模式。 </li>\n <li>兼容USB 2.0规格，USB和高速USB设备。 </li>\n <li>具有37键，速度响应和触感的伟大的合成键盘。 </li>\n <li> 2踏板输入。</li>\n</ul>\n<p> <span> Access病毒TI2 Dark Star将着名的病毒TI综合器升级到更高级别！</span> </p>"
                            },
                            "price": 2915,
                            "seedId": 0,
                            "name": "Access Limited Edition Virus TI2 Dark Star",
                            "from": {
                                "host": "http://product.mifanfan.cn/",
                                "source": "Music Fans"
                            },
                            "category": [
                                {
                                    "en": "Keys",
                                    "cn": "键盘"
                                },
                                {
                                    "en": "Keyboards & MIDI",
                                    "cn": "键盘 & MIDI"
                                },
                                {
                                    "en": "Synths / Modules",
                                    "cn": "合成器/模块"
                                }
                            ],
                            "brand": {
                                "reviews": 0,
                                "top": false,
                                "name": "Access",
                                "rating": 0,
                                "logo": "http://static.budee.com/yyren/image/220/14/974041.jpg"
                            }
                        },
                        "_id": "4",
                        "_score": 0.35355338
                    }
                ],
                "total": 1
            }
        }


## 产品搜索 [/product/profile/_search?q={q}&c={c}&s={s}&page={page}&size={size}&cluster={cluster}&aggregation={aggregation}&group={group}&b={b}]

+ Description
    + Condition [0_0_0_0_0_0] e.g: bass_0_0_50.0-500.0_FF0000_1, 查询关键词是bass价格在50.0-500.0之间的来源于ID=1的网站产品并按图片颜色[FF0000]红色降序排序, '-'为多选或区间, '_'为不同字段的delimiter符号
    + category:支持多选
    + brand:支持多选
    + tag:(保留字段)数据与category合并
    + price:格式一[50000.0-\*] 格式二[100.0-200.0] 格式三[\*-100.0]
    + color:单选 可用数据集合['000000','0000FF','00FF00','00FFFF','FF0000','FF00FF','FFFF00','FFFFFF']
    + from:来源网站的ID 支持多选
    + group:默认true, 是否按首字母分组如[A,B,C,...Z]
    + b:bit位, 显示的聚合数据 1:category 2:brand 4:price 8:from. e.g: 如果为【3】, 聚合结果中显示category与brand聚合数据. 默认显示不在条件中的其他聚合数据

+ Response
    + hits.total - 记录总数
    + hits.hits[]._source.category[] - 产品分类
    + hits.hits[]._source.brand - 品牌
    + hits.hits[]._source.tags[] - （保留）标签
    + hits.hits[]._source.name - 产品名称
    + hits.hits[]._source.items.seedId - 产品所属种子网站的ID
    + hits.hits[]._source.items.origin - 原始链接
    + hits.hits[]._source.items.priceUnit - 价格单位
    + hits.hits[]._source.items.price - 价格
    + hits.hits[]._source.items.rating - 产品评级, 取值范围[0,1], (0% ~ 100%) 
    + hits.hits[]._source.items.from.host - 网站主页
    + hits.hits[]._source.items.from.source - 网站简称
    + hits.hits[]._source.items.description - 产品简述
    + hits.hits[]._source.created - 抓取时间
    + hits.hits[]._source.images[] - 图片集合
    + hits.hits[]._source.images[].id - 图片标识符
    + hits.hits[]._source.images[].image - 本地图片链接
    + hits.hits[]._source.images[].width - 宽度
    + hits.hits[]._source.images[].height - 高度
    + hits.hits[]._source.images[].title - 图片标题
    + hits.hits[]._source.images[].alt - 图片alt
    + hits.hits[]._source.images[].sizes[] - 图片大小集合 e.g: ['80','220','400','800','1200'], 小于等于原生图片的最大高度或宽度 
    + hits.hits[]._source.features[]._name - 特性名称
    + hits.hits[]._source.features[]._value - 特性值
    + hits.hits[].highlight.contents.[en|cn].[] - 根据q选择相关度最高的n段句子, 并使用<span class='highlight'>{key}</span>包围关键词
    + aggregations.{name}.buckets - 根据搜索结果实时聚合的数据集合或实体对象
    + aggregations.{name}.buckets[].key - label名称, 会按key名称进行分组[A, B, C..., Z, #]
    + aggregations.{name}.buckets[].doc_count - count统计
    + aggregations.{name}.buckets[].reviews - (integer) 品牌评论/浏览数
    + aggregations.{name}.buckets[].rating - (double) 品牌评分,取值区间[0,1]
    + aggregations.{name}.buckets[].top - (boolean) 是否是置顶品牌（该品牌有详细数据）
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
    + s (string) - 排序字段目前是排他的, 排序优先级比颜色高, 可选参数, 没有的话按关键词相关度排序. 格式:[items.price|items.created]-[0|1], e.g: price-0 (0:ASC, 1:DESC)
    + page (number) - 页码
    + size (number) - 大小
    + cluster (boolean) - 是非返回搜索结果聚类数据
    + aggregation (string) - 聚合条件[0_0_0_50], (0|1:关闭|打开 聚合搜索, 0|1:asc|desc, 0|1:term|count 排序字段, 50:返回聚合结果的size)
    + group (boolean) - default true, 是否按首字母分组
    + b (number) - 取值范围一个字节的范围[0,15], 1:category 2:brand 4:price 8:from 以及其各种组合, 控制显示在聚合字段（aggs）中的数据

### 产品搜索 [GET]

+ Request (application/json)

        {
            "q": "bass",
            "c": "0_0_0_*-200.0_0_0"
            "s": "price-0"
            "page": 1,
            "size": 20,
            "cluster": "false",
            "aggregation": "0_0_0_50",
            "group": true,
            "b":3
        }

+ Response 200 (application/json)

        {
            "hits": {
                "hits": [
                    {
                        "highlight": {
                            "items.contents.cn": [
                                "<span class='highlight'>吉他</span>合成器+建模   最新一代的不同的，可编辑的Roland合成器声音   钢琴   字符串   管乐器   声音   复古合成器等   最多可选择并组合两种声音   来自VG-99的完整建模声链（<span class='highlight'>吉他</span>/贝司/合成器/效果/安培）包括虚拟调音拾音器可以输入到建模链   两个合成器声音可以与建模声音组合，合成器声音可以输入模型链（例如蓝调 - 竖琴到失真放大器）   270预设 - 按“lead”，“rhythm”和“others”排序的声音   297个自定义声音程序   USB插槽用于连接USB存储设备   通过脚踏开关播放音频文件   控制脚踏开关和表情踏板（可分配给多种功能）   20秒循环，无穷无尽的   GR-55为音频和MIDI以及常规MIDI输入/输出插座提供USB接口   GK-Pickup - 需要特殊拾音器GK-3（<span class='highlight'>吉他</span>）或GK-3B（低音）或GK-Ready<span class='highlight'>吉他</span>/低音（可选） 包括电源适配器   尺寸（宽x高x深）：405 x 78-106 x 244毫米   重量：3.3公斤"
                            ],
                            "items.contents.en": [
                                "Up to two sounds can be selected and combined Complete modelling sound chain (guitar/bass/synth/effects/amps) from the VG-99 including virtual tuning pickup can be fed into the modelling chain Two synth sounds can be combined with the modelling sounds - synth sounds can be fed into the modellin chain (e.g. blues-harp into distorted amp) 270 Preset-Sounds sorted by \"lead\", \"rhythm\" and \"others\" 297 programs for custom sounds USB slot for connection of a USB-Memory device Playback of audio files via foot switch Control foot-Switch and expression pedal (assignable to a multitude of functions) 20-Second-Loop with an endless amount of overdubs The GR-55 provides a USB-Interface for audio and MIDI, plus conventional MIDI-In/Out sockets GK-Pickup - special pickup GK-3 (guitar) or GK-3B (bass) or GK-Ready <span class='highlight'>Gitar</span>/Bass is required (optionally available) Including power adapter Dimensions (W x H x D): 405 x 78-106 x 244 mm Weight: 3.3 kg"
                            ]
                        },
                        "_source": {
                            "images": [
                                {
                                    "image": "http://static.budee.com/yyren/image/226/4/320187.jpg",
                                    "width": 800,
                                    "id": 320187,
                                    "height": 546
                                },
                                {
                                    "image": "http://static.budee.com/yyren/image/226/4/320188.jpg",
                                    "width": 800,
                                    "id": 320188,
                                    "height": 366
                                }
                            ],
                            "created": "2016-10-18T00:12:25.000Z",
                            "name": "GR-55S Black",
                            "category": [
                                {
                                    "en": "Keys",
                                    "cn": "键盘"
                                },
                                {
                                    "en": "Guitars and Basses",
                                    "cn": "吉他和贝斯"
                                },
                                {
                                    "en": "Guitar + Bass Effects",
                                    "cn": "吉他 + 贝斯的影响"
                                }
                            ],
                            "brand": {
                                "reviews": 7206,
                                "top": true,
                                "name": "Roland",
                                "rating": 0.92,
                                "logo": "http://static.budee.com/yyren/image/215/14/972687.jpg"
                            },
                            "items": [
                                {
                                    "priceUnit": "$",
                                    "price": 573.4,
                                    "seedId": 11,
                                    "origin": "https://www.thomann.de/gb/roland_gr55s_black.htm",
                                    "rating": 0.92,
                                    "from": {
                                        "source": "Thomann",
                                        "host": "https://www.thomann.de"
                                    },
                                    "descriptions": {
                                        "en": "",
                                        "cn": ""
                                    }
                                }
                            ],
                            "tags": [
                                "279996"
                            ]
                        },
                        "_id": "51313",
                        "_score": 0.17167637
                    },
                    {
                        "highlight": {
                            "items.contents.cn": [
                                "Harley Benton<span class='highlight'>吉他</span>拾音器"
                            ]
                        },
                        "_source": {
                            "images": [
                                {
                                    "image": "http://static.budee.com/yyren/image/14/10/659174.jpg",
                                    "width": 486,
                                    "id": 659174,
                                    "height": 600
                                },
                                {
                                    "image": "http://static.budee.com/yyren/image/14/10/659175.jpg",
                                    "width": 486,
                                    "id": 659175,
                                    "height": 600
                                }
                            ],
                            "created": "2016-10-21T13:44:41.000Z",
                            "name": "Guitar Pick Thin",
                            "category": [
                                {
                                    "en": "Guitars and Basses",
                                    "cn": "吉他和贝斯"
                                },
                                {
                                    "en": "Accessories",
                                    "cn": "附件"
                                },
                                {
                                    "en": "Standard Picks",
                                    "cn": "标准的选择"
                                }
                            ],
                            "brand": {
                                "reviews": 103303,
                                "top": true,
                                "name": "Harley Benton",
                                "rating": 0.87,
                                "logo": "http://static.budee.com/yyren/image/218/14/973387.jpg"
                            },
                            "items": [
                                {
                                    "priceUnit": "$",
                                    "price": 0.27,
                                    "seedId": 11,
                                    "origin": "https://www.thomann.de/gb/harley_benton_guitarpick_thin.htm",
                                    "rating": 0.82,
                                    "from": {
                                        "source": "Thomann",
                                        "host": "https://www.thomann.de"
                                    },
                                    "descriptions": {
                                        "en": "Harley Benton guitar pick - thin",
                                        "cn": "哈利本顿吉他挑"
                                    }
                                }
                            ],
                            "tags": [
                                "149393"
                            ]
                        },
                        "_id": "107321",
                        "_score": 0.13887548
                    }
                ],
                "total": 44363
            },
            "aggregations": {
                "price": {
                    "buckets": [
                        {
                            "doc_count": 15722,
                            "from": "-Infinity",
                            "to": 100,
                            "key": "*-100.0"
                        },
                        {
                            "doc_count": 6485,
                            "from": 100,
                            "to": 200,
                            "key": "100.0-200.0"
                        },
                        {
                            "doc_count": 3870,
                            "from": 200,
                            "to": 300,
                            "key": "200.0-300.0"
                        },
                        {
                            "doc_count": 4652,
                            "from": 300,
                            "to": 500,
                            "key": "300.0-500.0"
                        },
                        {
                            "doc_count": 6343,
                            "from": 500,
                            "to": 1000,
                            "key": "500.0-1000.0"
                        },
                        {
                            "doc_count": 4042,
                            "from": 1000,
                            "to": 2000,
                            "key": "1000.0-2000.0"
                        },
                        {
                            "doc_count": 3611,
                            "from": 2000,
                            "to": 5000,
                            "key": "2000.0-5000.0"
                        },
                        {
                            "doc_count": 629,
                            "from": 5000,
                            "to": 10000,
                            "key": "5000.0-10000.0"
                        },
                        {
                            "doc_count": 1,
                            "from": 50000,
                            "to": "Infinity",
                            "key": "50000.0-*"
                        }
                    ]
                },
                "from": {
                    "buckets": [
                        {
                            "doc_count": 10216,
                            "from": {
                                "source": "Music123",
                                "host": "http://www.music123.com"
                            },
                            "key": "1"
                        },
                        {
                            "doc_count": 10637,
                            "from": {
                                "source": "Sweetwater",
                                "host": "http://www.sweetwater.com"
                            },
                            "key": "2"
                        },
                        {
                            "doc_count": 3261,
                            "from": {
                                "source": "Guitar Center",
                                "host": "http://www.guitarcenter.com"
                            },
                            "key": "3"
                        },
                        {
                            "doc_count": 1345,
                            "from": {
                                "source": "Vintage King",
                                "host": "https://vintageking.com"
                            },
                            "key": "4"
                        },
                        {
                            "doc_count": 12949,
                            "from": {
                                "source": "Thomann",
                                "host": "https://www.thomann.de"
                            },
                            "key": "11"
                        },
                        {
                            "doc_count": 5168,
                            "from": {
                                "source": "B&H Photo Video",
                                "host": "https://www.bhphotovideo.com"
                            },
                            "key": "13"
                        },
                        {
                            "doc_count": 4303,
                            "from": {
                                "source": "Musiciansfriend",
                                "host": "http://www.musiciansfriend.com"
                            },
                            "key": "14"
                        },
                        {
                            "doc_count": 1498,
                            "from": {
                                "source": "Bax-Shop",
                                "host": "https://www.bax-shop.co.uk"
                            },
                            "key": "15"
                        }
                    ]
                },
                "category": {
                    "buckets": {
                        "A": [
                            {
                                "doc_count": 2271,
                                "key": "Accessories"
                            }
                        ],
                        "C": [
                            {
                                "doc_count": 489,
                                "key": "Cables + Plugs"
                            },
                            {
                                "doc_count": 358,
                                "key": "Cases"
                            },
                            {
                                "doc_count": 102,
                                "key": "Computer Audio"
                            }
                        ],
                        "D": [
                            {
                                "doc_count": 114,
                                "key": "DJ Equipment"
                            },
                            {
                                "doc_count": 379,
                                "key": "Drums + Percussion"
                            }
                        ],
                        "E": [
                            {
                                "doc_count": 713,
                                "key": "Effects + Signal Proc."
                            }
                        ],
                        "G": [
                            {
                                "doc_count": 33122,
                                "key": "Guitars and Basses"
                            }
                        ],
                        "K": [
                            {
                                "doc_count": 880,
                                "key": "Keys"
                            }
                        ],
                        "L": [
                            {
                                "doc_count": 137,
                                "key": "Lighting + Stage"
                            }
                        ]
                    }
                },
                "brand": {
                    "buckets": {
                        "#": [
                            {
                                "doc_count": 1,
                                "reviews": 0,
                                "top": false,
                                "name": "2nd SENSE",
                                "rating": 0,
                                "logo": null,
                                "key": "2nd SENSE"
                            },
                            {
                                "doc_count": 3,
                                "reviews": 0,
                                "top": false,
                                "name": "4ms Company",
                                "rating": 0,
                                "logo": null,
                                "key": "4ms Company"
                            },
                            {
                                "doc_count": 3,
                                "reviews": 0,
                                "top": false,
                                "name": "65 Amps",
                                "rating": 0,
                                "logo": null,
                                "key": "65 Amps"
                            },
                            {
                                "doc_count": 32,
                                "reviews": 0,
                                "top": false,
                                "name": "65amps",
                                "rating": 0,
                                "logo": null,
                                "key": "65amps"
                            },
                            {
                                "doc_count": 18,
                                "reviews": 0,
                                "top": false,
                                "name": "8DIO Productions",
                                "rating": 0,
                                "logo": null,
                                "key": "8DIO Productions"
                            }
                        ],
                        "A": [
                            {
                                "doc_count": 1,
                                "reviews": 0,
                                "top": false,
                                "name": "A Days Work",
                                "rating": 0,
                                "logo": null,
                                "key": "A Days Work"
                            },
                            {
                                "doc_count": 37,
                                "reviews": 0,
                                "top": false,
                                "name": "A Designs",
                                "rating": 0,
                                "logo": null,
                                "key": "A Designs"
                            },
                            {
                                "doc_count": 9,
                                "reviews": 201,
                                "top": true,
                                "name": "A Gift Republic",
                                "rating": 0.91,
                                "logo": "http://static.budee.com/yyren/image/220/14/974051.jpg",
                                "key": "A Gift Republic"
                            },
                            {
                                "doc_count": 11,
                                "reviews": 0,
                                "top": false,
                                "name": "A&S Crafted Products",
                                "rating": 0,
                                "logo": null,
                                "key": "A&S Crafted Products"
                            },
                            {
                                "doc_count": 5,
                                "reviews": 0,
                                "top": false,
                                "name": "AAS",
                                "rating": 0,
                                "logo": null,
                                "key": "AAS"
                            }
                        ]
                    }
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
                            "images": [
                                {
                                    "image": "http://static.budee.com/yyren/image/45/13/863554.jpg",
                                    "width": 300,
                                    "id": 863554,
                                    "height": 231
                                }
                            ],
                            "created": "2016-10-17T14:02:00.000Z",
                            "name": "Power Supply Al1012/E",
                            "category": [
                                {
                                    "en": "Keys",
                                    "cn": null
                                },
                                {
                                    "en": "Synthesizers",
                                    "cn": null
                                },
                                {
                                    "en": "Synthesizer Accessories",
                                    "cn": null
                                },
                                {
                                    "en": "PSU's for Synthesizers",
                                    "cn": null
                                }
                            ],
                            "brand": {
                                "reviews": 0,
                                "top": false,
                                "name": "Access",
                                "rating": 0,
                                "logo": "http://static.budee.com/yyren/image/220/14/974041.jpg"
                            },
                            "items": [
                                {
                                    "priceUnit": "$",
                                    "price": 44.68,
                                    "seedId": 11,
                                    "origin": "https://www.thomann.de/gb/access_netzteil_al1012_e.htm",
                                    "rating": 0.91,
                                    "descriptions": {
                                        "en": "Access Virus external Power supply AL1012/E for Virus TI/C; 12V DC; 1A; for Virus A, Virus B, Virus Classic, Virus Rack, Virus Rack XL, Virus C, Virus TI Desktop, Virus TI Snow, Virus TI2 Desktop.",
                                        "cn": "访问病毒外部电源AL1012 / E用于病毒TI / C; 12V DC; 1A;对于病毒A，病毒B，病毒经典，病毒机架，病毒机架XL，病毒C，病毒TI桌面，病毒TI雪，病毒TI2桌面。"
                                    }
                                }
                            ],
                            "tags": [
                                "191932"
                            ]
                        },
                        "_id": "32529",
                        "_score": 1.0740348
                    },
                    {
                        "_source": {
                            "images": [
                                {
                                    "image": "http://static.budee.com/yyren/image/96/17/1138761.jpg",
                                    "width": 1500,
                                    "id": 1138761,
                                    "height": 1500
                                },
                                {
                                    "image": "http://static.budee.com/yyren/image/96/17/1138762.jpg",
                                    "width": 1500,
                                    "id": 1138762,
                                    "height": 1500
                                }
                            ],
                            "created": "2016-11-25T02:48:07.000Z",
                            "name": "Access Virus TI2 Dark Star Limited Edition",
                            "category": [
                                {
                                    "en": "Keys",
                                    "cn": null
                                },
                                {
                                    "en": "Instruments",
                                    "cn": null
                                },
                                {
                                    "en": "Modular Synths",
                                    "cn": null
                                }
                            ],
                            "brand": {
                                "reviews": 0,
                                "top": false,
                                "name": "Access",
                                "rating": 0,
                                "logo": "http://static.budee.com/yyren/image/220/14/974041.jpg"
                            },
                            "items": [
                                {
                                    "priceUnit": "$",
                                    "price": 2104.94,
                                    "seedId": 15,
                                    "origin": "https://www.bax-shop.co.uk/synthesizer/access-virus-ti2-dark-star-limited-edition",
                                    "rating": null,
                                    "descriptions": {
                                        "en": "Acces now offers the Virus TI2 Dark Star in a limited black edition! It's a tweaked version of the TI Polar which can easily be integrated into your current stage or studio rig.",
                                        "cn": "Acces现在提供了一个有限的黑色版病毒TI2黑暗星！这是一个调整版的TI极地，可以很容易地集成到您当前的舞台或工作室钻机。"
                                    }
                                }
                            ],
                            "tags": [
                                "9000-0016-0219"
                            ]
                        },
                        "_id": "293841",
                        "_score": 1.0710148
                    }
                ],
                "total": 2321
            }
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
