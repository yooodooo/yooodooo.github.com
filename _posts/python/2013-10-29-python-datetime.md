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

<table class="table table-striped table-bordered">
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

## 2.  datetime.time ##

表示了一个时间对象，与日期对应，这里表示小时、分、秒等信息。其构造函数为`datetime.time([hour[, minute[, second[, microsecond[, tzinfo]]]]])`

- 0 <= hour < 24
- 0 <= minute < 60
- 0 <= second < 60
- 0 <= microsecond < 1000000.

同样的，我们用例子来展示常见的用法

	def datetime_datetime():
		#实例化time对象
		cur_time = time(20, 1, 2, 100)
		#常见属性
		print cur_time.hour, '-', cur_time.second, '-', cur_time.minute, '-', cur_time.microsecond, '-', cur_time.tzinfo
		#返回字符串,ISO格式:HH:MM:SS.mmmmmm 
		print cur_time.isoformat()
		#用给定的时间替换生成新的时间 
		new_time = cur_time.replace(13, 23) 
		#20:01:02.000100 --> 13:23:02.000100
		print new_time.isoformat()
		#根据格式返回日期的字符串
		print new_time.strftime('%H:%M:%S')
		
## 3. datetime.datetime ##

是date和time的结合，自然包括了两者的所有信息，其构造函数为`datetime(year, month, day[, hour[, minute[, second[, microsecond[, tzinfo]]]]])`
datetime中常用方法和属性

- datetime.today() 返回表示当前本地时间的datetime对象，与`datetime.fromtimestamp(time.time())`相同
- datetime.now([tz]) 返回表示当前指定时区的datetime对象，如果时区为None则与today()作用一样
- datetime.utcnow() 返回当前UTC日期和时间
- datetime.combine(date, time)：根据date和time，创建一个datetime对象
- datetime.strptime(date_string, format)：将格式字符串转换为datetime对象
- 包括date和time的方法

下面是基本的用法

	def datetime_datetime():
		#返回当前本地实际
		cur_dt = datetime.today()
		dt_now = datetime.now()
		#返回当前UTC时间
		cur_utcdt = datetime.utcnow()
		#根据日期和时间组合成一个datetime对象
		com_dt = datetime.combine(date(2013, 10, 28), time(20, 1, 2, 100))
		#根据timestamp返回UTC
		utc_stamp = datetime.utcfromtimestamp(ctime.time())
		print 'today: %s, now: %s, comb: %s, utcnow: %s, utcs: %s' % (cur_dt, dt_now, com_dt, cur_utcdt, utc_stamp)
		#根据date_string返回datetime: 日期字符串不对时会抛出ValueError
		str_dt = datetime.strptime('2013-9-28 12:12:12', '%Y-%m-%d %H:%M:%S') 
		print str_dt
		#常见属性
		print 'Hour: %s, Minutes: %s, Seconds: %s, Year: %s, Month: %s' % (str_dt.hour, str_dt.minute, str_dt.second, str_dt.year, str_dt.month)
		#来自date和time的方法
		print 'date: %s, time: %s, weekday: %s' % (str_dt.date(), str_dt.time(), str_dt.weekday())
		
## 4. datetime.timedelta ##

表示两个日期、时间间隔的时间段。通常与其他对象(date,time,datetime)使用，支持`(+,-,*)`操作等。其构造函数为:
`datetime.timedelta([days[, seconds[, microseconds[, milliseconds[, minutes[, hours[, weeks]]]]]]])`

	def datetime_timedelta():
		#一天后
		print date(2013, 10, 31) + timedelta(days=1)
		#一小时后
		print datetime.now() - timedelta(hours=1)
		#100天前
		print (datetime.now() - timedelta(days=100)).strftime("%Y-%m-%d")


 
## 5. strftime()和strptime() ##

日期格式

<table>
<tr><td>%a</td><td>	星期的简写。如 星期三为Web</td></tr>
<tr><td>%A</td><td>星期的全写。如 星期三为Wednesday</td></tr>
<tr><td>%b</td><td>	月份的简写。如4月份为Apr</td></tr>
<tr><td>%B</td><td>	月份的全写。如4月份为April</td></tr>
<tr><td>%c</td><td> 日期时间的字符串表示。（如： 04/07/10 10:43:39）</td></tr>
<tr><td>%d</td><td> 日在这个月中的天数（是这个月的第几天）</td></tr>
<tr><td>%f</td><td> 微秒（范围[0,999999]）</td></tr>
<tr><td>%H</td><td>小时（24小时制，[0, 23]）</td></tr>
<tr><td>%I</td><td> 小时（12小时制，[0, 11]）</td></tr>
<tr><td>%j</td><td> 日在年中的天数 [001,366]（是当年的第几天）</td></tr>
<tr><td>%m</td><td> 月份（[01,12]）</td></tr>
<tr><td>%M</td><td> 分钟（[00,59]）</td></tr>
<tr><td>%p</td><td> AM或者PM</td></tr>
<tr><td>%S</td><td> 秒（范围为[00,61]，为什么不是[00, 59]，参考python手册~_~）</td></tr>
<tr><td>%U</td><td> 周在当年的周数当年的第几周），星期天作为周的第一天</td></tr>
<tr><td>%w</td><td> 今天在这周的天数，范围为[0, 6]，6表示星期天</td></tr>
<tr><td>%W</td><td> 周在当年的周数（是当年的第几周），星期一作为周的第一天</td></tr>
<tr><td>%x</td><td> 日期字符串（如：04/07/10）</td></tr>
<tr><td>%X</td><td> 时间字符串（如：10:43:39）</td></tr>
<tr><td>%y</td><td> 2个数字表示的年份</td></tr>
<tr><td>%Y</td><td> 4个数字表示的年份</td></tr>
<tr><td>%z</td><td> 与utc时间的间隔 （如果是本地时间，返回空字符串）</td></tr>
<tr><td>%Z</td><td> 时区名称（如果是本地时间，返回空字符串）</td></tr>
<tr><td>%%</td><td> %% => %</td></tr>
</table>
