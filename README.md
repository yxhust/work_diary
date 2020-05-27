# 工作计划

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
  

## 附注2020-04-30
- [ ] 继续完成相关代码

## 附注2020-04-29
- [ ] 深度完善IDP入门课用户数据分析框架，即剖析需求，需要什么，我该做完什么
- [ ] 开始编写相关的Python代码

## 附注2020-04-28 
* [x] 分析IDP入门课用户数据分析框架
* [ ] [medium推荐文章](https://medium.muz.li/10-rules-of-dashboard-design-f1a4123028a2)

