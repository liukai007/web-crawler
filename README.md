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
    
+ Summary
    + 统一所有返回的列表数据只限定在两种格式内:一种是搜索引擎的hits格式, 另一个是PageImpl格式（包括从cache中查找, 数据库查找等, 都要封装成这种格式）.

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
    + 轮播图/links?keys=type&values=1&properties=sort&asc=true&fields=id,title,description,image,href
    + 导航栏/links?keys=type&values=2&properties=sort&asc=true&fields=id,title,description,image,href

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
    
## 产品参数对比接口 [/product/compare?art[]={ids}]

+ Response
    + header - 头部信息, 包含产品详细的信息
    + body - 产品参数
    
+ Parameters
    + ids (integer) - 数组, 待对比的产品ID集合 e.g : 1,2,3

### 查找产品对比信息

+ Response 200 (application/json)

		{
		    "header": {
			"Model": [
			    {
				"_source": {
				    "images": [
					{
					    "image": "http://static.budee.com/yyren/image/6/1748.jpg",
					    "width": 1800,
					    "id": 1748,
					    "height": 684
					},
					{
					    "image": "http://static.budee.com/yyren/image/6/1749.jpg",
					    "width": 1800,
					    "id": 1749,
					    "height": 1524
					},
					{
					    "image": "http://static.budee.com/yyren/image/6/1750.jpg",
					    "width": 1800,
					    "id": 1750,
					    "height": 854
					}
				    ],
				    "name": "Access Virus TI2 Keyboard",
				    "category": [
					"Keys",
					"Synths / Modules"
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
					    "seedId": "2",
					    "origin": "http://www.sweetwater.com/store/detail/VirusTI2Key",
					    "rating": "rating",
					    "description": "61-key Analog Modeling Synthesizer and 24-bit/192kHz Audio/MIDI Interface",
					    "from": {
						"source": "Sweetwater",
						"host": "http://www.sweetwater.com"
					    }
					}
				    ]
				},
				"_id": "2"
			    },
			    {
				"_source": {
				    "images": [
					{
					    "image": "http://static.budee.com/yyren/image/6/1745.jpg",
					    "width": 1800,
					    "id": 1745,
					    "height": 500
					},
					{
					    "image": "http://static.budee.com/yyren/image/6/1746.jpg",
					    "width": 1600,
					    "id": 1746,
					    "height": 794
					},
					{
					    "image": "http://static.budee.com/yyren/image/6/1747.jpg",
					    "width": 1600,
					    "id": 1747,
					    "height": 1151
					}
				    ],
				    "name": "Access Virus TI2 Desktop",
				    "category": [
					"Keys",
					"Synths / Modules"
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
					    "price": 2185,
					    "seedId": "2",
					    "origin": "http://www.sweetwater.com/store/detail/VirusTI2Desk",
					    "rating": "rating",
					    "description": "Analog Modeling Desktop Synthesizer and 24-bit/192kHz Audio/MIDI Interface",
					    "from": {
						"source": "Sweetwater",
						"host": "http://www.sweetwater.com"
					    }
					}
				    ]
				},
				"_id": "1"
			    }
			]
		    },
		    "body": {
			"Sound Engine Type(s)": [
			    "Analog Modeling",
			    "Analog Modeling"
			],
			"Number of Keys": [
			    "61",
			    "-"
			],
			"Type of Keys": [
			    "Semi-weighted",
			    "-"
			],
			"Other Controllers": [
			    "Pitchbend, Mod Wheel",
			    "-"
			],
			"Polyphony": [
			    "20 Notes-90 Notes (Depending On the Patch)",
			    "20 Notes-90 Notes (Depending On the Patch)"
			],
			"Number of Presets": [
			    "512 RAM Patches, 3328 ROM Sounds",
			    "512 RAM Patches, 3328 ROM Sounds"
			],
			"Number of Effects": [
			    "192",
			    "192"
			],
			"Effects Types": [
			    "Reverb, Chorus, Delay, Phaser, EQ, Ring Mod",
			    "Reverb, Chorus, Delay, Phaser, EQ, Ring Mod"
			],
			"Arpeggiator": [
			    "Yes",
			    "Yes"
			],
			"Audio Inputs": [
			    "2 x TS",
			    "-"
			],
			"Audio Outputs": [
			    "6 x TS, 1 x TRS (Headphones)",
			    "-"
			],
			"Digital Inputs": [
			    "1 x S/PDIF",
			    "1 x S/PDIF"
			],
			"Digital Outputs": [
			    "1 x S/PDIF",
			    "1 x S/PDIF"
			],
			"USB": [
			    "1 x Type B",
			    "1 x Type B"
			],
			"MIDI I/O": [
			    "In/Out/Thru/USB",
			    "In/Out/Thru/USB"
			],
			"Pedal Inputs": [
			    "1 x Hold, 1 x Control",
			    "-"
			],
			"Power Supply": [
			    "Included",
			    "Power Supply Included"
			],
			"Height": [
			    "4.6\"",
			    "3.2\""
			],
			"Width": [
			    "39.2\"",
			    "18.5\""
			],
			"Depth": [
			    "14.6\"",
			    "7.4\""
			],
			"Weight": [
			    "30.6 lbs.",
			    "7.4 lbs."
			],
			"Manufacturer Part Number": [
			    "Virus TI2 Key",
			    "Virus TI2 Desk"
			],
			"Analog Inputs": [
			    "-",
			    "2 x TS"
			],
			"Analog Outputs": [
			    "-",
			    "6 x TS, 1 x TRS (Headphones)"
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
    + hits.hits[]._source.rawContent - 富文本内容
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
                  "priceUnit": "CNY",
                  "rating": 0.80,
                  "images": [
                    {
                      "image": "http://static.budee.com/yyren/image/201610/10/1.jpg?w=80",
                      "alt": "Pops' Bass Rosin Bass Rosin",
                      "width": 180,
                      "id": 1389,
                      "height": 180,
                      "sizes":[
                        80,
                        220,
                        400,
                        800,
                        1200
                      ],
                      "distance": {
                        "000000": 0.001625050938040724,
                        "0000FF": 0.0017121483481907281,
                        "FF0000": 0.002108896256451791,
                        "FF00FF": 0.002219442528339659,
                        "00FF00": 0.0018475207605581057,
                        "00FFFF": 0.002146904123589382,
                        "FFFF00": 0.0026933843973544424,
                        "FFFFFF": 0.005582458482860849,
                      }
                    }
                  ],
                  "features":[
                        {
                            "_name": "Sound Engine Type(s)",
                            "_value": "Analog Modeling"
                        },
                        {
                            "_name": "Polyphony",
                            "_value": "20 Notes-90 Notes (Depending On the Patch)"
                        },
                        {
                            "_name": "Number of Presets",
                            "_value": "512 RAM Patches, 3328 ROM Sounds"
                        },
                        {
                            "_name": "Number of Effects",
                            "_value": "192"
                        },
                        {
                            "_name": "Effects Types",
                            "_value": "Reverb, Chorus, Delay, Phaser, EQ, Ring Mod"
                        },
                        {
                            "_name": "Arpeggiator",
                            "_value": "Yes"
                        },
                        {
                            "_name": "Analog Inputs",
                            "_value": "2 x TS"
                        },
                        {
                            "_name": "Analog Outputs",
                            "_value": "6 x TS, 1 x TRS (Headphones)"
                        },
                        {
                            "_name": "Digital Inputs",
                            "_value": "1 x S/PDIF"
                        },
                        {
                            "_name": "Digital Outputs",
                            "_value": "1 x S/PDIF"
                        },
                        {
                            "_name": "MIDI I/O",
                            "_value": "In/Out/Thru/USB"
                        },
                        {
                            "_name": "USB",
                            "_value": "1 x Type B"
                        },
                        {
                            "_name": "Height",
                            "_value": "3.2\""
                        },
                        {
                            "_name": "Width",
                            "_value": "18.5\""
                        },
                        {
                            "_name": "Depth",
                            "_value": "7.4\""
                        },
                        {
                            "_name": "Weight",
                            "_value": "7.4 lbs."
                        },
                        {
                            "_name": "Power Supply",
                            "_value": "Power Supply Included"
                        },
                        {
                            "_name": "Manufacturer Part Number",
                            "_value": "Virus TI2 Desk"
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
    + hits.hits[].highlight.content[] - 根据q选择相关度最高的n段句子, 并使用<span class='highlight'>{key}</span>包围关键词
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
                  "items.content": [
                    "Overview Pops' <span class='highlight'>Bass</span> Rosin is packaged in a convenient resealable red tub."
                  ]
                },
                "_source": {
                  "images": [
                    {
                      "image": "http://static.budee.com/yyren/image/201610/10/1.jpg?w=80",
                      "alt": "Pops' Bass Rosin Bass Rosin",
                      "width": 180,
                      "id": 1389,
                      "height": 180,
                      "distance": {
                        "000000": 0.001625050938040724,
                        "0000FF": 0.0017121483481907281,
                        "FF0000": 0.002108896256451791,
                        "FF00FF": 0.002219442528339659,
                        "00FF00": 0.0018475207605581057,
                        "00FFFF": 0.002146904123589382,
                        "FFFF00": 0.0026933843973544424,
                        "FFFFFF": 0.005582458482860849,
                      }
                    }
                  ],
                  "features":[
                        {
                            "_name": "Sound Engine Type(s)",
                            "_value": "Analog Modeling"
                        },
                        {
                            "_name": "Polyphony",
                            "_value": "20 Notes-90 Notes (Depending On the Patch)"
                        },
                        {
                            "_name": "Number of Presets",
                            "_value": "512 RAM Patches, 3328 ROM Sounds"
                        },
                        {
                            "_name": "Number of Effects",
                            "_value": "192"
                        },
                        {
                            "_name": "Effects Types",
                            "_value": "Reverb, Chorus, Delay, Phaser, EQ, Ring Mod"
                        },
                        {
                            "_name": "Arpeggiator",
                            "_value": "Yes"
                        },
                        {
                            "_name": "Analog Inputs",
                            "_value": "2 x TS"
                        },
                        {
                            "_name": "Analog Outputs",
                            "_value": "6 x TS, 1 x TRS (Headphones)"
                        },
                        {
                            "_name": "Digital Inputs",
                            "_value": "1 x S/PDIF"
                        },
                        {
                            "_name": "Digital Outputs",
                            "_value": "1 x S/PDIF"
                        },
                        {
                            "_name": "MIDI I/O",
                            "_value": "In/Out/Thru/USB"
                        },
                        {
                            "_name": "USB",
                            "_value": "1 x Type B"
                        },
                        {
                            "_name": "Height",
                            "_value": "3.2\""
                        },
                        {
                            "_name": "Width",
                            "_value": "18.5\""
                        },
                        {
                            "_name": "Depth",
                            "_value": "7.4\""
                        },
                        {
                            "_name": "Weight",
                            "_value": "7.4 lbs."
                        },
                        {
                            "_name": "Power Supply",
                            "_value": "Power Supply Included"
                        },
                        {
                            "_name": "Manufacturer Part Number",
                            "_value": "Virus TI2 Desk"
                        }
                    ],
                  "created": "2016-08-10T04:53:06.000Z",
                  "items": [
                        {
                            "rating": 0.80,
                            "priceUnit": "$", 
                            "price": 17.98, 
                            "seedId": 11, 
                            "origin": "https://www.thomann.de/gb/schott_bassmethode_der_einfache_weg.htm", 
                            "description": "Schott Bass-Methode by Ed Friedland: An easy bass tutor for beginners; with CD; Over 200 songs, riffs, exercises, tuning, playing positions, shuffle rhythm, major and minor scales, classic blues lines; in German language", 
                            "from": {
                              "source": "Thomann", 
                              "host": "https://www.thomann.de"
                            }
                          }
                    ],
                  "name": "Pops' Bass Rosin Bass Rosin  ",
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
                  "logo":"http://static.budee.com/yyren/image/1.jpg",
                  "key": "Hal Leonard",
                  "reviews": 999,
                  "rating": 0.86,
                  "top": true
                },
                {
                  "doc_count": 549,
                  "logo":"http://static.budee.com/yyren/image/1.jpg",
                  "key": "Gator",
                  "reviews": 543,
                  "rating": 0.03,
                  "top": false
                },
                {
                  "doc_count": 59,
                  "logo":"http://static.budee.com/yyren/image/1.jpg",
                  "key": "Epiphone",
                  "reviews": 33,
                  "rating": 0.99,
                  "top": false
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
                  "rating": 0.80,
                  "images": [
                    {
                      "image": "http://static.budee.com/yyren/image/201610/10/1.jpg?w=80",
                      "alt": "Pops' Bass Rosin Bass Rosin",
                      "width": 180,
                      "id": 1389,
                      "height": 180,
                      "sizes":[
                        80,
                        220,
                        400,
                        800,
                        1200
                      ],
                      "distance": {
                        "000000": 0.001625050938040724,
                        "0000FF": 0.0017121483481907281,
                        "FF0000": 0.002108896256451791,
                        "FF00FF": 0.002219442528339659,
                        "00FF00": 0.0018475207605581057,
                        "00FFFF": 0.002146904123589382,
                        "FFFF00": 0.0026933843973544424,
                        "FFFFFF": 0.005582458482860849,
                      }
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
