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
