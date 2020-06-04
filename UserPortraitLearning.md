

## ch2 数据指标体系

### 总览
1. 基于用户维度(userid)的用户标签体系
2. 基于用户使用设备维度(cookie_id)的标签体系
> 当用户没有登录账号却访问设备时，可以基于用户在设备上的行为对该设备推送广告、产品和服务。

### 用户属性维度
> 建好用户属性维度标签，再进行打标签。


 ==逻辑应该是这样：先确定标签主题，继而按标签主题展开成多个一级归类，每个一级归类下有多个标签名称，获得标签名称是何种方式即为标签类型==

| 标签名称 | 标签主题 | 一级归类 | 标签类型 |
| :---: | :---: | :---: | :---: |
| 男 | 用户属性 | 自然性别 | 统计 |
| 女 | 用户属性 | 购物性别 | 规则 |

`标签关系，可互斥或不互斥，如标签男标签女互斥`

1. 统计类标签
- 一级归类即为常说的维度分类，与标签名称是一对多的关系，与标签主题多对一的关系

| 标签名称 | 标签主题 | 一级归类 | 标签类型 |
| :---: | :---: | :---: | :---: |
| 钻石会员 | 用户属性 | 会员类型 | 统计 |
| 白金会员 | 用户属性 | 会员类型 | 统计 |
| 黄金会员 | 用户属性 | 会员类型 | 统计 |
| 白银会员 | 用户属性 | 会员类型 | 统计 |
| 黄铜会员 | 用户属性 | 会员类型 | 统计 |


2. 规则类标签
`依靠数据调研结果制定科学的规则`

| 标签名称 | 标签主题 | 一级归类 | 标签类型 |
| :---: | :---: | :---: | :---: |
| 高活跃度 | 用户属性 | 用户活跃度 | 规则 |
| 中活跃度 | 用户属性 | 用户活跃度 | 规则 |
| 低活跃度 | 用户属性 | 用户活跃度 | 规则 |
| 新用户 | 用户属性 | 用户活跃度 | 规则 |
| 流失用户 | 用户属性 | 用户活跃度 | 规则 |

3. 机器学习挖掘类标签

### 用户行为维度


| 标签名称 | 标签主题 | 一级归类 | 标签类型 |
| :---: | :---: | :---: | :---: |
| 近x日访问次数 | 用户行为 | 近x日行为 | 统计 |
| 近x日客单价 | 用户行为 | 近x日行为 | 统计 |
| 近x日活跃天数 | 用户行为 | 近x日行为 | 统计 |
| 近x日访问时长 | 用户行为 | 近x日行为 | 统计 |
| 高频用户 | 用户行为 | 购买频度 | 规则 |
| 土豪用户 | 用户行为 | 消费充值 | 规则 |


### 用户消费维度

### 风险控制维度

### 社交属性维度


## ch3 标签数据存储
> 存储方式的技术选型，取决于应用场景。主要介绍hive、mysql、hbase、elasticsearch的应用场景和解决方案。

### hive存储
- hive数仓
1. hive数仓，用于存储用户标签数据。
- 数仓特点：
  - 面向主题的（如用户主题、订单主题、社群主题等）
  - 集成的（从业务数据库到ods层，经etl清洗）
  - 非易失的（主要指使用规则上，不能修改删除）
  - 随时间变化（数据带有时间属性）
- 数仓开发：
  - 事实表：围绕业务过程设计，
  - 维度表：对事实属性的各个方面描述
  
2. 分区存储
- 分表存储
  - 人口属性表：
  - 行为属性表：
  - 用户消费表：
  - 风险控制表：
  - 社交属性表：
  - 在开发标签宽表时，为提高插入和查询效率，hive中使用分区表。在建表时引入partition，查询时通过hive的分区机制来控制一次遍历的数据量。
  
3. 标签汇聚
- 背景：由于hive分区存储标签，一个用户的标签会插入到不同分区里面。为方便分析和查询，将用户的标签做聚合处理。
- 方式：使用udf函数对存储用户标签的各个表中数据进行聚合，最终用户各个标签汇聚成json字符串格式。
- 目的：便于查询。在用户画像产品中，输入用户id查询，可在前端展示该用户的各个标签，如用户属性、用户行为等。

4. ID-MAP
- 背景：把用户不同来源的身份标识通过数据手段识别为同一主体。
- 场景：用户未登录app状态下，行为数据是设备id（cookieid）记录的；用户登录app后，行为数据是账号id（userid）记录的。虽然是同一个用户，但cookieid和userid数据是未打通的。
- 目的：通过ID-MAP打通cookieid和userid，可以获取用户登录和未登录的数据。
- 我的经历：当前公司的同一业务数据，同一用户在不同平台（神策埋点、mysql）不一样，在不同产品下也可能不同。

- 解决示例：
```
埋点日志表 ods.page_event_log
访问日志表 ods.page_view_log
```

### MySQL存储
1. 元数据管理
hive适用于大数据量的批处理作业，对于量级较小的数据，MySQL有更快的读写速度。因此，MySQL数据库方便标签元数据的编辑、查询、管理。

元数据录入
元数据查询
元数据表结构设计

2. 监控预警数据
MySQL存储每天对etl结果的监控信息。从画像调度流关键节点看：
- 标签计算数据监控
监控每天标签etl的数据量是否出现异常
- 服务层同步数据监控
服务层一般采用HBase、elasticsearch作为数据库存储标签数据供线上调用，将标签相关数据从hive数仓想服务层同步的过程中，需要记录相关数据在hive中的数量及同步到服务层的数量，若数量不一致则触发出错告警。

在对画像的数据监控中，调度流每跑完相应的模块，就将该模块的监控数据插入MySQL，进行校验任务判断。

3. 结果集存储
打通画像数据与线上业务系统时，考虑将存储在hive中的用户标签数据同步到各业务系统中，此时MySQL可用于存储结果集。结果集存储 多维透视分析的标签、圈人服务用的用户标签、当日记录各标签数量，用于校验标签数据是否出现异常 。

Sqoop支持Hadoop和关系型数据库两者数据的相互迁移。

工程例子：将hive中的标签数据迁移到MySQL中
首先，在hive中建立用户身份标签信息表`dw.userprofile_userservice_all`，设置日期分区以满足按日期筛选
然后在MySQL中建立用于接收同步数据的表`userservice_data`
使用Python脚本调用shell命令，完成hive到mysql的同步。
```python
import os
import MySQLdb
import sys
def export_data(hive_tab, data_date):
    sqoop_command = "sqoop export --connect jdbc:mysql://10.xxx.xxx.xxx:3306/mysql_database --username username -
/dw.db/" + hive_tab + "/data_date=" + data_date + " --input-fields-terminated-by '\001'"
    os.system(sqoop_command)
    print(sqoop_command)
if __name__ == '__main__':
    export_data("dw.userprofile_userservice_all", '20181201')
```
其中，sqoop参数如下：
```
sqoop export
--connect 指定JDBC连接字符串,包括IP 端口 数据库名称 \
--username JDBC连接的用户名\
--passowrd JDBC连接的密码\
--table 表名\
--export-dir 导出的Hive表, 对应的是HDFS地址 \
--input fileds-terminated-by ‘,’ 分隔符号
```

### HBase存储
1. HBase介绍
HBase是实时读写、高性能、列存储、可伸缩的分布式存储系统，同hive一样运行在HDFS之上。

与Hive不同的是，HBase能在数据库上实时运行，不是离线跑MapReduce任务，适合进行大数据实时查询。

画像系统每天在Hive里跑出的结果集，可同步到HBase数据库，用于`线上实时应用场景`。

- 几个基本概念：
  - row key： 表示唯一行记录的主键，HBase数据按row key的字典顺序进行全局排列。
  - 访问HBase行只有3种方式：访问单个row key；row key的正则访问；全表扫描
  - row key三个原则：唯一性；长度原则；散列原则
  - 由于row key的长度限制，因此不能将很多查询条件拼接在row key中。一般，通过二级索引来满足复杂条件查询。
  - columns family：指列簇，HBase中每个列都归属于某个列簇。列簇是表schema的一部分，必须在使用表前定义。
  - 划分columns family的原则：是否具有相似的数据格式；是否具有相似的访问类型。
  
2. HBase在画像系统的应用场景及工程化实现
- 场景描述:
某渠道运营人员为促进未注册的新安装用户注册、下单，计划通过App首页弹窗发放红包或优惠券的方式进行引导。

如何找出这样的未注册的新安装用户呢？
```
业务逻辑
1.运营人员通过组合用户标签（如“未注册用户”和“安装距今天数”小于××天）筛选出目标用户群体
2.将目标人群推送到“广告系统”（只有目标人群来访APP时才推送弹窗）
```

- 工程实现：
  - 用户标签数据经ETL，将每个用户的标签聚合后插入到hive的目标表中。
  - 将hive数据导入到HBase，便于线上接口实时调用HBase库的数据
    - 判断HBase当前活跃节点是哪台机器。因为HBase的服务器体系结构遵循主从服务器架构
    - 在活跃节点机器HMaster上创建预分区表，避免数据都写入一个region
    - 使用scala脚本将数据写入HBase集群中
  - 在线接口实时查询来访APP用户在HBase库的数据，若用户标签满足条件，则推送弹窗。
    - HBase需建立二级索引来查询复杂组合标签数据，选用elasticsearch存储HBase索引数据
    - 在elasticsearch中查询组合标签条件对应的索引数据，由索引数据去HBase中获取row key对应的数据，即通过二级标签完成组合标签的查询
  - 校验HBase和Hive的数据量是否一致：从hive到HBase灌入数据可能会缺失，因此在向HBase同步数据完成后没要校验数据量是否一致。
  ```python
  # 查询Hive中数据
  def check_Hive_data(data_date):
    r = os.popen("Hive -S -e\"select count(1) from dw.userprofile_usergroup_labels_all where data_date='"+data_
    Hive_userid_count = r.read()
    r.close()
    Hive_count = str(int(Hive_userid_count)
    print "Hive_result: " + str(Hive_count)
    print "Hive select finished!"
  # 查询HBase中数据
  def check_HBase_data(data_date):
    r = os.popen("HBase org.apache.hadoop.HBase.mapreduce.RowCounter 'userprofile_labels'\" 2>&1 |grep ROWS")
    HBase_count = r.read().strip()[5:]
    r.close()
    print "HBase result: " + str(HBase_count)
    print "HBase select finished!"
  # 连接 DB,将查询结果插入表
  db = MySQLdb.connect(host="xx.xx.xx.xx",port=3306,user="username", passwd="password", db="xxx", charset="utf8")
  cursor = db.cursor()
  cursor.execute("INSERT INTO service_monitor(date, service_type, Hive_count, HBase_count) VALUES('"+datestr_+"', '
  db.commit()
  ```

### Elasticsearch存储
1. elasticsearch简介
elasticsearch：实时存储数据、实时检索数据、处理PB级数据。

适用于对响应时间要求较高的场景，如：用户标签查询，用户人群计算，用户群多维透视分析

elasticsearch存储结构：（类似于MongoDB，json文档为行）

区分多个数据库的索引，索引包括多个类型（表），类型包括很多文档（行），然后每个文档包含很多字段（列）。定位方式：索引、类型、文档、字段。与关系型数据库差不多，from db.table where 满足条件的行 select 列。

2. 应用场景
HBase使用row key作为一级索引，不支持多条件查询。为支持高效查询，同时实现复杂查询，elasticsearch存储HBase的row key，实现步骤为：
- 在elasticsearch中存放用于检索条件的数据，row key也存进去（检索条件的数据，目前难以理解，这一步体现了elasticsearch的复杂查询优势，否则不需要通过elasticsearch这一步）
- 在elasticsearch中查找复杂标签对应的row key
- 使用上一步的row key在hbase数据库中查询结果
