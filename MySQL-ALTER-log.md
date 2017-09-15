# 数据库修改记录

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
