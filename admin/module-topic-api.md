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