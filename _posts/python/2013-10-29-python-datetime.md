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

## 1.  datetime.date ##

表示了一个日期对象，其构造函数为`datetime.date(year, month, day)`其中各个参数需要满足

- MINYEAR <= year <= MAXYEAR
- 1 <= month <= 12
- 1 <= day <= number of days in the given month and year

下面这个例子展示了一些常用的用法

	def datetime_date():
		one_day = date(2013, 10, 28)
		# 返回当前本地时间，与date.fromtimestamp(time.time())一样
		today = date.today()
		another_day = date.fromtimestamp(time.time())
		# 返回日期的ISO格式： ‘YYYY-MM-DD’
		print one_day.isoformat() == another_day.isoformat()
		print today == another_day
		# 返回日期的字符串格式: Mon Oct 28 00:00:00 2013
		print one_day.ctime()
		# 根据格式返回日期的字符串http://docs.python.org/2/library/datetime.html#strftime-strptime-behavior
		print another_day.strftime('%Y-%m-%d')
		print another_day.strftime('%Y-%m-%d %H:%M:%S') #当然这句完全没有必要，需要显示时间信息应该用datetime
		
		# 返回time.struct_time((d.year, d.month, d.day, 0, 0, 0, d.weekday(), yday, -1))
		dt = today.timetuple()
		for i in dt:
			print i
			
		# 返回(ISO year, ISO week number, ISO weekday)
		dtt = today.isocalendar()
		for i in dtt:
			print i

