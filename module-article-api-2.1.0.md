FORMAT: 1A
HOST: http://192.168.1.138/

# 米饭平台2.1.0

+ 2017年11月7日
    + 增加调度中心管理相关API
        + 增加调度任务
        + 删除调度任务
        + 修改调度任务(运行任务, 停止任务, 修改定时表达式等)
        + 查询任务列表与状态等信息

+ 2017年11月1日
    + 新增类别相关API

+ 2017年10月31日
    + 新增种子相关API
    
## (POST|GET)爬虫种子网站 [/article/seeds]

+ Description
    + [MUST] ROLE_ADMIN

+ Field
    + url (string) - 种子URL
    + source (string) - 种子来源名称/简称
    + conf (string) - 种子配置, JSON格式的大文本
    + name (string, nullable) - 类型名称 e.g products产品 | brands品牌
    + agencyIp (string, nullable) - 代理IP
    + agencyIpPort (string, nullable) - 代理端口
    + charset (string, nullable) - 字符集你
    + description (string, nullable) - 种子描述
    + language (string) - 语言 1:中文, 2英文

+ Meta
    + number (int) - 当前页
    + size (int) - 每页大小
    + numberOfElements (int) - 当前页有多少记录
    + last (boolean) - 是否是最后一页
    + totalPages (int) - 总页数
    + sort (object) - 排序相关信息
    + first (boolean) - 是否是第一页
    + totalElements - 总记录数

### 新增种子 [POST]

+ Request (application/json)

        {
            "data": {
                "url": "https://www.sweetwater.com/store/detail/KM183",
                "source": "Sweetwater",
                "conf": "{}",
                "name": "products",
                "agencyIp": "192.168.1.234",
                "agencyIpPort": 9000,
                "charset": "UTF-8",
                "description": "甜水网",
                "language": 1
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /article/seeds/1

    + Body

            {
                "data": {
                    "id": 1,
                    "type": "seeds"
                }
            }
            
+ Response 204 (application/json)
        
+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "some code",
                    "title": "Bad Request",
                    "detail": "格式错误",
                    "source": {
                        "pointer": "{class} -> {field}"
                    }
                }
            ]
        }

### 查询种子列表 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "totalPages": 8,
                "totalElements": 74,
                "size": 10,
                "number": 1,
                "numberOfElements": 10,
                "first": true,
                "last": false,
                "sort": null
            },
            "links": {
                "self": "/article/seeds?page[number]=1&page[size]=10",
                "first": "/article/seeds?page[number]=1&page[size]=10",
                "next": "/article/seeds?page[number]=2&page[size]=10",
                "last": "/article/seeds?page[number]=8&page[size]=10"
            },
            "data": [
                {
                    "id": 1,
                    "enabled": 0,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2016-06-16 10:29:36",
                    "modified": "2017-10-31 09:57:01",
                    "url": "http://www.music123.com/browse/shopByBrand/shopByBrand.jsp",
                    "source": "Music123",
                    "conf": "",
                    "name": "products",
                    "description": "Music123",
                    "rank": 500,
                    "updateRate": 1,
                    "language": 2
                },
                {
                    "id": 2,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "url": "https://www.sweetwater.com/store/detail/KM183",
                    "source": "Sweetwater",
                    "conf": "",
                    "name": "products",
                    "description": "甜水网",
                    "rank": 0,
                    "updateRate": 1,
                    "language": 2
                },
                {
                    "id": 3,
                    "enabled": 0,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2016-06-22 10:29:36",
                    "modified": "2016-06-22 10:29:36",
                    "url": "http://www.guitarcenter.com",
                    "source": "Guitar Center",
                    "conf": "",
                    "name": "products",
                    "description": "GuitarCenter",
                    "rank": 400,
                    "updateRate": 1,
                    "language": 2
                }
            ]
        }

## (GET|DELETE|PATCH)爬虫种子网站 [/article/seeds/{id}]

+ Description
    + [MUST] ROLE_ADMIN

+ Parameters
    + id (long) - 种子ID

### 查询种子详情 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 1,
                "enabled": 0,
                "creator": 0,
                "modifier": 0,
                "created": "2016-06-16 10:29:36",
                "modified": "2017-10-31 09:57:01",
                "url": "http://www.music123.com/browse/shopByBrand/shopByBrand.jsp",
                "source": "Music123",
                "conf": "",
                "name": "products",
                "description": "Music123",
                "rank": 500,
                "updateRate": 1,
                "language": 2
            }
        }

### 删除种子 [DELETE]

+ Response 204 (application/json)

### 修改种子[PATCH]

+ Request (application/json)

        {
            "data": {
                "url": "https://www.sweetwater.com/store/detail/KM183",
                "source": "Sweetwater",
                "conf": "{}",
                "name": "products",
                "agencyIp": "192.168.1.234",
                "agencyIpPort": 9000,
                "charset": "UTF-8",
                "description": "甜水网",
                "language": 1
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)

## (POST|GET)类别 [/article/forumCategories]

+ Description
    + [MUST] ROLE_ADMIN

+ Field
    + forumId (long) - forum ID
    + parentId (long) - 父节点ID
    + title (string) - 类别名称
    + displayOrder (int, nullable) - 排序字段, 可选

+ Meta
    + number (int) - 当前页
    + size (int) - 每页大小
    + numberOfElements (int) - 当前页有多少记录
    + last (boolean) - 是否是最后一页
    + totalPages (int) - 总页数
    + sort (object) - 排序相关信息
    + first (boolean) - 是否是第一页
    + totalElements - 总记录数

### 新增类别 [POST]

+ Request (application/json)

        {
            "data": {
                "forumId": 1,
                "parentId": 2147,
                "title": "吉他和贝斯",
                "displayOrder": 0
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /article/forumCategories/1

    + Body

            {
                "data": {
                    "id": 1,
                    "type": "forumCategories"
                }
            }
            
+ Response 204 (application/json)
        
+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "some code",
                    "title": "Bad Request",
                    "detail": "格式错误",
                    "source": {
                        "pointer": "{class} -> {field}"
                    }
                }
            ]
        }

### 查询类别列表 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "totalPages": 215,
                "totalElements": 2143,
                "size": 10,
                "number": 1,
                "numberOfElements": 10,
                "first": true,
                "last": false,
                "sort": null
            },
            "links": {
                "self": "/article/forumCategories?page[number]=1&page[size]=10",
                "first": "/article/forumCategories?page[number]=1&page[size]=10",
                "next": "/article/forumCategories?page[number]=2&page[size]=10",
                "last": "/article/forumCategories?page[number]=215&page[size]=10"
            },
            "data": [
                {
                    "id": 1,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 11,
                    "created": "2017-10-31 17:07:52",
                    "modified": "2017-11-01 13:16:01",
                    "forumId": 1,
                    "rootId": 1,
                    "parentId": 0,
                    "title": "Accessories",
                    "path": "1",
                    "depth": 1,
                    "leaf": 0,
                    "displayOrder": 0
                },
                {
                    "id": 2,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-10-31 17:07:52",
                    "modified": "2017-10-31 17:07:52",
                    "forumId": 1,
                    "rootId": 1,
                    "parentId": 1,
                    "title": "Accessories for Mics",
                    "path": "1.2",
                    "depth": 2,
                    "leaf": 0,
                    "displayOrder": 0
                },
                {
                    "id": 3,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-10-31 17:07:52",
                    "modified": "2017-11-01 11:46:46",
                    "forumId": 1,
                    "rootId": 1,
                    "parentId": 2,
                    "title": "Booms & Handles",
                    "path": "1.2.3",
                    "depth": 3,
                    "leaf": 1,
                    "displayOrder": 0
                }
            ]
        }        

## (GET|DELETE|PATCH)类别 [/article/forumCategories/{id}]

+ Description
    + [MUST] ROLE_ADMIN
    
+ Field
    + id (long) - 类别ID标识符
    + forumId (long) - forum ID
    + rootId (long) - 根节点ID
    + parentId (long) - 父节点ID
    + title (string) - 类别名称
    + path (string) - 节点路径
    + depth (int) - 深度
    + leaf (int) - 是否是叶子节点 0:否, 1:是
    + displayOrder (int) - 排序字段

+ Parameters
    + id (long) - 类别ID

### 查询类别详情 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 1,
                "enabled": 1,
                "creator": 0,
                "modifier": 11,
                "created": "2017-10-31 17:07:52",
                "modified": "2017-11-01 13:16:01",
                "forumId": 1,
                "rootId": 1,
                "parentId": 0,
                "title": "Accessories",
                "path": "1",
                "depth": 1,
                "leaf": 0,
                "displayOrder": 0
            }
        }

### 删除类别 [DELETE]

+ Response 204 (application/json)

### 修改类别[PATCH]

+ Request (application/json)

        {
            "data": {
                "id": 1,
                "enabled": 1,
                "title": "Accessories",
                "displayOrder": 0
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)

## (POST|GET)双语词典 [/article/wordDictionary]

+ Description
    + [MUST] ROLE_ADMIN

+ Field
    + wordEn (string) - 英文词
    + wordEn (string) - 中文词

+ Meta
    + number (int) - 当前页
    + size (int) - 每页大小
    + numberOfElements (int) - 当前页有多少记录
    + last (boolean) - 是否是最后一页
    + totalPages (int) - 总页数
    + sort (object) - 排序相关信息
    + first (boolean) - 是否是第一页
    + totalElements - 总记录数

### 新增双语词典 [POST]

+ Request (application/json)

        {
            "data": {
                "wordEn": "accessories",
                "wordCn": "配件"
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /article/wordDictionary/1

    + Body

            {
                "data": {
                    "id": 1,
                    "type": "wordDictionary"
                }
            }
            
+ Response 204 (application/json)
        
+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "some code",
                    "title": "Bad Request",
                    "detail": "格式错误",
                    "source": {
                        "pointer": "{class} -> {field}"
                    }
                }
            ]
        }

### 查询双语词典列表 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "totalPages": 270,
                "totalElements": 2694,
                "size": 10,
                "number": 1,
                "numberOfElements": 10,
                "first": true,
                "last": false,
                "sort": null
            },
            "links": {
                "self": "/article/wordDictionary?page[number]=1&page[size]=10",
                "first": "/article/wordDictionary?page[number]=1&page[size]=10",
                "next": "/article/wordDictionary?page[number]=2&page[size]=10",
                "last": "/article/wordDictionary?page[number]=270&page[size]=10"
            },
            "data": [
                {
                    "id": 1,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-10-31 18:30:30",
                    "modified": "2017-10-31 18:30:30",
                    "wordEn": "accessories",
                    "wordCn": "配件"
                },
                {
                    "id": 2,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-10-31 18:30:31",
                    "modified": "2017-10-31 18:30:31",
                    "wordEn": "accessories for mics",
                    "wordCn": "麦克风配件"
                },
                {
                    "id": 3,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 0,
                    "created": "2017-10-31 18:30:31",
                    "modified": "2017-10-31 18:30:31",
                    "wordEn": "booms & handles",
                    "wordCn": "吊杆和手柄配件"
                }
            ]
        }
        
## (GET|DELETE|PATCH)双语词典  [/article/wordDictionary/{id}]

+ Description
    + [MUST] ROLE_ADMIN

+ Parameters
    + id (long) - 标识符ID

### 查询双语词典详情 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 1,
                "enabled": 1,
                "creator": 0,
                "modifier": 0,
                "created": "2017-10-31 18:30:30",
                "modified": "2017-10-31 18:30:30",
                "wordEn": "accessories",
                "wordCn": "配件"
            }
        }

### 删除双语词典 [DELETE]

+ Response 204 (application/json)

### 修改双语词典 [PATCH]

+ Request (application/json)

        {
            "data": {
                "wordEn": "accessories",
                "wordCn": "配件"
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)

## (PATCH)主题 [/topics/{id}]

+ Description
    + [MUST] ROLE_ADMIN
    + 用于修改影响搜索结果排名的助推数
    + 用于训练时选择的样本集合
    + 用户人工标注类别
    + 一下三个参数通常都是单独在API里进行修改的

+ Field
    + boost (double) - 搜索引擎助推数, 值越高排名越靠前
    + trainSample (int) - 是否加入到训练样本集中, 0:否, 1:是
    + categoryId (long) - 人工标注的分类ID

### 修改主题[PATCH]

+ Request (application/json)

        {
            "data": {
                "boost": 1,
                "trainSample": 1,
                "categoryId": 1
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)

## (POST|GET) 调度任务 [/article/quartzJobs]

+ Description
    + [MUST] ROLE_ADMIN
    + 修改调度任务执行状态
        + RUNNABLE 

+ Field
    + id (long) - 唯一标识符
    + jobName (string) - 任务名称
    + jobClass (string) - 任务策略类全限定名称 
    + jobData (string, nullable) - json格式的任务参数, 不同的策略类有不同的参数
    + triggerName (string) - 触发器名称
    + triggerCronExpression (string) - cron表达式
    + description (string) - 描述,
    + jobStatus (enum) - NEW:新建, SUSPEND:暂停, RUNNABLE:运行中, TERMINATED:终止
        + 新增任务时, 默认都是NEW状态
        + 当修改任务的状态为RUNNABLE时, 加入自动调度管理中心
        + 当修改任务的状态为非RUNNABLE时, 撤销在自动调度中心的任务
        + 前端要有一个控制每个任务状态的按钮以管理任务
        + SUSPEND状态暂时不同考虑, 该枚举为扩展类型
    + jobStatusValue (string) - 对应jobStatus的中文描述值
    + version (int) - 版本号, 修改数据时需要该参数以防止并发修改带来的问题
    + auto (int) - 是否加入到应用启动自动部署中, 0:否, 1:是

+ Meta
    + number (int) - 当前页
    + size (int) - 每页大小
    + numberOfElements (int) - 当前页有多少记录
    + last (boolean) - 是否是最后一页
    + totalPages (int) - 总页数
    + sort (object) - 排序相关信息
    + first (boolean) - 是否是第一页
    + totalElements - 总记录数

### 新增调度任务 [POST]

+ Request (application/json)

        {
            "data": {
                "jobName": "job-3",
                "jobClass": "com.mifan.article.job.SpiderJob",
                "jobData": "{\"seeds\":[1,2,3]}",
                "triggerName": "job-3-trigger",
                "triggerCronExpression": "0/8 * * * * ?",
                "description": "爬虫任务"
            }
        }

+ Response 201 (application/json)

    + Headers

            Location: /article/quartzJobs/1

    + Body

            {
                "data": {
                    "id": 1,
                    "type": "quartzJobs"
                }
            }
            
+ Response 204 (application/json)
        
+ Response 400 (application/json)

        {
            "errors": [
                {
                    "status": "400",
                    "code": "some code",
                    "title": "Bad Request",
                    "detail": "格式错误",
                    "source": {
                        "pointer": "{class} -> {field}"
                    }
                }
            ]
        }

### 查询调度任务列表 [GET]

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
                "sort": null
            },
            "links": {
                "self": "/article/quartzJobs?page[number]=1&page[size]=10",
                "first": "/article/quartzJobs?page[number]=1&page[size]=10",
                "last": "/article/quartzJobs?page[number]=1&page[size]=10"
            },
            "data": [
                {
                    "id": 1,
                    "enabled": 1,
                    "creator": 0,
                    "modifier": 11,
                    "created": "2017-11-02 16:54:41",
                    "modified": "2017-11-07 10:05:11",
                    "jobStatus": "RUNNABLE",
                    "jobStatusValue": "运行中",
                    "jobGroup": "DEFAULT",
                    "jobName": "job-1",
                    "jobClass": "com.mifan.article.job.SpiderJob",
                    "jobData": "{\"timeout\":8}",
                    "triggerGroup": "DEFAULT",
                    "triggerName": "job-1-trigger",
                    "triggerCronExpression": "0/8 * * * * ?",
                    "version": 22,
                    "auto": 1
                },
                {
                    "id": 2,
                    "enabled": 1,
                    "creator": 11,
                    "modifier": 11,
                    "created": "2017-11-07 11:06:13",
                    "modified": "2017-11-07 11:06:13",
                    "jobStatus": "NEW",
                    "jobStatusValue": "新建",
                    "jobGroup": "DEFAULT",
                    "jobName": "job-2",
                    "jobClass": "com.mifan.article.job.SpiderJob",
                    "jobData": "{\"timeout\":8}",
                    "triggerGroup": "DEFAULT",
                    "triggerName": "job-2-trigger",
                    "triggerCronExpression": "0/8 * * * * ?",
                    "description": "爬虫任务",
                    "version": 0,
                    "auto": 0
                },
                {
                    "id": 3,
                    "enabled": 1,
                    "creator": 11,
                    "modifier": 11,
                    "created": "2017-11-07 11:07:04",
                    "modified": "2017-11-07 11:07:04",
                    "jobStatus": "NEW",
                    "jobStatusValue": "新建",
                    "jobGroup": "DEFAULT",
                    "jobName": "job-3",
                    "jobClass": "com.mifan.article.job.SpiderJob",
                    "jobData": "{\"timeout\":8}",
                    "triggerGroup": "DEFAULT",
                    "triggerName": "job-3-trigger",
                    "triggerCronExpression": "0/8 * * * * ?",
                    "description": "爬虫任务",
                    "version": 0,
                    "auto": 0
                }
            ]
        }
        
## (GET|DELETE|PATCH) 调度中心任务 [/article/quartzJobs/{id}]

+ Description
    + [MUST] ROLA_ADMIN

+ Parameters
    + id (long) - 唯一标识符ID

### 查询调度任务详情 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 1,
                "enabled": 1,
                "creator": 0,
                "modifier": 11,
                "created": "2017-11-02 16:54:41",
                "modified": "2017-11-07 10:05:11",
                "jobStatus": "RUNNABLE",
                "jobGroup": "DEFAULT",
                "jobName": "job-1",
                "jobClass": "com.mifan.article.job.SpiderJob",
                "jobData": "{\"timeout\":8}",
                "triggerGroup": "DEFAULT",
                "triggerName": "job-1-trigger",
                "triggerCronExpression": "0/8 * * * * ?",
                "version": 22,
                "auto": 1,
                "jobStatusValue": "运行中"
            }
        }

### 删除调度任务 [DELETE]

+ Response 204 (application/json)

### 修改调度任务 [PATCH]

+ Request (application/json)

        {
            "data": {
                "id": 1,
                "enabled": 1,
                "version": 21,
                "triggerCronExpression": "0/8 * * * * ?",
                "jobData": "{\"timeout\":8}",
                "jobStatus": "RUNNABLE"
            }
        }
        
+ Response 200 (application/json)

+ Response 204 (application/json)
