# 项目合并

## 2017年3月14日

* 干掉CAS了
* 干掉DUBBO
* 功能性代码可以采用共享库开发模式，业务功能不再采用这种模式。
* 为了方便未来对系统的分解/拆分，每个业务模块有自己独立的依赖管理，并且不依赖于其他模块，业务上的交集不采用共享库（即去掉了DTO传输对象 ）的方式进行集成。所以部分API需要修改；
* 为了保证系统的松耦合与高内聚，不推荐采用共享数据库的方式进行数据库集成；使用数据库集成的方式会使消费方与特定的技术耦合在一起。
    * 爬虫系统分离
    * 加入MQ
* 业务模块的开发使用[语义化的版本管理](http://semver.org/lang/zh-CN/)
    * 客户端在调用各个服务时（业务模块暴露的服务），仅仅通过查看服务的版本号，就知道它是否能够与之进行集成或向前兼容。
* dao与servcie继承等级结构稍微改变，变化如下：
    * 因为引入多数据源配置，源抽象类中的Autoware注解下移，需要手动配置指定的SqlSessionTemplate与事务管理器；
    * Dao与Service的实现类现在继承一个业务领域相关数据集范围的适配器
* MODULE-用户中心(认证/授权/ACL管理等)
    * mifan-user-dao : 数据访问层
    * mifan-user-domain : 领域模型
    * mifan-user-service : 业务层
    * mifan-user-web : WEB层
* MODULE 产品中心 (原b-web-crawler)
    * mifan-sku-dao : 数据访问层
    * mifan-sku-domain : 领域模型
    * mifan-sku-service : 业务层
    * mifan-sku-web : WEB层
* MODULE 爬虫 （暂未创建）
* MODULE 日志 （暂未创建）
* MODULE 抽奖 （暂未创建）
* MODULE analyzer : 分析相关
* 需要夸服务的API写在mifan-api-web模块里
* 不需要快服务的API写在特定的服务中。e.g：用户登录API写在mifan-user-web中
* 类库命名空间变化，由com.budee变更为com.mifan
* API命名空间暂无变化，待未来系统拆分再说
* 所有配置信息在mifan-api-web中集中管理

