# 数据库修改记录

+ 2017年9月15日 
> ucenter.user_addresses 新增字段
```sql
ALTER TABLE `ucenter`.`user_addresses` 
ADD COLUMN `address_label` VARCHAR(255) NULL COMMENT '地址标签' AFTER `address`,
ADD COLUMN `postal_code` VARCHAR(30) NULL COMMENT '邮政编码' AFTER `address_label`;
```
+ 2017年9月12日 
> article.moderation 删除原post_id_idx普通索引, 并设置其为唯一索引
```sql
ALTER TABLE `mifan_article`.`moderation` 
DROP INDEX `post_id_idx` ;

ALTER TABLE `mifan_article`.`moderation` 
ADD UNIQUE INDEX `post_id_unique` (`post_id` ASC);
```

+ hope_translate
        + user_id、extend_id设置联合唯一索引
+ praise
        + comment_id删除外键，设置默认值为0，不允许为空，conf_id、theme_id、comment_id、creator设置为联合唯一索引
