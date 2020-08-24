# 工作日常

## 2020-08-24 :panda_face:
1. 完成最后一项投资方数据的python代码，基于此，快速学习了python其他知识点
- yield与return
- 截断列表为多个子列表代码逻辑
- Series的accessors，包括dt,str常用的属性和方法
- python的 for...else...语句，很不常见
2. 周会
3. 排查特定用户聊天记录丢失的原因，导出数据

## 2020-08-22 & 2020-08-23
1. 自主加班一天，取数还差一个指标，python代码遇到困惑，尚未解决。
2. 阅读完《增长黑客》。对美丽说创业者徐易容的一段话印象深刻：
	```
	创业要有梦想，而不是理想。
	梦想由你的心决定，它不一定成真，但你会喜欢，而理想由大脑决定，容易给自己太大的压力。
	所谓欲速则不达，人被吹得太高就不淡定了。现在做美丽说我希望能让自己慢下来，别老跟人家比来比去，先把用户体验修炼到极致再说，其他都是浮云。如果有梦想，那就坚持，
	顺势而为……以前我做的东西是我喜欢的。现在我觉得我做的东西应该是市场需要的，大家喜欢的。
	```
3. 衣物、床套都清洗晾晒铺好，并吃了水果，保持锻炼。
4. 既然选择了，就尽管去做吧，不荒废时光就好。

## 2020-08-21 
1. 继续昨天的业务数据提取
- SQL完成中位数：
  - 使用row_number() over (partition by col order by col2)
  - 组内数量的统计，用于计算组内中位数。ceil(count(*)/2),floor(count(*)/2)+1

## 2020-08-20 :feet:
1. 构建业务的用户分层
- 基于用户画像的相关知识
- 对业务的全路径梳理，整理出相关指标。后续还要继续
2. 临时紧急取数需求：投资方要查看公司业务相关数据，明天还得接着取。

## 2020-08-19 :pouting_cat:
1. 文本的有监督训练结果
- 标签文本是句子。处理方式有二：
  - 一是把句子算作最后的类别，但这样的类数量太少，文本分类就几十种，不全面。
  - 二是吧句子分词，做关键词提取，这样的类别很多。由于标签文本数量少，测试集的结果是准确率低、召回率很低。
- 有限的标签文本不做训练集测试集划分。直接训练，将得到的模型在未知数据集上测试，人工观测结果。**由于标签太少，观测的结果很类似，区分度不高**
- 标签文本数量太少，调整fasttext.train_supervised(input="etl_text.txt",lr=0.05,epoch=5,wordNgrams=5)参数值的效果并不大。
- 总结：fasttext适用于样本数据大的情形。


## 2020-08-18 :joy_cat:
1. 数据组周会
- 埋点：事件+属性，属性包括属性名、属性值、属性类型、属性备注。
- 一个页面对应多个事件，一个事件对应多个属性。
- 后端埋点：落地页支付、关注公众号。
- [神策代码埋点与可视化全埋点分别的适用场景](https://manual.sensorsdata.cn/sa/latest/%E4%BB%A3%E7%A0%81%E5%9F%8B%E7%82%B9%E4%B8%8E%E5%8F%AF%E8%A7%86%E5%8C%96%E5%85%A8%E5%9F%8B%E7%82%B9%E5%88%86%E5%88%AB%E7%9A%84%E9%80%82%E7%94%A8%E5%9C%BA%E6%99%AF-7547408.html)
2. 文本的有监督训练
- 文本预处理一：中文jieba分词，查寻停用词文件(cn_stopwords.txt , baidu_stopwords.txt)合并，去除业务特定词
- 文本预处理二：标签格式符合fasttext形式，带有__label__前缀
- 文本预处理三：将两列的dataframe输出为txt文本，标签和文本以行写入。

- **学习os.walk(dir_path, topdown=True)**  

- **pycharm的代码目录管理，使用Linux的'../path'表示path的上级目录。windows和mac两种操作系统的文件目录形式不一样，前者是\，后者是/**

- **停用词代码处理`if not(word.strip() in stop_words_list) and len(word.strip())>1:`，其中涉及列表与字符串序列的转换，即str.split(' '),' '.join(list)**


## 2020-08-17 :crying_cat_face:
1. 今天是2020年的第34周周一。2020余额不足一半了。
2. 业务周会。
3. 文本语义的有监督训练。
- 精读fasttext有监督训练的[text classification技术文档](https://fasttext.cc/docs/en/supervised-tutorial.html)
- 对于业务的文本数据，采取如下方案：
  - 数据预处理：中文文本进行分词，制定停用词；去除标点符号
  - 将原始文本输出fasttext的符合格式的文本，标签带__label__
  - 文本集划分为训练集、测试集
  - 调整fasttext.train_supervised()的输入参数
  - 结合准确率、召回率选出最优参数
- 难点1：文本和标签都是句子，如何精简是个难点。
- 难点2：标签样本量过小，如何解决。

## 2020-08-15 & 2020-08-16 :smiley_cat:
1. 整理本周的浏览器未读资料
2. 阅读《growth hacker》一书，初读一遍，目前进度50%。

## 2020-08-14 :smile_cat:
1. 对流失率看板数据，排查原因
- 有两个看板都有相关数据，但数据不对，排查原因
- 查看tableau的SQL逻辑，排查出原因：使用的表有字段active，限制active=1才是正确的添加人数；再加上企业微信的使用，表数据也会有缺失
- 排查完毕，将问题以issue反馈给责任方
2. 基于聊天记录的有监督训练
- 此前的聊天记录，有标签，因此，尝试使用fasttext做text classification
- 尚未完成

## 2020-08-13 :scream_cat:
1. 与运营策略方讨论用户生命周期的实际方案。
- 意义需要：数据规则来对用户分层，制作通用型数据模板，结合运营策略供参考
2. 重做流失率看板
- 外部因素：wehub被封，企业微信的引入，新的数据表。
3. 跟进产品内容的迭代。
- 这次迭代与上次相比效果如何。
- 这次迭代的意义在哪？
- 需要哪些数据支持？

## 2020-08-12 :smirk_cat:
1. 思考用户生命周期意义。结合运营，提高CLV。
2. 确定初版数据方案。步骤是：
- 整理用户路径
- 制定数据指标
- 对不同分层用户配合不同的运营策略（还需与运营方讨论）

## 2020-08-11 :heart_eyes_cat:
1. 完成tableau漏斗制作的多种方案，及显示实现。
- 漏斗每个指标的数量是必须的
- 占比可以由SQL计算，也可以有tableau函数计算。
- 占比计算方式，全局占比分母固定，相对占比采用表计算函数
2. 业务的用户生命周期数据方案初稿
- 推导Clv计算公式还是挺有意义。
- 提高CLV等价于提高转化率。

## 2020-08-10
1. 准备会议材料
2. 开会，做会议小结
- 埋点：页面、事件、属性、触发时机、埋点方式
3. Tableau漏斗问题研究
- 一种是SQL计算出漏斗字段和数值
- 另一种是SQL计算基础数据，tableau中计算漏斗字段和数值
- 前者在tableau更快捷，且计算字段简便。

## 2020-08-08 & 2020-08-09  :dizzy:
1. 杂闻记录
- 2008年8月8号，北京奥运会开幕，那一年我上高中，转眼已过去12年。
- 美国新冠患者出院费用高达百万美元；医疗保险报销比例即使80%，也是一笔天价；美国低收入人群免费申请医险；美国有破产保护，租户可以申请破产保护bp房东。
2. 更新“每日一问”
3. 整理本周遗留的浏览器未读网页，包括python文本分析。大都写在有道云笔记里了。


## 2020-08-07 :heartbeat:
1. 学习用户生命周期，着手数据模型
- 用户生命周期，围绕**what\why\how**去了解
- 结合自身产品路径，开始着手用户生命周期数据模型
2. 更新“每日一问”分栏

## 2020-08-06 :see_no_evil:
1. 完成后续的聊天文本处理
- fasttext适用于对较大的语料库做语义训练，不适用本场景的数据量。
- jieba库，调整字典以过滤特定词。
- 若对一个用户的多条聊天记录做tfidf关键词提取，并不能分析出语义（语义是目的）；因此，尝试按用户将该用户所有聊天数据聚合，使用df.groupby('user_id')['聊天'].apply(lambda x:x.str.cat(sep=' '))完成分组的字符串拼接，对聚合后的数据做tfidf关键词提取，也不能反映语义。
- 应该尝试其他的语义分析方法。但由于用户量不大，没有预先的标签集，人工分析提取标签集也是一个方案，为后续的分析打基础。
2. 沟通“用户生命轨迹”的需求背景
3. 更新“每日一问”分栏

## 2020-08-05 :muscle:
1. 处理聊天文本
  - 目的明确：通过文本分析，期望得到用户痛点和产品信息。
  - 步骤：首先清洗无效数据，然后计算tfidf找出top5词。根据前两步结果迭代，直至清洗的数据有较佳的分词结果。随后聚类，主题归纳。
  - 在清洗时，使用了正则表达式，re.search()与re.match()有区别。
  - 当re.search()在字符串中查找不到正则表达式模式时，返回None。  
  **DataFrame对象，在判断行是否为None时，不是用==，而是用df.isna()或pd.isna(df)**
2. 对接业务需求，多方参与
  - 当业务方提的数据需求因为所用数据在当前数据库不存在或存在错误的情况下，如何去对接接下来的需求呢？
  - **与业务方说明数据现状，由业务方找产品经理说明情况，由产品经理规划好后给开发提数据需求。等待数据能正常使用后，最终由数据方完成业务方数据需求**
  - 今天的对接流程大抵如此，有数据、业务、产品、开发一起参与。由产品提给开发的需求往往更加全面周到。
3. 新闻
- 华为天才少年计划四人公布，有三人来自华科母校。`努力真的很重要，当懂得了这个道理却已错过最有机会的时机。现在的努力，上限有限，远远达不到过去一直努力的高度。`
- 清华姚班硕士放弃Google工作机会，回山西二本教书，朋友替之征友。 `有才华的人的这种选择，如果父母亲人能支持这种想法，真的是一件幸福的事`
- 海归女研究生回家包田种地，遭职责。  `这种果敢，逆社会主流的勇气实在是佩服`
- 南京女大学生被男友在云南边境杀害。 `识人不只是表面。以前看到一句话，女孩子攀不上有钱人，女孩子看不上老实人，最后女孩子都去哪了呢？都选了没钱但不老实的人。`
- 阿里内网离职贴写到“阿里巴巴不需要年轻人”。   `涉及到资本的本质`
4. 更新"每日一问"分栏

## 2020-08-04 :runner:
1. 开组会，收获有：
- 新且小的项目，善用excel表格来管理需求，不需要额外的开发来开发项目管理系统
- 学习其他同事的tableau看板，总结来说：
  - **SQL能清洗的越深，tableau用较少的计算字段就能完成；反之，SQL只完成数据提取，未过多清洗，tableau则需要添加很多的计算字段**
  - *SQL函数有限不能完成的清洗，只能放在tableau中，此前做过手机号脱敏处理，使用tableau的regexp_replace()函数，SQL中没有此函数*
  - ***tableau实现漏斗的数值和百分比呈现有很多种方式，关键在于转化率的定义***
2. 与业务方对接需求，对聊天记录使用python做初步处理，大大减少了分析的数据量。
- 后续需要做语义分析，大概率使用fasttext的有监督学习。
3. 新增***每日一问.md***，多问点，多想点。

## 2020-08-03 :fire:
1. 在tableau上完成明细数据的聚合，直观辅助产品迭代
- 添加参数，以满足交互性、实时性
- 使用表计算完成分区合并百分比
- 表计算有两种实现，一是度量字段添加表计算，二是创建字段使用表计算函数。
2. 日常开会

## 2020-08-01 & 2020-08-02 :fist: :+1:
1. 七月结束了，八月开始啦。两个要重视的点：
- `学习要有计划，不是乱糟糟的东学一点，西逛一下`
- `一边学习的同时，也要保证结果产生`
2. 整理电脑桌面资料
3. 其他以前拖延没去落实的事项，个人方面的


## 2020-07-31 :tired_face:
1. 产品内容迭代，没有特别好的思路，带着问题与产品研发人员沟通：
- 产品迭代痛点：当前完课率指标不够好，通过数据和电话访谈明确知道产品的改进点
- 产品迭代方案：产品内容框架不变，在内容丰富程度、内容进度上迭代。
- 数据在其中能做什么：详细的资源数据指标的聚合，粒度太大，不能定位异常资源；粒度太小，分析起来不易。粒度适中。
- 下周对最细粒度的数据聚合，以看板形式呈现，帮助产品迭代。

## 2020-07-30 :dizzy:
1. 宽表代码重构中断，原因是：
- 对于这块，主要与数据开发沟通，问题出在了replace into。
- 增量查询需要保证一个事情，对新增用户插入时原数据不能删除。但replace into插入的表缺少唯一主键。
- 后续在看看代码。
2. 与转化率负责人讨论业务，对过去一周的转化率做讨论。
3. 下班时间了解知识，如知识图谱，贝叶斯预测等。

## 2020-07-29 :purple_heart:
1. 关于睡眠起床时间问题
- 7.28晚上十一点就睡了，7.29凌晨五点就自然醒了。自然醒的。现在2020年7月29日05:39:37
- 以往转钟后睡，七个小时都睡不够
- 难道深度睡眠的作用吗？早睡，深度睡眠占比更多？
2. 迭代昨天完成的tableau看板
- 新增期数维度，更改数据源的每个SQL。
- `cast (user_id as varchar) user_id ` 完成字段类型转换
- 看板外观优化。
3. 宽表代码重构思路
- 当前的宽表是replace插入，不会清空插入；数据量大采用全量更新效率低
- 重构思路是增量查询，限制关键SQL。
4. 一些其他杂项：
- 大数定律和小数定律
- 《思考，快与慢》很有意思的一本书
- 加特洛夫事件：灵异登山死亡事件，未解之谜
- 商品的非整数定价


## 2020-07-28 :stars:
1. 完成tableau看板制作。
- MongoDB数据同步到hive，presto连接hive，presto SQL完成数据简单清洗提取
- 使用lead() lag()开窗函数，取当前记录前后n行，[参考资料](https://blog.csdn.net/kent7306/article/details/50441967)
- 开窗函数比自联结更高效
- presto SQL 开窗函数：https://prestodb.io/docs/current/functions/window.html
- presto计算时间差，字符串解析为timestamp类型。 `date_diff('second', date_parse(temp1.triggered_at, '%Y-%m-%d %H:%i:%s'), date_parse(temp1.next_t, '%Y-%m-%d %H:%i:%s')) `
- Tableau在一个仪表盘中可以添加多个数据源，利用多个数据源可以完成不同SQL的相同主题看板。

## 2020-07-27 :clock1:
1. 完成Tableau业务看板
- hive和mysql数据源，在tableau中联结
2. 周一日常两个会议


## 2020-07-25 & 26 :alarm_clock:
1. update resume
2. 整理过去的笔记本，按时间编号，现在到了`No.07`
- 温故而知新。
3. 一周新闻
- 杭州失踪女士分尸案。
  - 半路夫妻，同床异梦，杀害分尸，贼喊捉贼，法网恢恢
  - 不能直视的只有烈日和人心
- 中方撤出在美领事馆，美方破门而入
  - 不可告人之密，欲行欲加之罪


## 2020-07-24 :waxing_crescent_moon:
1. 完成业务指标梳理初版，后续根据数仓工程师反馈修改
2. 完成业务用户内容行为数据的提取，简要分析，尚未完成
- dataframe.groupby()
- series.value_counts(normalize=True)
3. Tableau连接hive和mysql两个数据源制作样例看板
-  SQL类型转换：cast(user_id as varchar) 
-  一个SQL里不能跨数据源，但可以将两个数据源的SQL在tableau中连接。

## 2020-07-23 :hammer:
1. 梳理所负责业务指标，给数仓工程师开发DM层。
- 包括市场指标、服务指标、学习指标、社群指标等
- 后续还需根据数仓工程师迭代修改

## 2020-07-22 :flags:
1. 完成业务用户选择分支数据提取，待分析。
- 预分析：选定数据版本，期数，有利于分析人群特点的选择题，等等
- presto sql
- python dataframe清洗处理

2. 阅读《精益数据分析》的UGC内容。
- 漏斗
- 指标
- 用户参与度


## 2020-07-21 :baby_chick:
1. 完成业务SQL，群聊和私聊记录查询。
2. 巩固学习“数据分析指标”学习
- 用户指标
- 行为指标
- 指标建模

## 2020-07-20 :koala:
1. 写业务SQL，查询群聊记录。三点限制条件：
- 一创群时间限制
- 二用户加群时间限制
- 三聊天记录时间限制
2. 开一下午会，业务的同步工作还是有收获
3. 很喜欢《赤伶》这首歌，评论里有一个很虐的故事：
	```
	她是青楼名妓。一曲琵琶弹得人断了肠，一首小曲唱得人丢了魂。长安城里的人都知道这是个美人，很多人不惜掷千金为博她一笑。他是落魄书生。十载寒窗己知，无功无名谁问。天降小雨，书生无伞，浑浑噩噩的走在这长安城下，好不不是一番可怜景象。
	兰亭处。“这天下怎无我留身之处？”书生擦拭着身上的雨水。“这位公子，进来喝杯热茶暖暖身子把。”兰亭相遇，他是落魄书生，她是青楼名妓。一眼相遇，便是一颗心思只为卿。端茶倒水，她的一举一动都是那么的小心翼翼，那么的认真。书生看着她，不仅入了迷。
	“公子，公子，喝茶，”她端着茶轻声说到，“公子一直看着我干嘛，我这妆花了吗？”“不不不，这妆，真好看。”“公子说笑了。”“敢问公子大名，来这所谓何事啊。”“公子不敢当，小生名千寻，姓秋，想进京赶考。哈哈。”说最后一句话时，明显语气弱了几分。
	“秋公子，你先喝茶暖身子，我为你奏一曲如何。”“小生谢过姑娘。”琵琶声响，轻拢慢捻抹复挑，每次拨弦都恰到好处，余音绕梁，不绝于耳。琵琶声停。
	“敢问姑娘，是哪家的千金，有这般手法。”“我，只是这青楼一妓女，弹得一手好琵琶罢了。”她低下头，手摆弄着裙角，好似受了莫大的委屈。“姑娘，姑娘，小生没有什么其他意思，我能为描一下眉吗，刚刚被外面的雨，打湿了。”“嗯嗯。”“对了，还没问姑娘芳名呐。”“小女子，原名叫若一心。”“愿得一人心，白首不分离。好名字。”
	自此便是，他为她执笔描眉，她为他素手为羹。他说，待我金榜题名时，十里红妆，不负卿。她巧笑嫣然，却将许诺牢记于心。数日之后，他要走了，大事可耽误不得。他这一走数月，她每天望眼欲穿。她散尽家财，只为他能金榜题名，光明正大的取自己为妻。
	又过数日，发榜的时候到了。若一心不识字，却只记着他的名字。秋千寻。金榜题名，她在楼里大宴宾客替他高兴，众姐妹一一祝福，都羡慕她如此好命。她翘首以盼，等他迎娶。却不见半点消息，若一心派人去打听发生了什么事。
	来的人说他将要迎娶公主，大婚当日。若一心托人带信给他，问可还记得，当初许诺，待你金榜题名时，十里红妆，不负卿。送信之人不日便回，还带回了他的回信，苍劲有力的字迹，写的却是字字诛心的话。
	你不过一青楼名妓。一双玉臂千人枕，半点朱唇万人尝。以尔青楼素女身，怎配红袍状元郎。好一个半点朱唇万人尝，怎配我这状元郎。
	大堂内，一对新人正在叩拜。一拜天地。对不起。二拜高堂。负了你。夫妻对拜。我爱你。
	只有秋千寻知道，自己金榜题名，准备荣归故里，娶她为妻，却不料，公主看上了自己，龙威难抗，这公主又娇生惯养，嫉妒心强，怕她会对若一心不利。愿这份绝笔信，虽字字诛心，却能换她半生无恙，也好过红颜无命，白骨空鸣。
	```

## 2020-07-19 :fist:
1. 写了个周记，对自己充满信心、保持期待。
2. 看b站数据分析方法视频


## 2020-07-18 :snail:
1. 做家务（衣服被子床单，洗晒，清理柜子，下蟑螂药）
2. 吃好 睡好，卷腹锻炼 :pig:

## 2020-07-17 :joy_cat:
1. 对接业务需求--聊天信息相关。实际SQL过程中，数据表存在脏乱的情况，得手动修正。
2. 学习[产品经理角度的数据分析视频](https://www.bilibili.com/video/BV1E4411d7i2?p=4)
3. 一些有意思的新闻
- 武汉科协比赛两名小学生研究茶多酚抗肿瘤研究获奖。这是近期第二个这样的新闻，前一个是云南某个小学生“结直肠癌基因研究”获奖。现在的小学生太厉害了，网上出现一个新词`学阀`。
	```知乎网友 万能模板
	此案发生后，____ 高度重视，立即连夜召开____会议。
	____立即作出____，要求组织____，妥善处理___，迅速查清____，严肃追究____；立即组织开展_____，进一步做好____，防止_____。
	目前，____情绪稳定。____深感内疚，自责，痛心。
	```
- dilidili创始人被批捕后，微博被员工diss。
- 股市风云。
  - 赚不到超出自己认知的钱。
  - 散户占交易量的90%，却只占市值的25%。顶层的富人们支配巨额资产。
  - 牛市是散户最容易赔钱的地方。
- 豫章书院案件，主犯被判有期徒刑不到三年。表面打着戒网瘾、治疗心理的旗号，实则是无底线的虐待、凌辱青少年的，不知道这些送孩子去的父母知道真相后是如何的滋味。


## 2020-07-16 :construction_worker:
1. 制作tableau仪表盘，知识点总结如下：
- 维度和度量。tableau系统会自带的将数值型字段归到度量，若不注意维度与度量，直接把该字段拖拽到行列功能区，自动聚合（一般是总和）的结果与预想的不一致。
- [维度筛选器和度量筛选器](https://help.tableau.com/current/pro/desktop/zh-cn/filtering.htm#%E7%AD%9B%E9%80%89%E5%88%86%E7%B1%BB%E6%95%B0%E6%8D%AE%EF%BC%88%E7%BB%B4%E5%BA%A6%EF%BC%89) 。维度筛选器有“仅相关值”直接选择，维度筛选器则没有，需要将维度筛选器设为连续才有“仅相关值”选取（离散 则没有）。
- 字段数据类型。
  - 字段“更改数据类型”，可以根据实际场景确定，是数字还是字符串。
  - 另一种方式，计算字段时使用转换函数，如 STR()、DATE()、DATETIME()、INT() 和 FLOAT()。
- tableau合并字段。
  - 使用加号 (+) 运算符合并两个字符串字段，[参考资料](https://kb.tableau.com/articles/howto/combining-two-string-fields?lang=zh-cn)。如[string1]+[string2]
  - 特定字符串合并，也是+。如"abc" + " " + "def"，合并的结果为"abc def"

2. tableau技巧总结：多个字段，聚合一个字段的最大值，得到另一字段的值。（类似于一字段满足条件的另一字段值）
- 以一个用户的数据为例，status为1表示已完课，为0表示未完课。对于示例数据，目的是找出用户的最近学习进度，聚合结果应该为 `L10未完课`。

	| created_at | 关卡数字 | status |
	| :--- | :---: | :---: |
	| 7/14 | 8 | 1 |
	| 7/15 | 9 | 1 |
	| 7/16 | 10 | 0 |
	
- 如何实现呢？
- 若使用if创建字段col1。`if [status]=1 then '已完课' else '未完课' end ` 该字段col1并未实现聚合。
- 若聚合col1，使用max(col1)，得到的结果是`9已完课`。因为`9已完课`>`8已完课`>`10未完课`，字符串比较并不是整数的比较。
- **最后实现的关键是：先找出最大数字，再拼接字符串；而不是先拼接字符串，再比较出最大字符串。**

- 若使用created_at字段，取最晚时间的关卡数字，`if [created_at] = max([created_at]) then [关卡数字] else null end`。tableau报错：if不允许非聚合和聚合做比较。

- 解决方案：结合所有相关字段，建立中间字段，满足if的聚合与聚合的比较。
	- 建立字段col2。`if [status]=1 then [关卡数字]+1 else [关卡数字] end` 为停留关卡号。
	
		| created_at | 关卡数字 | status | col2 |
		| :--- | :---: | :---: | :---: |
		| 7/14 | 8 | 1 | 9 |
		| 7/15 | 9 | 1 | 10 |
		| 7/16 | 10 | 0 | 10 |
	
	- 目标字段target。 `if max([col2])-max([关卡数字])=1 then 'L'+STR(MAX([关卡数字]))+'已完课' else 'L'+STR(MAX([关卡数字]))+'未完课' end`
	- max()函数，不只是用在最外层，上述计算target字段也是聚合。

3. 业务仪表盘
- 更多交互式的操作本次未使用，如分层下钻、参数等。
- 部分指标还可以优化。
  ![业务仪表盘](https://github.com/yxhust/work_diary/blob/master/Snipaste_2020-07-16_21-15-09.png)




## 2020-07-15 :droplet:
1. 完成看板需求的业务SQL
- 如何提取字段值如'L7.掌握OKR， 让大目标落地'中的数字呢？
- 数据表有脏数据，需要人为修正

## 2020-07-14 :pushpin:
1. wordcloud.WordCloud()类的源码分析
	```
	def generate_from_frequencies(self, frequencies, max_font_size=None):  # noqa: C901 使用词及词频字典，返回self
	def process_text(self, text):   将文本分成词，返回词及词频字典
	def generate_from_text(self, text):  使用process_text处理text，将其输出再使用generate_from_frequencies
	def generate(self, text):    等同于def generate_from_text(self, text):        
	```
- 使用generate()方法，会将文本分词。文本也可以是jieba.cut（）的结果。
- 对于关键词文本的词云生成方法，要关键词文本按关键词频率降序排列，这样的生成词云是ok的。
2. 使用fasttext.train_unsupervised(path)训练文本时，结果仅为一个词汇的情况。经过排查：
- [In order to compute word vectors, you need a large text corpus.](https://fasttext.cc/docs/en/unsupervised-tutorial.html#getting-the-data)
- 换一个更大的文本语料库，同样的代码，训练结果可观。
3. 正则表达式替换
-  `pay_df.apply(lambda x:re.sub(string=x,pattern=r'[\\r\\n没有无理想\s]',repl=''))`
- 替换\r \n 及 所有中文字符



## 2020-07-13 :beginner:
1. 一直觉得7月13是什么日子，但记不得，查历史上的今天，有：
- 国际奥委会于这一天宣布2008北京举办奥运会
- 中华人民共和国卫生部叫停精神病医生杨永信所采用的电刺激
- 唐睿宗李旦逝世
- 由北京飞往莫斯科的苏联民航12号班机在伊尔库茨克坠毁，导致33人死亡
2. 正则表达式
- 替换字符串中的\r或\n
- pay_df.apply(lambda x:re.sub(string=x,pattern=r'[\\r\\n]',repl='\n'))
- \后面接啥都有含义，因此即使是原始字符串中，要匹配\本身，也得写作\\，没有其他的好方法
3. wordcloud生成词云的原理研究
。

## 2020-07-12 :large_orange_diamond:
1. 理解bilibili的业务及指标体系
- Z世代群体：1990~2000的人口，3.28亿，占总人口24%。bilibili认为该人群：深度用户渗透、愿意花时间、很强的付费意愿
- 运营模式：UP主 ，社区
- 业务：手游，直播，电商，广告，增值服务
- 指标体系：根据业务来建立
- 与爱奇艺传统视频播放网站最大的区别：爱奇艺的用户群体粘性不高，没有用户社区；业务主要是广告和增值服务，广告很影响观看体验；版权费用很高。
2. 做一些python练习

## 2020-07-11 :diamond_shape_with_a_dot_inside:
1. 睡够了
2. 整理过去一周的浏览器未关闭网页，如开窗函数、python文本分析文档、停用词、制作词云步骤整理等等，主要是梳理工作。

## 2020-07-10 
1. 记录安装fasttext填坑之路
- 根据[fasttext官方文档](https://github.com/facebookresearch/fastText/tree/master/python) 安装，报错：error: Microsoft Visual C++ 14.0 is required.
- 查询，根据[github issue资料](https://github.com/benfred/implicit/issues/76) ，需要安装vs，其余的特定包也不一定需要安装vs，但fasttext必须得安装vs
- 根据报错信息的vs网址，下载，看到安装c++生成工具，5G大小，选择放弃，寻求替代方案。
- 替代方案，下载离线whl文件，pip安装。[安装fasttext](https://blog.csdn.net/weixin_44388679/article/details/88937700)
- [python第三方安装包](https://www.lfd.uci.edu/~gohlke/pythonlibs/#pandas)
- 下载whl文件很轻松，但pip安装报错。ERROR: fasttext-0.9.2-cp36-none-win32.whl is not a supported wheel on this platform.
- 开始了解whl知识，排查原因，最后的解决方案是:`pip install E:\Anaconda3\Anaconda3\Lib\site-packages\fasttext-0.9.2-cp37-cp37m-win_amd64.whl` 
- 总结原因，whl文件要与python版本一致，不是向下兼容，我的python是3.7，下载的3.6和3.8都会报错。
- 安装fasttext最佳资料： https://blog.csdn.net/weixin_44388679/article/details/88937700

2. fasttext使用总结
- [无监督训练:word representation](https://fasttext.cc/docs/en/unsupervised-tutorial.html) ：从文本中训练 词向量
- [有监督训练:text classification](https://fasttext.cc/docs/en/supervised-tutorial.html)
	```
	With a few steps, we were able to go from a precision at one of 12.4% to 59.9%. Important steps included:

	preprocessing the data ;
	changing the number of epochs (using the option -epoch, standard range [5 - 50]) ;
	changing the learning rate (using the option -lr, standard range [0.1 - 1.0]) ;
	using word n-grams (using the option -wordNgrams, standard range [1 - 5]).
	```

## 2020-07-09 :beginner:
1. 使用f.write(i)方法写入文件时报错`'gbk' codec can't encode character '\U0001f4b0' in position 5: illegal multibyte sequence`
- 什么是encode？decode呢？ https://segmentfault.com/a/1190000015788943 （这篇文档是python2的，完全不适用现在的python3，得亏钻研了，要不会学习到错的知识）
- gbk编码是什么？ 用于简体中文
- 解决方案：打开文件对象制定encoding参数，而不是使用windows默认的编码方式

2. 字符编码知识
- [ascii,unicode,utf-8 通俗易懂的资料](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)
- encode() 和 decode()在python2和python3中是相反的，查阅资料注意辨别。因为在python2中有unicode类型，[参考资料](https://www.cnblogs.com/evening/archive/2012/04/19/2457440.html)
- 现在只考虑python3中:
  - str.encode(encoding='UTF-8',errors='strict')：将字符串str按UTF-8规则编码，返回bytes对象
  - bytes.decode(encoding='UTF-8',errors='strict')：将bytes对象按UTF-8规则译码（解码），返回str对象
3. 回到报错信息，写入文件时，报错gbk不能编码，涉及到了`文件编码`
- [资料1](https://blog.csdn.net/jim7424994/article/details/22675759) 的分析思路特别好，但部分知识为python2，已过时。
- [资料2](https://www.cnblogs.com/cwp-bg/p/7835434.html) ，文中分析后结论是：这是在文件写入的时候报的错误，而windows打开文件默认是以“gbk“编码的。
- 写入文件的具体操作是什么，编码解码是怎么进行的？这里面的学问很深啊。暂不去深究，`记住，报错涉及到编码，记得open文件时带上encoding参数`
4. 文本处理方式
- 关键词提取，jieba
- 词云，wordcloud
- 语义分析，LSA，fasttext（明天接着去深入）


## 2020-07-08 :bulb:
1. 完成职业的平均获客成本
- 基本逻辑：使用职业人群的手机号，匹配来源于哪个公众号，再去匹配公众号的投放成本，计算职业人群的平均获客成本
- 问题：部分公众号会在不同日期投放多次，仅通过公众号匹配结果会不对，还需要结合日期
- pandas常用匹配方法：pd.merge()  df.join()  df1.merge(df2) 这些都是值相等字段匹配。现在的场景是：在用户填写日期之前的投放日期的公众号才满足匹配条件，类似SQL中的不等号联结条件
- 经过查询，使用[pd.merge_asof()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.merge_asof.html#pandas.merge_asof)
  - pandas.merge_asof(left, right, on=None, left_on=None, right_on=None, left_index: bool = False, right_index: bool = False, by=None, left_by=None, right_by=None, suffixes='_x', '_y', tolerance=None, allow_exact_matches: bool = True, direction: str = 'backward') 
  - 特点：只有左联结，且联结key必须是排好序的
  - by:在不等号联结条件使用前，进行by的字段左联结（与pd.merge的on参数类似）
  - on:left和right有相同的字段名。若没有，使用left_on/right_on，按left_on/right_on字段的值进行不等号匹配。`Furthermore this must be a numeric column, such as datetimelike, integer, or float. On or left_on/right_on must be given.`
  - direction='backward'，为默认值。`A “backward” search selects the last row in the right DataFrame whose ‘on’ key is less than or equal to the left’s key.` 翻译过来，右表的键的值应该小于或等于左键的值，且只取满足条件的最后一行。（这里也明白了，为什么on的键要排好序）
  	- A “forward” search selects the first row in the right DataFrame whose ‘on’ key is greater than or equal to the left’s key.
        - A “nearest” search selects the row in the right DataFrame whose ‘on’ key is closest in absolute distance to the left’s key.
  - allow_exact_matchesbool, default True。True允许on的左右字段值相等，False,不含等于情况。
  
2. 数据挖掘算法 evolution algorithm，b站视频粗略的过完了一遍。`明天开始去代码实践`
- genetic algorithm
- genetic program 树状结构
- crossover,mutution,selection，fitness function,evolvuable things
- 给我最大的震撼是：`适者生存，而非强者生存`


## 2020-07-07 :imp:
1. 使用pandas进行常规处理，`df[(df['col1']==value1)&(df['col2']==value2)]['col3']`取df中满足条件的行的列
2. 业务复盘会。明确产品定位、产品价值、目标人群后产品化。
3. 学习数据挖掘理论的ensemble learning，其中bagging的 random forests 、boosting的AdaBoost。


## 2020-07-06 :arrow_up:
1. 继续完善下钻特征的是否转化分析
2. 课程内容选择数据的取数（presto sql+pandas）分析
3. 学习推荐算法相关，有tf-idf、latent semantic analysis（term-document matrix 分解）、page rank、collaborative filtering(基于用户，基于项目，基于模型)。`当前只停留在了解，还未代码实操`

## 2020-07-05 :yum:
1. 学习了聚类算法，基于密度聚类，层次聚类（包括簇间距离的计算方式）
2. 学习了关联规则，如支持度、置信度（本质是条件概率），频繁项集，apriori算法，sequence pattern。
3. 跟着视频做笔记思考，能初步入门，但很缺实践。近两天，看完后续的十几个视频，`开始去代码实践操作，运用到案例中。`

## 2020-07-04 :sleepy:
1. 处理琐事，睡舒服了，晚上也好好地锻炼了。休闲的一天。

## 2020-07-03 :umbrella:
1. 在梳理数据分析的思路时，还是有点着急，导致没有选择最合适的分析思路。回过来来发现，又重走一遍路。
- 具体的，此前考虑到幸存者偏差，转化的特征分布和未转化的特征分布进行对比，这样的结论很不直观
- 后来呢，单独把特征列出来，比较转化和未转化数值。这样直接，且不受特征人群数量限制，每一个特征在转化和未转化的百分比之和都是100%。
2. 重制用户标签信息问卷，必须经过我的同意才能上线。
3. 维度下钻，特征的组合是一个分析的方法。 :pencil:
4. 总体的分布，与子集的分布，在正常情况下子集的分布会服从总体的分布。
5. 数据分析方法，数据分析理论，在学习后要去实践。 :symbols:


## 2020-07-02 :stuck_out_tongue_closed_eyes:
1. 今天主要做职业行业数据的清洗。聚类算法效果不明显，关键词匹配更加直接。但需要有一个标准的行业分类。
2. 数据源头能控制好，数据清洗会省掉很多事情；数据源头不控制好，数据清洗往往事倍功半。

## 2020-07-01 :sunflower:
1. 七月第一天，过去一个多月github坚持天天更新，已经养成习惯了。七月继续加油，每天进步一点点。
2. 身体很重要，继续坚持锻炼。
3. python层次聚类
   1. 方法一：使用scipy库,linkage()输入为稀疏矩阵或稠密矩阵，输出为(n-1)*4的矩阵，n为样本数量，共n-1次聚合，dendrogram()函数可视化聚类树状图。该方法在我的数据集上报错，因为数据深度超过限制。
   
			```
			import scipy
			from scipy.cluster.hierarchy import linkage,dendrogram
			import matplotlib.pyplot as plt
			Z = linkage(tfidf.toarray())
			dendrogram(Z)
			plt.show()
			```
   2. 方法二：使用sklearn库，[参考资料](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.AgglomerativeClustering.html#sklearn.cluster.AgglomerativeClustering)
   
			```
			from sklearn.cluster import AgglomerativeClustering
			c = AgglomerativeClustering(n_clusters=6).fit_predict(tfidf.toarray()) 
			#TypeError: A sparse matrix was passed, but dense data is required. Use X.toarray() to convert to a dense numpy array
			```
   3. 原始的职业数据 由用户填空而来，存在大量较短、频次较低的职业数据，仅依靠tf-idf计算可能不行，在计算tf-idf前需要过滤掉仅出现一次的词。
   4. 数据清洗未做好，聚类的结果也不理想。

## 2020-06-30 :stuck_out_tongue_closed_eyes:
1. jieba计算tf_idf，[参考资料](https://github.com/fxsjy/jieba#%E5%9F%BA%E4%BA%8E-tf-idf-%E7%AE%97%E6%B3%95%E7%9A%84%E5%85%B3%E9%94%AE%E8%AF%8D%E6%8A%BD%E5%8F%96)
- jieba.analyse.extract_tags(sentence, topK=20, withWeight=False, allowPOS=())
  - sentence 为待提取的文本
  - topK 为返回几个 TF/IDF 权重最大的关键词，默认值为 20
  - withWeight 为是否一并返回关键词权重值，默认值为 False
	- allowPOS 仅包括指定词性的词，默认值为空，即不筛选
- jieba.analyse.TFIDF(idf_path=None) 新建 TFIDF 实例，idf_path 为 IDF 频率文件
- 对于df['col']这种文本，即文本存储在Series里，这种方式不适合。

2. 制作词云:ok_hand:
- 使用jieba和wordcloud库
- 参考资料1：https://juejin.im/post/5b0f962751882515773af358  
- 参考资料2：https://amueller.github.io/word_cloud/generated/wordcloud.WordCloud.html#wordcloud.WordCloud

3. sklearn计算df['col']的tfidf
- sklearn.feature_extraction.text.CountVectorizer()
	- 功能：统计原始数据的词的频次
  - 代码：先实例化，再使用fit_transform(data)方法返回document—term matrix。data为可迭代对象，不能包含空值、非str
- sklearn.feature_exyraction.text.TfidfTransformer()
	- 功能：
	- 代码：先实例化，再使用fit_transform(X)方法。X可为fit_transform(data)的输出，`{array-like, sparse matrix, dataframe} of shape (n_samples, n_features)`
	- 输出：X_newndarray array of shape (n_samples, n_features_new)。与X类似，但值现在为tf-idf值，不再是{0,1}
- 多行数据样本，所有的词。计算出的tf-idf为n*m的矩阵。每行数据不出现的词的tf-idf为0。

4. 对于sklearn计算的tf-idf，如何层次聚类呢？层次聚类能否实现目标-将凌乱的填空数据统一化？

## 2020-06-29 :sunny::kissing:
1. 投放环节数据处理 
   1. jupyter notebook读取PG和MySQL数据，pandas处理，输出csv文件 :smile:
   2. csv文件呈现形式是，一个用户多个维度特征以多行展开:smirk:，即一对多关系。
   3. 要取出某个特征的数据很容易，dataframe行列操作即可。但想要特征下钻，如性别为男的用户的学历构成，dataframe里要：找到性别为男的用户，在从这些用户中找出其学历信息。这样的坏处是：处理效率低，代码繁琐。
   4. 实现特征下钻，需要改变csv文件格式，一个用户一行数据，一行数据包括多个维度，每个维度一列，这需要`行列转化+用户分组聚合`:scream:
   5. 使用tableau实现，如`max(IF [field_id]='field_9' then [value] else null end)`，类似于max(case when then else end)
   6. 输出新格式数据，数据透视表是个很好的数据分析工具。:grimacing:
2. 对于数据文本处理，计算tf_idf，层次聚类

## 2020-06-28
1. 数据透视表对于小数据量非常好用，[参考资料](https://zhuanlan.zhihu.com/p/36785151)
- 行，列，值，筛选
- 值字段设置，汇总方式、显示方式
2. 投放环节数据处理
- jupyter notebook 

## 2020-06-27
1. 蛙还是不要在外面吃，寄生虫太多，吃坏了肚子难受的不是一天。
2. 整理过去一周的未读资料。
3. 确定下周目标：学习完b站数据挖掘理论与算法视频。

## 2020-06-26
1. 出去跟老同学聚会，好像吃的蛙有点坏肚子OuO，休息一天。
2. 明天继续学习b站数据挖掘视频。

## 2020-06-25
1. 使用psycopg2模块查询pg库 sql
- 默认参数，查询结果的columns为默认索引0、1、2...
- pymysql中想查询结果为带有字段的columns，代码如下：
	```
	cur = connection.cursor(pymysql.cursors.DictCursor)
	或
	connection = pymysql.connect(host='localhost',
				     user='user',
				     password='passwd',
				     db='db',
				     charset='utf8mb4',
				     cursorclass=pymysql.cursors.DictCursor)
	```
- psycopg2中显然不是这样的，需要额外导入`import psycopg2.extras`，参考资料https://stackoverflow.com/questions/6739355/dictcursor-doesnt-seem-to-work-under-psycopg2 ,具体代码为：
	```
	cur = psycopg2.connect(database="xx", user="xx", password="xx", host="xx", port="xx").cursor(cursor_factory=psycopg2.extras.RealDictCursor)
	```

## 2020-06-24
1. 记录下PostgreSQL（简称PG库）首次查询遇到的问题。
- 写一个SQL `select count(*) from for_spyder.public.questionnaire_record` 报错信息如下：cross-database references are not implemented
- 根据报错信息，该SQL执行了跨库引用
- 查看当前database SQL，`select current_database()`，返回结果是postgres，默认数据库。
- 查看，使用datagrip连接PG库时确实没指定database，于是重新指定database，重新连接。多次尝试，还是出错。
- 查阅一些相关文档，学习一些PG库基础知识：
  - database.schema.table 该语法使用表
  - 简要阐述了database的结构和使用规则。[PG库官方文档](https://www.postgresql.org/docs/7.4/ddl-schemas.html)
	  ```
	    A PostgreSQL database cluster contains one or more named databases. Users and groups of users are shared across the entire cluster, but no other data is shared across databases. Any given client connection to the server can access only the data in a single database, the one specified in the connection request.
	  ``` 
- 这篇文章https://my.oschina.net/Kenyon/blog/90863 也给予我首次使用PG库的函数的提示。
- 最终的解决方案竟然是重启datagrip，`重启重启重启`。重启完current_database就切换过来了，刷新没用，刷新没用。

## 2020-06-23
1. 周会
- 职责界限不明显（数据异常排查）
- 项目排期不考虑数据方
- 话语权低，推不动项目
- 容易接受临时解决方案，无法解决长期问题
2. 投放数据分析，
- 投放效果受到很多因素影响，如特殊事件时间、素材、报名页、人群、渠道头条次条、推送时间、竞品推送等等。
- 我本意是想，通过过去两个月的全链路投放数据来分析哪些公众号渠道优质，哪些不优质，提取优质特征和不优质特征。但与投放同事沟通过后，她给我的反馈是，投放那边是根据经验进行人群投放，并不知道投放获客是否达到预期，投放更想知道用户画像特征。
- 我这边的思考结果是：公众号特征需要投放同事给出；公众号特征没法做到很精细；即使有精细的公众号特征，最影响投放效果的还是公众号人群；获取公众号人群特征，可以指导投放目标用户是谁，目标用户在哪，寻找目标用户就是投放同事的工作。
- 幸存者偏差：当前的数据，都是已获客群体的人群画像，这里存在幸存者偏差，未获客群体不在数据表里，该怎么去获取未获客群体的人群画像呢？
- 锚定效应：自己对于此前的工作有锚定效应，做完了一直认为一个结论（可能不是结论的结论），未去深入探索。应该去持怀疑、探索的态度，继续深入下钻，直至探索出结论。


## 2020-06-22
1. 投放数据处理，遇到很大的问题：
- 当多个指标都会影响到对一个维度的判断时，应该用什么方法去分析呢？
- 去搜集数据分析方法时，找到很多资料，如
  - 统计学知识，样本抽样，抽样分布，正态分布，假设检验
  - 逻辑树分析法、多维度分析法、对比分析法（锚定效应，心理学现象）、相关分析法（相关性，因果性）、群组分析法
- 制作了一些tableau数据看板，将表数据可视化，怎么处理还没确定好。



## 2020-06-21
1. 凌晨五点多就起来了，不禁感叹：这一天真长，这种感觉三月份在家远程办公有过，起得早一天做的事情都要多些。
2. 不能住一楼了，我本科和研究生都住的一楼，现在还是住一楼。今天做的一部分事情就是：洗发霉的衣服，晒衣服，整理收纳换季的衣服。做完这些的感觉就是：`堆积的事项终于清理完了，神清气爽`。
3. 整理过去一周的未完成知识。

## 2020-06-20
1. 写完周报
2. 写完个人总结

## 2020-06-19
1. 使用python处理完过去两个月的投放数据，主要指标有`转化率`和`ROI`。下一步：
	- 指标统计	
	- 挖掘优质公众号特征以拓宽公众号
	- 不优质的公众号挖掘特征，以避免再浪费投放金额
	- 挖掘转化人群特征，迭代产品卖点及投放素材、报名页
2. 数据来不及或者有效性未知，建议做小的对照实验。

## 2020-06-18
1. 对近两月的投放数据进行清洗，主要是excel和python
2. SQL里的排名和累计值问题其实是一样的，都可以用关联子查询来解决，排名是组内比它值大的个数(count)，累计值是组内值得累加(sum)
3. 这几天与业务同事沟通的比较多，感觉“结果不好，大家都很着急，饱受着互相指责”。可以从以下几个方面来落实：
	- 做好自己分内的事
	- 有数据做指引是最好，但这个我一个人做还需要时间，当前最重要的是投放和产品内容
	- 没数据的指引，按照经验做简单的对照实验
4. 淘宝618，卡着整点加码红包，竟然领到了无门槛70元的红包，真有狗运。`赶在前面还是好，走在后面汤都没了`

## 2020-06-17
1. 完成业务全环节文档
- 主要在与业务各环节沟通
- 如何整理出数据需求也是很费精力，按自定义模板来整理每个环节的数据需求：目的 > 实现方式 > 交付 > 备注

## 2020-06-16
1. 继续与业务部门负责人沟通，整理业务全环节文档
2. 周会
- 桑基图
- 序列波动： t=x-u/sigma;窗口函数

## 2020-06-15
1. 整理业务全环节文档，与业务负责人沟通，根据当前现状找出数据可以发挥价值的点
2. 学习决策树的ID3算法，信息熵，信息增益，信息增益率（引入惩罚量消除很多属性值的影响）。

## 2020-06-14
1. 自己动手做早餐
2. 更新简历

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
  


  
