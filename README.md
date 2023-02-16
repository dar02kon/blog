# 数据库课程设计报告

## 基于Web的博客网站

### 需求分析

#### 设计意义

写博客可以记录自己的成长史，我们可以随时清晰的看到自己的努力，是很有意义的一件事情。写博客也可以分享自己的知识，利用博客的论坛交流，你可以和博客上的人互相交流经验，获取一些目前的行业知识，拓展自己的知识面 。

虽然有很多关于博客平台，但我更希望拥有一个属于自己的博客网站来记录与分享自己的学习历程。

#### 功能实现 

**整体功能**

![img](https://dar-1305869431.cos.ap-shanghai.myqcloud.com/java_note/wps27.jpg) 

**用户管理**

![img](https://dar-1305869431.cos.ap-shanghai.myqcloud.com/java_note/wps28.jpg) 

**分类管理**

![img](https://dar-1305869431.cos.ap-shanghai.myqcloud.com/java_note/wps29.jpg) 

**电子书管理**

![img](https://dar-1305869431.cos.ap-shanghai.myqcloud.com/java_note/wps30.jpg) 

### 概念结构设计

**ER图**

![img](https://dar-1305869431.cos.ap-shanghai.myqcloud.com/java_note/wps31.jpg)  

### 逻辑结构设计

user (用户表)

| **属性名**  | **含义**     | **类型** | **说明**                       |
| ----------- | ------------ | -------- | ------------------------------ |
| id          | 用户表主键ID | bigint   | 由雪花算法生成，唯一，不能为空 |
| login_name  | 用户登录名   | varchar  | 不能为空                       |
| name        | 用户昵称     | varchar  |                                |
| password    | 用户密码     | char     | 不能为空，MD5加密存储          |
| create_time | 创建时间     | datetime | 用户创建时填入                 |
| update_time | 更新时间     | datetime | 用户创建和信息更新时填入       |



ebook (电子书表)

| **属性名**   | **含义**        | **类型** | **说明**                       |
| ------------ | --------------- | -------- | ------------------------------ |
| id           | 电子书表主键ID  | bigint   | 由雪花算法生成，唯一，不能为空 |
| name         | 电子书名称      | varchar  |                                |
| category1_id | 分类1（父分类） | bigint   |                                |
| category2_id | 分类2（子分类） | bigint   |                                |
| description  | 电子书描述      | varchar  |                                |
| cover        | 电子书封面      | varchar  | 图片URL                        |
| doc_count    | 文档数          | int      | 该电子书对应下的文档总数       |
| view_count   | 阅读数          | int      |                                |
| vote_count   | 点赞数          | int      |                                |
| create_time  | 创建时间        | datetime | 创建时填入                     |
| update_time  | 更新时间        | datetime | 创建和信息更新时填入           |

 

doc (文档表)

| **属性名**  | **含义**     | **类型** | **说明**                                       |
| ----------- | ------------ | -------- | ---------------------------------------------- |
| id          | 文档表主键ID | bigint   | 由雪花算法生成，唯一，不能为空                 |
| ebook_id    | 电子书ID     | bigint   | 外键，引用ebook电子书表ID                      |
| parent      | 父ID         | bigint   | 父文档ID，引用doc文档表的id，不能为空，默认为0 |
| name        | 文档名称     | varchar  |                                                |
| sort        | 顺序         | int      | 值大的会优先展示                               |
| view_count  | 阅读数       | int      |                                                |
| vote_count  | 点赞数       | int      |                                                |
| create_time | 创建时间     | datetime | 创建时填入                                     |
| update_time | 更新时间     | datetime | 创建和信息更新时填入                           |



content (文档内容表)

| **属性名** | **含义**     | **类型**   | **说明**                  |
| ---------- | ------------ | ---------- | ------------------------- |
| id         | 内容表主键ID | bigint     | 外键，引用doc文档表主键ID |
| content    | 文档内容     | mediumtext | 存储HTML代码              |

 

category(分类表)

| **属性名\** | **含义**     | **类型** | **说明**                                        |
| ----------- | ------------ | -------- | ----------------------------------------------- |
| id          | 文档表主键ID | bigint   | 由雪花算法生成，唯一，不能为空                  |
| parent      | 父ID         | bigint   | 父ID，引用category文档表的id，不能为空，默认为0 |
| name        | 分类名称     | varchar  |                                                 |
| sort        | 顺序         | int      | 值大的会优先展示                                |

 

ebook_snapshot(电子书快照表)

| **属性名**    | **含义**     | **类型** | **说明**                       |
| ------------- | ------------ | -------- | ------------------------------ |
| id            | 文档表主键ID | bigint   | 由雪花算法生成，唯一，不能为空 |
| ebook_id      | 电子书ID     | bigint   | 外键，引用ebook电子书表ID      |
| date          | 快照日期     | date     |                                |
| view_count    | 阅读数       | int      |                                |
| vote_count    | 点赞数       | int      |                                |
| view_increase | 阅读增长     | int      |                                |
| vote_increase | 点赞增长     | int      |                                |

 

### 开发工具

开发环境：Windows10，JDK1.8，MySql8.0.23，Redis6.0

开发语言：Java，Html，JavaScript，SQL

### 总结

1. 在保存信息的service层方法上需要添加注解@Transactional，加上这个注解，如果发生unchecked exception，就会发生rollback，它会在事务开始时，通过`AOP`机制，生成一个代理connection对象，并将其放入 `DataSource` 实例的某个与 `DataSourceTransactionManager` 相关的某处容器中。在接下来的整个事务中，客户代码都应该使用该 connection 连接数据库执行所有数据库命令。事务结束时，回滚在第1步骤中得到的代理 connection 对象上执行的数据库命令，然后关闭该代理 connection 对象。

2. 生成今日电子书快照在数据库层有两种方案：

   > 方案一（ID不连续）：
   >
   >  删除今天的数据
   >
   >  为所有的电子书生成一条今天的记录
   >
   >  更新总阅读数、总点赞数
   >
   >  更新今日阅读数、今日点赞数

   > 方案二（ID连续）：
   >
   >  为所有的电子书生成一条今天的记录（如果还没有）
   >
   >  更新总阅读数、总点赞数
   >
   >  更新今日阅读数、今日点赞数

3. 在查询当天阅读数据与近30天阅读数据都需要用到函数`date_sub(date,interval expr type)`，从日期减去指定的时间间隔。

   `t1.`date` between date_sub(curdate(), interval 30 day) and date_sub(curdate(), interval 1 day)`就表示近30天。

4.  使用`WebSocket`进行消息推送，如点赞消息,一个用户对一篇文章点赞后24小时内不能再对该文章点赞(没有做点赞状态持久化)。因为使用的用单点token来验证用户身份,token在创建时需要设置过期,过期后用户就需要重新登录,没有做到自动刷新登录状态。
