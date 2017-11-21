# 数据库修改记录
+ 2017年11月21日 2.1.0 数据库改动
> article.channels，增加频道表创建人和修改人字段
```sql
ALTER TABLE `channels`
ADD COLUMN `creator`  bigint(20) UNSIGNED NOT NULL DEFAULT 0 AFTER `enabled`,
ADD COLUMN `modifier`  bigint(20) UNSIGNED NOT NULL DEFAULT 0 AFTER `creator`;
```

+ 2017年11月02日 2.1.0 数据库改动
> article.translate_task，新增翻译任务表
```sql
CREATE TABLE `translate_task` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `topic_id` bigint(20) unsigned NOT NULL,
  `post_id` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '翻译文档关联id',
  `state` tinyint(3) unsigned NOT NULL COMMENT '状态：1：待领取，2：未提交，3：待审核，4：审核中，5：审核失败，6：审核成功，7：已支付',
  `words_num` int(11) DEFAULT NULL COMMENT '单词数',
  `words_num_cn` int(11) DEFAULT NULL COMMENT '中文字数',
  `bonus` decimal(10,2) DEFAULT NULL COMMENT '支付金额',
  `translator` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '翻译人',
  `auditor` bigint(20) NOT NULL DEFAULT '0',
  `audit_opinion` text CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci COMMENT '审核员反馈',
  `enabled` tinyint(1) NOT NULL DEFAULT '1' COMMENT '启用/禁用',
  `modifier` bigint(20) unsigned NOT NULL COMMENT '修改人',
  `creator` bigint(20) unsigned NOT NULL COMMENT '创建人',
  `modified` datetime NOT NULL COMMENT '修改时间',
  `created` datetime NOT NULL COMMENT '创建人',
  PRIMARY KEY (`id`),
  UNIQUE KEY `topic_id_unique` (`topic_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=34 DEFAULT CHARSET=utf8;
```
+ 2017年11月08日 2.1.0 数据库改动
> article.topics_model新增表, 分类模型增加数据库表, 修改表, 增加了一个字段
```sql
CREATE TABLE `topics_model` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `forum_id` bigint(20) unsigned NOT NULL COMMENT '版块ID',
  `model_status` enum('NEW','RUNNABLE','CLASSIFICATION','WAITING','DONE','DELETE','TERMINATED') NOT NULL DEFAULT 'NEW' COMMENT 'NEW:新创建的模型\nRUNNABLE:正在执行训练的模型\nCLASSIFICATION:正在执行全量分类\nWAITING:等待\nDONE:完成\nDELETE:已经被删除, 无法回滚\nTERMINATED:训练中的模型被终止',
  `model_name` varchar(100) NOT NULL COMMENT '模型名称, e.g : topic-1',
  `priority` tinyint(4) NOT NULL DEFAULT '0' COMMENT '每个forum_id下, 使用一个优先级最高的模型用于分类',
  `conf_random_selection` tinyint(4) NOT NULL DEFAULT '20' COMMENT '(1-100) n%用于测试数据',
  `conf_overwrite` tinyint(1) NOT NULL DEFAULT '1' COMMENT '是否覆盖',
  `result_samples` int(11) NOT NULL DEFAULT '0' COMMENT '训练样本数量',
  `result_text` text COMMENT '模型结果:测试结果或异常结果',
  `enabled` tinyint(1) NOT NULL DEFAULT '1',
  `creator` bigint(20) unsigned NOT NULL DEFAULT '0',
  `modifier` bigint(20) unsigned NOT NULL DEFAULT '0',
  `created` datetime NOT NULL,
  `modified` datetime NOT NULL,
  PRIMARY KEY (`id`),
  KEY `enabled_forum_id_model_status_priority_idx` (`enabled`,`forum_id`,`model_status`,`priority`),
  KEY `forum_id_model_status_priority_idx` (`forum_id`,`model_status`,`priority`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
+ 2017年10月31日 2.1.0 数据库改动
> article.topics表改动 增加两个新字段, 修改索引, SQL初始化训练数据
```sql
ALTER TABLE `mifan_article`.`topics` 
ADD COLUMN `boost` DOUBLE NOT NULL DEFAULT 1.0 AFTER `thumbs_down`,
ADD COLUMN `train_sample` TINYINT(1) NOT NULL DEFAULT 0 AFTER `boost`,
ADD INDEX `forum_id_train_sample_idx` (`forum_id` ASC, `train_sample` ASC);
```
```sql
ALTER TABLE `mifan_article`.`topics` 
DROP INDEX `forum_id_idx` ;
```
```sql
UPDATE topics, topics_fetch SET topics.train_sample = 1 
WHERE topics.id = topics_fetch.topic_id AND topics_fetch.seed_id = 11;
```
> article.topics表改动 增加人工标注的字段 用于与标注的类别进行关联
```sql
ALTER TABLE `mifan_article`.`topics` 
ADD COLUMN `category_id` BIGINT UNSIGNED NOT NULL DEFAULT 0 AFTER `forum_id`,
ADD INDEX `category_id_idx` (`category_id` ASC);
```
> article.forum_categories 新增类别表
```sql
CREATE TABLE IF NOT EXISTS `mifan_article`.`forum_categories` (
  `id` BIGINT UNSIGNED NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `forum_id` BIGINT UNSIGNED NOT NULL COMMENT '版块ID',
  `root_id` BIGINT UNSIGNED NOT NULL DEFAULT 0 COMMENT '根节点ID',
  `parent_id` BIGINT UNSIGNED NOT NULL DEFAULT 0 COMMENT '父节点ID',
  `title` VARCHAR(255) NULL,
  `path` VARCHAR(255) NULL,
  `depth` TINYINT UNSIGNED NOT NULL DEFAULT 1 COMMENT '深度',
  `leaf` TINYINT(1) NOT NULL DEFAULT 1 COMMENT '是否是叶子节点',
  `display_order` INT NOT NULL DEFAULT 0,
  `enabled` TINYINT(1) NOT NULL DEFAULT 1,
  `creator` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `modifier` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `created` DATETIME NOT NULL,
  `modified` DATETIME NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `root_id_enabled_idx` (`root_id` ASC, `enabled` ASC),
  INDEX `parent_id_enabled_idx` (`parent_id` ASC, `enabled` ASC),
  INDEX `enabled_forum_id_root_id_parent_id_idx` (`enabled` ASC, `forum_id` ASC, `root_id` ASC, `parent_id` ASC))
ENGINE = InnoDB
```
> article.word_dictionary 英汉字典表
```sql
CREATE TABLE IF NOT EXISTS `mifan_article`.`word_dictionary` (
  `id` BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  `word_en` VARCHAR(255) NOT NULL,
  `word_en_hash` BIGINT NOT NULL,
  `word_cn` VARCHAR(255) NOT NULL,
  `word_cn_hash` BIGINT NOT NULL,
  `enabled` TINYINT(1) NOT NULL DEFAULT 1,
  `creator` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `modifier` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `created` DATETIME NOT NULL,
  `modified` DATETIME NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `enabled_word_en_hash_idx` (`enabled` ASC, `word_en_hash` ASC),
  INDEX `enabled_word_cn_hash_idx` (`enabled` ASC, `word_cn_hash` ASC))
ENGINE = InnoDB
```
+ 2017年9月27日 2.0.3 数据库改动
> article.navigation 新增导航菜单表
```sql
CREATE TABLE IF NOT EXISTS `mifan_article`.`navigation` (
  `id` BIGINT UNSIGNED NOT NULL AUTO_INCREMENT COMMENT 'ID',
  `parent_id` BIGINT UNSIGNED NOT NULL DEFAULT 0 COMMENT 'PARENT ID',
  `elastic_query_builder_id` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `display_order` INT NOT NULL DEFAULT 0 COMMENT 'display order',
  `title` VARCHAR(100) NULL COMMENT '标题',
  `image` VARCHAR(255) NULL,
  `enabled` TINYINT(1) NOT NULL DEFAULT 1,
  `creator` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `modifier` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `created` DATETIME NOT NULL,
  `modified` DATETIME NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `elastic_query_builder_id_idx` (`elastic_query_builder_id` ASC),
  INDEX `parent_id_idx` (`parent_id` ASC),
  INDEX `enabled_parent_id_display_order_idx` (`enabled` ASC, `parent_id` ASC, `display_order` ASC))
ENGINE = InnoDB;
```
> article.elastic_query_builder 新增elastic查询表
```sql
CREATE TABLE IF NOT EXISTS `mifan_article`.`elastic_query_builder` (
  `id` BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  `title` VARCHAR(100) NOT NULL,
  `query_fields_en` VARCHAR(255) NULL,
  `query_fields_cn` VARCHAR(255) NULL,
  `filter_categories_en` VARCHAR(255) NULL,
  `filter_categories_cn` VARCHAR(255) NULL,
  `positive_query_string_en` VARCHAR(1024) NULL,
  `positive_query_string_cn` VARCHAR(1024) NULL,
  `negative_query_string_en` VARCHAR(1024) NULL,
  `negative_query_string_cn` VARCHAR(1024) NULL,
  `negative_boost` DOUBLE NOT NULL DEFAULT 0.2,
  `function_score_mode` ENUM('FIRST', 'MULTIPLY', 'SUM', 'AVG', 'MAX', 'MIN') NOT NULL DEFAULT 'MULTIPLY',
  `function_boost_mode` ENUM('REPLACE', 'MULTIPLY', 'SUM', 'AVG', 'MAX', 'MIN') NOT NULL DEFAULT 'MULTIPLY',
  `enabled` TINYINT(1) NOT NULL DEFAULT 1,
  `creator` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `modifier` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `created` DATETIME NOT NULL,
  `modified` DATETIME NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `enabled_idx` (`enabled` ASC))
ENGINE = InnoDB;
```
> article.elastic_function_score 新增elastic函数评分表
```sql
CREATE TABLE IF NOT EXISTS `mifan_article`.`elastic_function_score` (
  `id` BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
  `title` VARCHAR(100) NOT NULL,
  `numeric_field` VARCHAR(100) NOT NULL,
  `filters` VARCHAR(255) NULL,
  `weight` DOUBLE NOT NULL DEFAULT 1,
  `function_modifier` ENUM('NONE', 'LOG', 'LOG1P', 'LOG2P', 'LN', 'LN1P', 'LN2P', 'SQUARE', 'SQRT', 'RECIPROCAL') NOT NULL DEFAULT 'NONE',
  `function_factor` DOUBLE NOT NULL DEFAULT 1,
  `function_missing` DOUBLE NOT NULL DEFAULT 1,
  `function_global` TINYINT(1) NOT NULL DEFAULT 1,
  `enabled` TINYINT(1) NOT NULL DEFAULT 1,
  `creator` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `modifier` BIGINT UNSIGNED NOT NULL DEFAULT 0,
  `created` DATETIME NOT NULL,
  `modified` DATETIME NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `enabled_global_idx` (`enabled` ASC, `function_global` ASC))
ENGINE = InnoDB;
```

+ 2017年9月15日 
> ucenter.user_addresses 新增字段
```sql
ALTER TABLE `ucenter`.`user_addresses` 
ADD COLUMN `address_label` VARCHAR(255) NULL COMMENT '地址标签' AFTER `address`,
ADD COLUMN `postal_code` VARCHAR(30) NULL COMMENT '邮政编码' AFTER `address_label`;
```
+ 2017年9月12日 
> mifan_article.moderation 删除原post_id_idx普通索引
```sql
ALTER TABLE `mifan_article`.`moderation` 
DROP INDEX `post_id_idx` ;
```

> mifan_article.moderation 设置其为唯一索引
```sql
ALTER TABLE `mifan_article`.`moderation` 
ADD UNIQUE INDEX `post_id_unique` (`post_id` ASC);
```

> mifan_article.hope_translate 设置联合唯一索引
```sql
ALTER TABLE `mifan_article`.`hope_translate` 
ADD UNIQUE INDEX `user_id_extend_id_unique` (`user_id` ASC, `extend_id` ASC);
```

> mifan_support.praise 设置默认值为0, 不允许为空
```sql
ALTER TABLE `mifan_support`.`praise` 
CHANGE COLUMN `comment_id` `comment_id` BIGINT(20) UNSIGNED NOT NULL DEFAULT '0' COMMENT '评论标识' ;
```

>  mifan_support.praise 删除comment_id外键
```sql
ALTER TABLE `mifan_support`.`praise` 
DROP INDEX `comment_id` ;
```

>  mifan_support.praise 设置联合唯一索引
```sql
ALTER TABLE `mifan_support`.`praise` 
ADD UNIQUE INDEX `conf_id_theme_id_comment_id_creator_unique` (`conf_id` ASC, `theme_id` ASC, `comment_id` ASC, `creator` ASC);
```
+ 2017年10月28日 
> mifan_article.topics_fetch 修改原topic_id_idx普通索引为唯一索引
```sql
ALTER TABLE `mifan_article`.`topics_fetch` 
DROP INDEX `topic_id_idx` ;
ADD UNIQUE INDEX `topic_id_idx` (`topic_id` ASC);
```
