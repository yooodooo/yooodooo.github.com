---
layout: post
title: Python 日期和时间
tagline: [python] 
group: Python
---
{% include JB/setup %}

# 日期和时间 #
在python中处理日期和时间主要有三个模块: `datetime,calendar,time`针对了不同的使用场景


## datetime ##
datetime模块提供了处理大部分情况下日期、时间的解决方案。比如日期、时间、时间间隔、时区等。主要包括下面的类

<table>
<tr><td>datetime.date</td><td>提供基于Gregorian日历下year,month,day的处理方案</td></tr>
<tr><td>datetime.time</td><td>时间理想化的处理(考虑一天是24*60*60的情况，没有闰秒),提供属性hour,minute,second,microsecond,和tzinfo</td></tr>
<tr><td>datetime.datetime</td><td>日期和时间的集合(包括date和time)</td></tr>
<tr><td>datetime.timedelta</td><td>时间间隔：date,time或datetime</td></tr>
<tr><td>datetime.tzinfo</td><td>时区抽象类，用于datetime或time</td></tr>
<tr><td>datetime.timezone</td><td>tzinfo的一个实现类,基于UTC</td></tr>
</table>

