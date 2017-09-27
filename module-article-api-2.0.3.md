FORMAT: 1A
HOST: http://192.168.1.138/

# topics

+ 2017年9月26日
    + 新增获取导航栏菜单集合API
    + 搜索API增加新的过滤参数, /topics/search?filter[navigation]={navigationId}, 根据导航标签过滤数据
        + 目前只有 navigationId=1 的有测试数据

文章主题模块2.0.3

## (GET)导航栏菜单 [/article/navigation/{id}]

+ Description
    + <del>include=children</del>
    + <del>include=parent</del>

+ Data
    + id (long) - 导航ID
    + parentId (long) - 父ID
    + title (string) - 导航标题
    + image (string) - icon

+ Parameters
    + id (long) - navigation id

### 查询颜色集合 [GET]

+ Response 200 (application/json)

        {
            "data": {
                "id": 1,
                "parentId": 0,
                "elasticQueryBuilderId": 2,
                "displayOrder": 1,
                "title": "麦克风",
                "image": "microphone",
                "children": [],
                "root": true
            }
        }

## (GET)导航栏菜单集合 [/article/navigation]

+ Description
    + 暂不支持排序srot先关参数, 后端强制控制
    + 暂不支持分页page相关参数, 后端强制控制
    
+ Data
    + id (long) - 导航ID
    + parentId (long) - 父ID
    + title (string) - 导航标题
    + image (string) - icon
    + children (array) - 子节点

### 查询导航菜单集合列表 [GET]

+ Response 200 (application/json)

        {
            "meta": {
                "totalPages": 1,
                "totalElements": 15,
                "size": 0,
                "number": 1,
                "numberOfElements": 15,
                "first": true,
                "last": true,
                "sort": null
            },
            "links": {
                "self": "/article/navigation?page[number]=1&page[size]=0",
                "first": "/article/navigation?page[number]=1&page[size]=0",
                "last": "/article/navigation?page[number]=1&page[size]=0"
            },
            "data": [
                {
                    "id": 1,
                    "parentId": 0,
                    "elasticQueryBuilderId": 1,
                    "displayOrder": 1,
                    "title": "麦克风",
                    "children": [
                        {
                            "id": 16,
                            "parentId": 1,
                            "elasticQueryBuilderId": 0,
                            "displayOrder": 1,
                            "title": "动圈麦克风",
                            "children": [],
                            "root": false
                        },
                        {
                            "id": 17,
                            "parentId": 1,
                            "elasticQueryBuilderId": 0,
                            "displayOrder": 2,
                            "title": "电容麦克风",
                            "children": [],
                            "root": false
                        },
                        {
                            "id": 18,
                            "parentId": 1,
                            "elasticQueryBuilderId": 0,
                            "displayOrder": 3,
                            "title": "微型无线麦克风",
                            "children": [],
                            "root": false
                        },
                        {
                            "id": 19,
                            "parentId": 1,
                            "elasticQueryBuilderId": 0,
                            "displayOrder": 4,
                            "title": "边界麦克风",
                            "children": [],
                            "root": false
                        },
                        {
                            "id": 20,
                            "parentId": 1,
                            "elasticQueryBuilderId": 0,
                            "displayOrder": 5,
                            "title": "鹅颈麦克风",
                            "children": [],
                            "root": false
                        },
                        {
                            "id": 21,
                            "parentId": 1,
                            "elasticQueryBuilderId": 0,
                            "displayOrder": 6,
                            "title": "麦克风插头支架",
                            "children": [],
                            "root": false
                        },
                        {
                            "id": 22,
                            "parentId": 1,
                            "elasticQueryBuilderId": 0,
                            "displayOrder": 7,
                            "title": "话放",
                            "children": [],
                            "root": false
                        }
                    ],
                    "root": true
                },
                {
                    "id": 2,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 2,
                    "title": "DJ设备",
                    "children": [],
                    "root": true
                },
                {
                    "id": 3,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 3,
                    "title": "舞台灯光",
                    "children": [],
                    "root": true
                },
                {
                    "id": 4,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 4,
                    "title": "吉他贝斯",
                    "children": [],
                    "root": true
                },
                {
                    "id": 5,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 5,
                    "title": "套鼓",
                    "children": [],
                    "root": true
                },
                {
                    "id": 6,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 6,
                    "title": "键盘与合成器",
                    "children": [],
                    "root": true
                },
                {
                    "id": 7,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 7,
                    "title": "交响与乐队乐器",
                    "children": [],
                    "root": true
                },
                {
                    "id": 8,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 8,
                    "title": "中国民族乐器",
                    "children": [],
                    "root": true
                },
                {
                    "id": 9,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 9,
                    "title": "乐器箱包及配件",
                    "children": [],
                    "root": true
                },
                {
                    "id": 10,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 10,
                    "title": "耳机",
                    "children": [],
                    "root": true
                },
                {
                    "id": 11,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 11,
                    "title": "调音台与周边设备",
                    "children": [],
                    "root": true
                },
                {
                    "id": 12,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 12,
                    "title": "扬声器",
                    "children": [],
                    "root": true
                },
                {
                    "id": 13,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 13,
                    "title": "MIDI设备（电脑音乐）",
                    "children": [],
                    "root": true
                },
                {
                    "id": 14,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 14,
                    "title": "软件",
                    "children": [],
                    "root": true
                },
                {
                    "id": 15,
                    "parentId": 0,
                    "elasticQueryBuilderId": 0,
                    "displayOrder": 15,
                    "title": "附件（配件）",
                    "children": [],
                    "root": true
                }
            ]
        }


        
