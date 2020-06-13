# 工作日常

## 2020-06-13
1. 完成业务的用户课程行为画像部分，当前程序完成标签计算，标签写入数据库暂未考虑。
2. 学习了决策树的ID3算法原理。

## 2020-06-12
1. 调整宽表代码
- 宽表设计到部分内容标签，但此前的宽表逻辑限制了期数（某时间点之后的数据）会导致缺少用户数据，因此需要取消期数限制以完整化用户内容标签。
- 宽表数据源涉及到mysql和MongoDB库，使用airflow调度宽表脚本。
2. 业务逻辑问题排查

	IDP切期聊天记录消失原因：用户切期前跟助教a聊天，用户付费转化后，该用户被切换期数，该用户的助教变成了助教b（用户与助教a的关系找不到了），而用户并没有跟切换后的助教有好友关系（只与助教a有好友关系，与逻辑切换的助教b没有好友关系），因此看板找不到助教显示不了聊天记录。这是个业务逻辑问题。

3. 给业务增长提供数据支持，包括投放、内容、运营等环节。虽然数据基础设施不够支持，但还是要去推进落实。

## 2020-06-11
1. presto sql与hive sql在json字符串函数上有区别，[参考资料](https://www.cnblogs.com/drjava/p/10536922.html)
- hive sql: `get_json_object(string json_string, string path)` ，其中 string path用法为`$.key`，若有数组，用[0]、[1]...
- presto sql: 没有该函数，取而代之的函数是`json_extract()`，数组用` json_array_get()`函数。
- 我们使用的hive是presto库连接的hive服务器，因此不支持hive sql 的语法。
2. presto的函数返回类型并不如眼见的那样,[presto函数参考](https://prestodb.io/docs/current/functions/json.html)
```
json_extract(json, json_path) → json
Evaluates the JSONPath-like expression json_path on json (a string containing JSON) and returns the result as a JSON string

json_extract_scalar(json, json_path) → varchar
Like json_extract(), but returns the result value as a string (as opposed to being encoded as JSON). The value referenced by json_path must be a scalar (boolean, number or string)
```
- 因此，若要对json字符串的结果进行匹配，需要类型一致。presto有cast函数
- cast(value AS type)  显式转换一个值的类型。如`SELECT CAST(JSON 'null' AS VARCHAR); -- NULL`
3. 数据处理逻辑
- 方式一，presto sql取完数据（未深度处理），由pd.read_sql(sql,connectable)读取为dataframe，再进行深度处理。
- 方式二，presto sql利用自身函数进行深度处理，再交由python处理presto sql不能处理的部分。
- 对于200w数据，方式一会查询200W，耗用200w数据查询的时间，占用200w数据的内存，df函数处理也耗内存
- 对于200w数据，由prestosql 处理成100w，只需要查询100w数据，df再处理时也会比方式一占内存少。
- 因此，能sql解决的简单处理，由sql完成。

## 2020-06-10
1. 总结连接不同的数据库
- 场景一：在客户端（如Navicat，datagrip，dbeaver，nosqlbooster）连接数据库。
	 - 需要在客户端下载对应的数据库服务器驱动，如dbeaver客户端下载jdbc驱动连接presto库的hive服务器。
- 场景二：在python中连接各数据库。如连接presto数据库的hive
    - 方式一，`pyhive.presto`,注意presto.connect()方法格式,其中没有db参数
    ```
    conn = presto.connect(
    host='your-presto-host.net',
    port=8080,
    protocol='https',
    catalog='the-catalog',
    schema='the-schema',
    username='the-user',
    requests_kwargs=req_kw,
    )
    ```
    参考资料：https://gist.github.com/tommarute/9e6c18350964d99988667707b66af259 , 
    https://pydoc.net/PyHive/0.3.0/pyhive.presto/
    - 方式二，`sqlalchemy.engine`,create_engine('presto...')	    
	
2. 规划行为标签代码框架

3. 总结python中连接MySQL数据库的方案
方案一：使用pymysql的想关方法（即DBAPI方式），
```
cur = pymysql.connect(user=user,password=password,host=host,port=port).cursor() 
# cursor方法参数cursor(cursor=pymysql.cursors.DictCursor)，查询结果为字典key-value
cur.execute(sql)
results = cur.fetchall()
print(results) # 返回多行记录元组的元组
```
或
```
con = pymysql.connect(user=user,password=password,host=host,port=port)
result = pd.read_sql(sql,con)
print(results) # 返回dataframe，这种方式可直接使用dataframe操作，推荐
```

方案二：使用sqlalchemy（对这个不熟练）
- 以下这种方式，使用pymysql驱动的sqlalchemy的[链接格式](https://docs.sqlalchemy.org/en/13/dialects/mysql.html#module-sqlalchemy.dialects.mysql.pymysql),不需要导入pymysql库
```
import pandas as pd
sql ='SELECT * from table'
con = 'mysql+pymysql://{username}:{password}@{host}:3306/{db}'.format(username=user,password=password,host=host,db=db)
results = pd.read_sql(sql,con)
print(results)
```

- 以下方式，使用engine
```
from sqlalchemy.engine import create_engine
import pandas as pd
sql = 'SELECT * from table'
engine = create_engine('mysql+pymysql://{username}:{password}@{host}:3306/{db}'.format(username=user,password=password,host=host,db=db)')
results = pd.read_sql(sql,engine)
print(results)
```

- 以下方式，使用engine.connect()
```
from sqlalchemy.engine import create_engine
import pandas as pd
sql = 'SELECT * from table'
engine = create_engine('mysql+pymysql://{username}:{password}@{host}:3306/{db}'.format(username=user,password=password,host=host,db=db)')
con = engine.connect() # con.cursor()会报错，AttributeError: 'Connection' object has no attribute 'cursor'
results = pd.read_sql(sql,con)
print(results)
```

- pd.read_sql(sql,con)的参数官方文档说明 `con:SQLAlchemy connectable (engine/connection) or database str URI`

	


## 2020-06-09
1. 使用HQL完成hive取数,主要阅读《hive编程指南》的HQL部分
```sql
select
	/*+ STREAMTABLE(evcrr) */
	evcrr.*,
	cp.title,
	cp.resource_message ,
	cp.resource_option
from
	hive.forchange_prod.external_vm_classroom_resource_records evcrr
left outer join hive.forchange_prod.cleword_packages cp on
	evcrr.app_id = cp.app_id
	and evcrr.package_id = cp.package_id
	and evcrr.resource_id = cp.resource_id
where
	evcrr.app_id = '9' 
```
2. 在python使用sqlalchemy读取presto的hive时，报错：`sqlalchemy.exc.NoSuchModuleError: Can't load plugin: sqlalchemy.dialects:presto`
- 方案一：
使用Teradata连接presto，文章来源 https://stackoverflow.com/questions/53367842/python-compiled-script-giving-error-of-cant-load-plugin-sqlalchemy-dialectsp

关于teradata数据库及查询语句，[官方文档](https://www.docs.teradata.com/reader/uR6Sq~i19bIG3mKP7cb6lA/4gXDlAZwdey2LOD3mob1MA)这样描述：
```
Teradata® QueryGrid™ 2.0x 是一种数据分析结构，它可以跨一个或多个数据源提供无缝、高性能的数据访问、处理和移动。数据源可以是相同类型的，例如 Teradata Database 和 Teradata Database，也可以是不同类型的，例如 Teradata Database 和 Presto。

Teradata QueryGrid 连接器用于联接网络结构中的数据源。每个连接器都能启动 SQL 查询，并且可以是该网络结构中的 SQL 查询目标。链接指定哪些连接器可以相互通信。

Teradata QueryGrid 支持以下连接器：
Teradata Database
Presto
hive
Spark SQL
Oracle（仅作为目标连接器）
```

上述只是简单了解teradata数据库的基础概念，而我要解决的是在python中怎么使用teradata包连接presto库的hive。

查阅python teradata资料，https://downloads.teradata.com/tools/reference/teradata-python-module#Installing ，报错信息为`teradata.api.InterfaceError: ('DRIVER_NOT_FOUND', "No driver found with name 'Teradata'.  Available drivers: SQL Server,PostgreSQL ANSI(x64),PostgreSQL Unicode(x64),Amazon Redshift (x64),MySQL ODBC 8.0 ANSI Driver,MySQL ODBC 8.0 Unicode Driver,ODBC Driver 17 for SQL Server")
`,这表明需要装Teradata数据库驱动。

即使耗费时间装好teradata驱动后，但teradata的函数不是主流，开始选择方案二。

- 方案二：安装好pyhive模块。出现的问题为pyhive.exc.DatabaseError。（与 https://quabr.com/56219581/how-to-fix-pyhive-exc-databaseerror-message-line-1130-function-date-sub类似）

应该是hive的url问题，环境变量不要''。
`hive_engine = create_engine('presto://{0}@{1}:{2}/hive/{3}'.format(hive_user, hive_host, hive_port,hive_db))`

pyhive参考资料：
https://github.com/dropbox/PyHive




## 2020-06-08
- 使用DBeaver连接PrestoDB,使用PrestoDB连接host（含有名为hive的数据库）。
   - 直接使用DBeaver连接hive会报错，'Could not open client transport with JDBC Uri: jdbc:hive2://host:port: Invalid status 72',这个报错信息表明无法用jdbc uri打开客户端传输。若host:port未出错，那就是hive客户端的问题。而连接远程host不需要安装本地hive，经过分析才明白原因在哪。
   - 启示：要去仔细分析报错信息，不要忽视报错信息。
   - PrestoDB：是一种用于大数据的高性能分布式SQL查询引擎。其架构允许用户查询各种数据源，如Hadoop、AWS S3、Alluxio、MySQL、Cassandra、Kafka和MongoDB。甚至可以在单个查询中查询来自多个数据源的数据。Presto是Apache许可证下发布的社区驱动的开源软件。
   
- 使用DBeaver连接远程mysql服务器ADB，不报错，完美查数据；但连接本地mysql库，报错，报错信息如下：
   - 'Communications link failure
The last packet sent successfully to the server was 0 milliseconds ago. The driver has not received any packets from the server.
  Connection refused: connect
  Connection refused: connect'
  - 该信息表明，通信连接失败，具体的，驱动不能收到服务器的包。
  - 第一番操作，驱动版本原因，MySQL本地是8.0以后版本，dbeaver默认驱动版本是5.0后。但更改8.0后驱动下载失败，mysql驱动直接挂掉了。
  - 第二番操作，查看windows本地mysql服务，竟然是“已停止”。启动MySQL服务后，报错“Unable to load authentication plugin 'caching_sha2_password'.”。
  - 查阅资料后，这是由于MySQL server8.0以后，默认的认证插件从mysql_native_password 变为caching_sha2_password，解决方案：https://blog.csdn.net/m0_37619183/article/details/81873465
  - 打开'mysql 8.0 command line client'，输入密码闪退。切换到MySQL server bin目录，密码出问题，转换成MySQL本地安装的问题了。

   
- 周会总结：
1. 中节点自媒宝投放：平台文章>平台二维码>用户阅读>用户扫码进入报名页>用户报名成功转化。涉及指标有：阅读数，扫码报名数，转化数，还有投放金额，投放成本等，都是用比率对过程环节的衡量。
   - 现在遇到问题：数据看板突然不能显示某平台的投放数据。
   - 分析：从路径上，且每个路径上都要有数据去验证是否是该路径出问题。一是平台二维码链接，二是扫码后能否跳转，三是跳转的目的链接是否正确，四是埋点数据能否被程序获取正常入库。
   - 定位完问题，手动更新历史链接，调整程序脚本，能正常解析链接。
2. hive
  




## 2020-06-07
- 整理工作内容，修改
- 回顾自我学习的效率问题，照书抄一遍虽然能加深记忆，但缺乏自我思考知识内化的过程，后续会`记录成自己思考的样子，而不是书本中那种完美的样子，代码多结合实例去操作，即使是hive`
- 确定后续的学习重点`hive、埋点，abtest`

## 2020-06-06  
- 休息，外加处理杂事。

## 2020-06-05
1. 阅读数据仓库开发规范
- 涉及到阿里系列产品：ADB, EMR。
- 涉及到环境：生产环境、POC（proof of concept测试环境）
- 涉及到数仓层：prod(数据原始层)、ods（）、dwd（DW明细层，经ETL生成的事实表及宽表数据）、dws（轻度汇总数据层，经dwd生成的聚合数据）、dm（数据集市层，事实表与维度表聚合后或宽表加工后的数据层）、dim（维度数据层，保存维度表信息，如日期维度、用户维度、产品维度）

2. idp业务周会
- 业务六月份目标确定。
- 未来涉及到新版本课程内容的数据需求。
- wehub被封后的聊天数据，如何恢复聊天数据的解决方案。

3. 开始使用dbeaver连接hive。

## 2020-06-04
1. 完成业务看板：出勤时段统计，及出勤时段的完课率。
- 样式上，使用了树状图和水平条，工具提示插入表。
- 内容上，找出有意义的潜在指标。
2. 阅读用户画像，学习数据挖掘。


## 2020-06-03
1. idp业务：整理存储学习行为的MongoDB表，确定所需要同步到MySQL库的目标表结构。涉及到MongoDB文档的嵌套字段。
2. 确定idp用户学习习惯看板要完成的功能点：在期数、关卡维度下，出勤时段（只取hour）次数（而非人数）统计，在出勤时段的完课率。目前，纠结于看板样式的呈现方式。
3. 继续阅读用户画像，并整理有用信息到github。

## 2020-06-02
1. 社群的用户表，因wehub被限制，如今开发使用opendns作为解决方案，登录微信全量更新好友关系。
2. tableau看板：行功能区有聚合维度，对看板呈现的影响。[维度聚合资料](https://help.tableau.com/current/pro/desktop/zh-cn/calculations_aggregation.htm#tableau-%E4%B8%AD%E9%A2%84%E5%AE%9A%E4%B9%89%E8%81%9A%E5%90%88%E7%9A%84%E5%88%97%E8%A1%A8)
```
对维度聚合，返回度量或维度中的所有唯一值。
因此，min(date([填写时间]))字段在行功能区，会呈现度量区的唯一值，度量区会按列功能区展开。
若不添加列功能区，呈现度量区唯一值，若唯一值对应的min(date([填写时间]))只有一个时间，则显示一行（该最小时间对应的度量区唯一值）
tableau呈现不存在管道关系，不会先计算维度再呈现度量区。
```

3. 周会收获
- 神策登录id(业务系统的openid)与业务数据库user_id的ID-map抽样验证。
   - 获取神策id、设备id、登录id等，读取文件为datafram
   - 利用获取的神策登录id作为业务数据库的openid查询，结果保存为dataframe
   - 两dataframe左联结（使用pd.merge），完美匹配上。逻辑是这样的：1000个登录id能查到多少个业务数据库的user_id，这个需要验证，这里使用datafram联结。
- tableau看板，若不止能提供数据可视化呈现，还能再呈现的基础上给出分析，那才是完美。
- 时间序列模型预测。思路很好，需要学习。
- 埋点要完成两件事：一是区别不同事件的埋点，二是埋点事件的返回值。
- 后裔采集器：前谷歌技术团队倾力打造，基于人工智能技术，只需输入网址就能自动识别采集内容 (http://www.houyicaiji.com/)

## 2020-06-01
- 六一儿童节，有怀旧零食，更让人喜欢的是刻着名字的印章，盖了整整一页纸，很好玩。
- 完成满意度问卷看板迭代，看板兼容两个版本问卷：
   - SQL兼容两个版本问卷，可以使用in 或者union  all，结合具体业务来实现
   - 问卷选项题干兼容，使用case表达式，使用SQL或者tableau函数都可以
   - 问卷选项内容兼容，使用case表达式。
   - 问卷推送规则、满意度指标、填写率等，需要与业务商量一起。
- 讨论流失用户问题以及wehub工具对wechat社群运营的影响
   - 问题：由于wehub工具被限制，添加用户这个行动发生了，但基于wehub的wechat_user_friends的数据表是否会更新数据呢？（原数据肯定有，wehub限制了，用户关系是否会更新呢？）
   - 猜想：聊天记录只有wehub被使用才会采集到wechat_chat_records表，用户关系是不是也如此呢？
   - 实际原因：明日问开发
   - 与开发沟通后消息同步：开发临时使用opendns完成用户好友关系维护，登录微信全量更新用户最新关系，解决了因wehub受限造成的社群用户表不更新的问题。

## 2020-05-31
- 写个周记（本子上），这周坚持的事下周还得坚持。
- 开个《数据挖掘：理论与算法》的视频学习沉淀文档


## 2020-05-30
- 学习《数据挖掘：理论与算法》，在宏观层面有所学习，如幸存者偏差、时间维度对数据分析的影响
- 找到《浪潮之巅》，去阅读。


## 2020-05-29
- 参加IDP各线负责人周会
   1. 管理者融入各业务线同学工作中，有利于了解底下业务情况，关心业务同学状态，业务人员往往报以高效工作
   2. 计划A和计划B，两计划会占用资源但每个计划占用资源都不是全部的。设计一个计划C，拉上全部资源，做一个最小化完整产品。
   3. 数据自己完成的看板，站在业务视角，会不会实用呢？因此，记录需求的issue还是很有必要的。
   4. 数据能支持决策，支持迭代，如何科学地预设获取数据，如何在预设时提供科学的支持呢?
- python连接数据库，学习使用连接类来完成代码，而不是散装函数。
- 学习[数据挖掘：理论与算法](https://www.bilibili.com/video/BV1gE411p73X?p=7)
   



## 2020-05-28
1. 整理用户画像的指标体系，熟悉确定标签名称的思路： `先确定标签主题，多个一级归类，多个标签名称`。

   指标  ——> 根据业务线梳理用户画像涉及的主题 ——> 标签
2. 确定IDP业务的课程行为标签实验



## 2020-05-27
- Tableau文本格式说明：
  - 正文：宋体，9号。
  - 行列字段引用：宋体，9号，加粗，并以[字段]表示。
  - 文本标题：宋体，12号，加粗。
  - 字母：Times New Roman
  - 字段值：普通正文，加<value>表示。
- Tableau阴影色彩说明：
  - [ ] 一直使用不好，对调色方案还需要学习
  - 阴影格式：工作表颜色，区颜色，标题颜色
  - 仪表板阴影：颜色
  - 仪表板对象：布局 > 背景颜色
- Tableau字段处理经验：
  - 创建分层，一是减小看板的显示内容，二是提供交互体验给予使用者，需要细粒度就下钻。一般是相关的维度创建分层，不相关的分层意义不大。
  - 统计次数方式
    - `SUM(IF [消息发送者]!=[nickname] THEN 1 
    ELSE 0 END)`,满足条件的行赋值为1（新字段，没显示，sum后显示），累加后即为新创建字段。
    - 满足条件的行赋值为某字段的值（新字段，没显示，countd后显示），计数后即为新创建的字段。
    ```
      // 学员发送消息，的活跃天数
    COUNTD(if not ISNULL([消息类型]) and [nickname]!=[消息发送者]
    then [入门课Day] end)
    ```
    - 两种方式都可以来完成计数。
    - 要使新创建的字段表达出数据分析师想呈现的数据效果，注意在行列功能区拖动维度指标，依据维度展开数据计算。
    - 上述计算方式依据视图而变化，等同于聚合表达式include。`{include :COUNTD(if not ISNULL([消息类型]) and [nickname]!=[消息发送者]
    then [入门课Day] end)}`
  - 在计算比例时，如[总人数]由fixed聚合而成，分子必须也得是聚合表达式。
  ```
    {include [remark]:sum(if CONTAINS([聊天文本],'我要参加') 
  then 1
  ELSE null 
  END)}/[总人数]
  ```
  -  `在写tableau计算公式时，要有数据源的行列印象`。
- Tableau字段排序
  - 按照另一行列的结果排序，间接完成目标行列的排序，即依据`字段`排序。
- Tableau工具提示
  - 参与视图的字段，才可以被用做`标记区`的`工具提示`插入，即使该字段只是以颜色显示也可以。
  - 想显示在`工具提示`里，但不想显示在视图行列里，可以不勾选`显示标题`。
  
  





## 2020-05-26
- [ ] IDP私聊记录
- 指标确定
  - 消息数应答积极度=学员发送消息数/助教发送消息数
  - 天数应答活跃度=学员活跃天数/助教活跃天数
  - 助教日均消息数=助教发送消息数/助教活跃天数
  - 学员日均消息数=学员发送消息数/学员活跃天数
  - 学员发送消息活跃时间段，在每个学员维度上展开，按两种方式度量：
    - 一是按时段，以0到23小时进行统计，
    - 二是按星期，以1到7进行统计，
- 实现方式
  - tableau计算
    - 时间函数：
      - DATEPART(date_part, date, [start_of_week]) ：根据date_part返回date相应的整数数值
      - DATETRUNC(date_part, date, [start_of_week])：根据date_part返回date相应的截断的制定日期，如以月份级别截断处于月份中间的日期时，此函数返回当月的第一天。
    - 活跃时间段使用DATEPART函数即可，注意date_part取值：
    - https://help.tableau.com/current/pro/desktop/zh-cn/functions_functions_date.htm#datepart-%E5%80%BC
    - https://help.tableau.com/current/pro/desktop/zh-cn/functions_operators.htm#%E8%AE%A1%E7%AE%97%E7%9A%84%E7%BB%84%E4%BB%B6
    - 
    - 参数的灵活使用，给予看板交互式体验
  

