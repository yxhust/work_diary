# 工作计划



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

