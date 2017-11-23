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
```
        {
            "data": {
                "boost": 1,
                "trainSample": 1,
                "categoryId": 1
            }
        }
 ```       
+ Response 200 (application/json)

+ Response 204 (application/json)

## (POST)新增/重建索引 [/article/topics/index}]

+ Description
    + [MUST] ROLE_ADMIN

+ Parameters
    + id (long) - topic ID
    + array (long) - 需要重建索引的topic ID数组
    + start 需要重建索引的起始topic ID 须于end搭配使用
    + end 需要重建索引的结束topic ID 须于start搭配使用
    
### 新增索引 [POST]

+ Request (application/json)
```
        {
            "data": {
                "id": 1,
                "array": [
                    1,
                    2
                ],
                "start": 1,
                "end": 2
            }
        }
```
+ Response 200 (application/json)
```
        {
            "data": "some information"
        }
```         
+ Response 204 (application/json)
        
+ Response 400 (application/json)
```
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
```    
## (DELETE) 删除主题索引 [/article/topics/index/{id}]

+ Description
    + [MUST] ROLE_ADMIN

+ Parameters
    + id (long) - topic ID


### 删除主题索引 [DELETE]

+ Response 200 (application/json)
```
        {
            "data": {
                "context": {
                    "empty": true
                },
                "headers": [],
                "shardInfo": {
                    "total": 2,
                    "successful": 1,
                    "failures": [],
                    "failed": 0
                },
                "index": "topic-0928",
                "id": "1",
                "type": "post",
                "version": 24,
                "found": true,
                "contextEmpty": true
            }
        }
```