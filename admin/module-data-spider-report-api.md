## 爬虫爬取数据统计

+ Data
    + seedId (Integer) - 网站种子ID
    + name (String) - 名字（在seeds表中设置）
    + description (String) - 网站描述 （在seeds表中设置）
    + counts (Long) - 爬取topic数据合计

## 爬虫爬取数据统计详情 [GET] /spider/statistics?filter[priortime]=2017-01-09%2023:11:45&filter[latertime]=2017-11-08%2023:11:45&page[number]=1&page[size]=5
+ Description

+ Parameters
    + filter[priortime] 必填,起始时间
    + filter[latertime] 必填,结束时间
    + page[number] 页数
    + page[size]  每页多少条记录
+ Response 200 (application/json)

    
    {
        "meta": 
        {
            "totalPages": 13,
            "totalElements": 63,
            "size": 5,
            "number": 1,
            "numberOfElements": 5,
            "first": true,
            "last": false,
            "sort": null
        },
        "links": 
        {
            "self": "/spider/statistics?filter[priortime]=2017-01-09 23:11:45&filter[latertime]=2017-11-08 23:11:45&page[number]=1&page[size]=5",
            "first": "/spider/statistics?filter[priortime]=2017-01-09 23:11:45&filter[latertime]=2017-11-08 23:11:45&page[number]=1&page[size]=5",
            "next": "/spider/statistics?filter[priortime]=2017-01-09 23:11:45&filter[latertime]=2017-11-08 23:11:45&page[number]=2&page[size]=5",
            "last": "/spider/statistics?filter[priortime]=2017-01-09 23:11:45&filter[latertime]=2017-11-08 23:11:45&page[number]=13&page[size]=5"
        },
        "data": 
        [
            {
                "seedId": 1,
                "name": "products",
                "description": "Music123",
                "counts": 5724
            },
            {
                "seedId": 2,
                "name": "products",
                "description": "甜水网",
                "counts": 3445
            },
            {
                "seedId": 3,
                "name": "products",
                "description": "GuitarCenter",
                "counts": 1555
            },
            {
                "seedId": 4,
                "name": "products",
                "description": "VintageKing",
                "counts": 143
            },
            {
                "seedId": 5,
                "name": "products",
                "description": "MartinAudio",
                "counts": 14
            }
        ]
    }


## 通过SeedId获取爬取的数据详情
+ Description

+ Parameters
    + filter[priortime] 必填,起始时间
    + filter[latertime] 必填,结束时间
    + page[number] 页数
    + page[size]  每页多少条记录
+ Response 200 (application/json)
+ Data
    + id(Long) - id
    + topicId(Long) - topicId
    + seedId(Long) - 种子ID    
    + origin(String) - 被爬取的url