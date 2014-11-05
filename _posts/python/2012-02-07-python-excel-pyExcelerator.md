---
layout: post
title: python 操作excel之pyExcelerator
tagline: [python] 
group: Python
categories: [Python]
---
{% include JB/setup %}

pyExcelerator

>pyExcelerator is a library for generating Excel 97/2000/XP/2003 and OpenOffice Calc compatible spreadsheets. pyExcelerator has full-blown support for UNICODE in Excel and Calc spreadsheets, allows using variety of formatting features, provides interface to printing options of Excel and OpenOffice Calc.

支持生成excel97+,支持OpenOffice。

>Main advantage is possibility of generating Excel spreadsheets without COM servers. The only requirement -- Python 2.4b2 or higher. From version 0.5 pyExcelerator can import data from Excel spreadsheets.
最主要的优势是不依赖于Windows/COM接口，需要注意的是python2.4+
 
## 在ubuntu中的安装 ##

	sudo python setup.py install   

安装后，创建一个文件做简单测试：

	#!/usr/bin/env python  
	# -*- coding: utf-8 -*-  
	  
	from pyExcelerator import *  
	  
	w = Workbook()  
	ws = w.add_sheet('Hey, Dude')  
	w.save('mini.xls')  

执行：

	chmod a+x excel.py  
	./excel.py  

在当前目录下可看到文件`mini.xls`。
 
## pyExcelerator结构介绍 ##

在应用pyExcelerator的时候，需要导入模块

	from pyExcelerator import *  

进到该package中的信息，可以看到pyExcelerator的模块

	from Workbook import Workbook  
	from Worksheet import Worksheet  
	from Row import Row  
	from Column import Column  
	from Formatting import Font, Alignment, Borders, Pattern, Protection  
	from Style import XFStyle  
	from ImportXLS import *  
	from ExcelFormula import *  

这里可以看到主要的划分目录，这也是excel中常见的模块：`Workbook、Worksheet、Row、Column`。其他还包括样式`Formatting、Style`模块，读取excel模块ImportXLS，支持公式的模块`ExcelFormula`。同时，在安装目录下有文件夹examples包含大量的使用实例
 
## 生成一个excel的步骤 ##
1. 实例化Workbook

		wb = Workbook()  

2. 增加一个工作表Worksheet，获得工作表实例

		ws = wb.add_sheet('sheet1')  

3. 对Worksheet进行写数据、样式、合并等属性的操作

		#这里写入数据，指定到具体的行、列。style指定样式，包括num_format_str、font、alignment、borders等  
		def write(self, r, c, label="", style=Style.XFStyle()):  
		#合并单元格，指定起止的行、列  
		def write_merge(self, r1, r2, c1, c2, label="", style=Style.XFStyle()):  

4. 保存Workbook

		wb.save()  

	在写入的时候，如果当前写入目录同文件名的文件打开，会抛出异常，如：

		IOError: [Errno 13] Permission denied: 'mini.xls'
 
一个简单的写入数据到excel的例子

	w = Workbook()  
	ws = w.add_sheet('sheet')  
	#第一行作为header：注意是(0,0)作为第一行第一列  
	ws.write(0,0,u"姓名")  
	ws.write(0,1,u"年龄")  
	ws.write(0,2,u"班级")  
  
	data = [["aaaa",9,u"三年二班"], ["bbbb",9,u"三年二班"], ["cccc",9,u"三年二班"]]  
  
	#这里一般的处理是对数据循环，对应到sheet中的行列，写入数据  
	for i in range(len(data)):  
	    for j in range(len(data[i])):  
	        ws.write(i+1,j,data[i][j])  
	    i+=1  
	  
	w.save('mini.xls')  
 
## 写入特定样式 ##
在ws中的写方法提供了

	def write(self, r, c, label="", style=Style.XFStyle()):  

其中`style=Style.XFStyle()`就是定义各种样式的参数，`在Style.XFStyle()`的构造函数中：

	def __init__(self):  
	    self.num_format_str  = _default_num_format#数字或日期的格式  
	    self.font            = _default_font.copy()#字体  
	    self.alignment       = _default_alignment.copy()#对齐方式  
	    self.borders         = _default_borders.copy()#边框  
	    self.pattern         = _default_pattern.copy()  
	    self.protection      = _default_protection.copy()  

这里以写入数字格式为例。在Style.py中`StyleCollection`定义了`_std_num_fmt_list`，对支持的数字格式做了说明，如：

	_std_num_fmt_list = ['general','0','0.00','#,##0','#,##0.00','D-MMM-YY','D-MMM','MMM-YY',...]  

一般过程

	#实例一个style对象  
	style = XFStyle()  
	#设置相应的格式  
	style.num_format_str = 'D-MMM-YY'  
	ws.write(self, r, c, label="", style)  

一个写入数字格式的例子：

	from datetime import datetime  
	num_data = ["123", "123.1232", "123.12", datetime.now(), datetime.now(), datetime.now()]  
	fmts =     ['general', '0.00', '#,##0.00', 'D-MMM-YY', 'h:mm:ss AM/PM','M/D/YY']  
	  
	w = Workbook()  
	ws = w.add_sheet('sheet')  
	  
	for i in range(len(num_data)):  
	    ws.write(i+1, 0, num_data[i])  
	    ws.write(i+1, 2, fmts[i])  
	    #写入样式  
	    style = XFStyle()  
	    style.num_format_str = fmts[i]  
	    ws.write(i+1, 4, num_data[i], style)  
	    i+=1  
	  
	w.save('mini.xls')  

当然也可以设置字体格式

	style.font.name = 'Times New Roman'  
	style.font.struck_out = True  
	style.font.bold = True  
	style.font.outline = True  

其中，字体的更多设置见字体见Style.py中`_default_font = Formatting.Font()`pyExcelerator对excel的常见操作如合并单元格，冻结行列等都支持

## 合并单元格 ##

	from pyExcelerator import *  
	  
	#这里设置边框  
	borders = Borders()  
	borders.left = 1  
	borders.right = 1  
	borders.top = 1  
	borders.bottom = 1  
	  
	#这里设置对齐方式  
	al = Alignment()  
	al.horz = Alignment.HORZ_CENTER  
	al.vert = Alignment.VERT_CENTER  
	  
	style = XFStyle()  
	style.borders = borders  
	style.alignment = al  
	  
	wb = Workbook()  
	ws = wb.add_sheet('sheet1')  
	  
	#跨列合并  
	ws.write_merge(1, 1, 1, 5, u"这个合并(1,1)-(1,5)", style)  
	ws.write_merge(2, 2, 2, 3, u"这个合并(2,2)-(2,3)", style)  
	ws.write_merge(2, 2, 4, 5, u"这个合并(2,4)-(2,5)", style)  
	#跨行合并  
	ws.write_merge(2, 4, 1, 1, u"这个合并(2,1)-(4,1)", style)  
	ws.write_merge(3, 4, 2, 2, u"这个合并(3,2)-(4,2)", style)  
	ws.write_merge(3, 4, 3, 3, u"这个合并(3,3)-(4,3)", style)  
	ws.write_merge(3, 4, 4, 4, u"这个合并(3,4)-(4,4)", style)  
	ws.write_merge(3, 4, 5, 5, u"这个合并(3,5)-(4,5)", style)  
	#合并多行多列  
	ws.write_merge(1, 4, 6, 7, u"这个合并(1,6)-(4,7)", style)  
	  
	wb.save('merged.xls')  

这里跨行合并的时候需要注意，不知道出于什么原因考虑，在合并的时候并不支持对同一列的跨行合并，见Cell.py中MulBlankCell的构造方法：

	def __init__(self, parent, col1, col2, xf_idx):  
	    assert col1 < col2, '%d < %d is false'%(col1, col2)  
	    self.__parent = parent  
	    self.__col1 = col1  
	    self.__col2 = col2  
	    self.__xf_idx = xf_idx  

如果需要改功能，在源代码中注释掉这个断言即可 
 
## 冻结行列 ##

之前的操作是针对行列中某个写入的属性设置，比如Font、Alignment，如果设置冻结行、列则属于worksheet的操作。
在WorkSheet.py中Worksheet的构造函数设置了大量这样的属性，可以通过get/set方法进行设置。以下是对冻结行列的相关设置

	self.__panes_frozen = 0  
	self.__vert_split_pos = None  
	self.__horz_split_pos = None  
	self.__vert_split_first_visible = None  
	self.__horz_split_first_visible = None  

如：

	self.__panes_frozen = True  
	self.__vert_split_pos = 2  #垂直方向（列）冻结两列  
	self.__horz_split_pos = 2  #水平方向（行）冻结两列  

	ws4.panes_frozen = False  
	ws4.horz_split_pos = 20    #这个时候好像并不是指冻结20列  
	ws4.horz_split_first_visible = 4 #excel打开时初始显示在第四列，即excel的scrollbar显示在第四列的位置  

这个例子来自sample目录下：

	# -*- coding: utf-8 -*-  
	from pyExcelerator import *  
	  
	w = Workbook()  
	ws1 = w.add_sheet('sheet 1')  
	ws2 = w.add_sheet('sheet 2')  
	ws3 = w.add_sheet('sheet 3')  
	ws4 = w.add_sheet('sheet 4')  
	ws5 = w.add_sheet('sheet 5')  
	ws6 = w.add_sheet('sheet 6')  
	  
	for i in range(0x100):  
	    ws1.write(i/0x10, i%0x10, i)  
	  
	for i in range(0x100):  
	    ws2.write(i/0x10, i%0x10, i)  
	  
	for i in range(0x100):  
	    ws3.write(i/0x10, i%0x10, i)  
	  
	for i in range(0x100):  
	    ws4.write(i/0x10, i%0x10, i)  
	  
	for i in range(0x100):  
	    ws5.write(i/0x10, i%0x10, i)  
	  
	for i in range(0x100):  
	    ws6.write(i/0x10, i%0x10, i)  
	  
	ws1.panes_frozen = True  
	ws1.horz_split_pos = 2  
	  
	ws2.panes_frozen = True  
	ws2.vert_split_pos = 2  
	  
	ws3.panes_frozen = True  
	ws3.horz_split_pos = 2  
	ws3.vert_split_pos = 2  
	  
	ws4.panes_frozen = False  
	ws4.horz_split_pos = 20  
	#excel打开时初始显示在第四列，即excel的scrollbar显示在第四列的位置  
	ws4.horz_split_first_visible = 4   
	  
	ws5.panes_frozen = False  
	ws5.vert_split_pos = 40  
	ws4.vert_split_first_visible = 2  
	  
	ws6.panes_frozen = False  
	ws6.horz_split_pos = 12  
	ws4.horz_split_first_visible = 2  
	ws6.vert_split_pos = 40  
	ws4.vert_split_first_visible = 2  
	  
	w.save('panes.xls')  
 
## 设置行列高度 ##

对行高、列宽的设置可以通过对Worksheet的属性设置来改变。Worksheet中提供了对col和row操作的方法
对列宽的设置

	ws.col(i).width = 0  

行高没有相应的属性如height设置，可以通过style

	ws.row(i).height = 0  #但是测试好像并没有效果  

可以通过字体样式来达到设置高度的目的

	fnt = Font()  
	fnt.height = i*20  
	style = XFStyle()  
	style.font = fnt  
	ws.row(i).set_style(style)  

例子如下：

	w = Workbook()  
	ws = w.add_sheet('sheet')  
	  
	for i in range(6, 80):  
	    fnt = Font()  
	    #这里设置字体的高度,可以用来设置行的高度。可没有宽度  
	    fnt.height = i*5  
	    style = XFStyle()  
	    style.font = fnt  
	    ws.write(1, i, 'Test', style) #也可以这样来设置行高  
	    ws.write(1, i, 'Test')  
	    #这里设置列宽，主要通过设置Worksheet中的col来操作，其底层也是通过设置Column.width来达到改变其宽度的目的  
	    ws.col(i).width = 0x0d00 + i*50  
	    #这里设置行高，通过设置字体高度  
	    ws.row(1).set_style(style)  
	w.save('width.xls')  
 
## 写入公式 ##

pyExcelerator支持对excel公式的写入，主要在模块ExcelFormula，定义了公式的语法，解析等
主要体现在方法：`Worksheet.write`
在Worksheet的构造函数中，设置相关写入公式的属性设置，如

	self.__save_recalc = 0  
	self.__frmla_opts = ExcelFormula.RecalcAlways | ExcelFormula.CalcOnOpen  
	self.__show_formulas = 0  

这里设置公式参数，包括四个属性见ExcelFormula.py中：

	NoCalcs                           #不计算  
	RecalcAlways                   #值改变时立即计算  
	CalcOnOpen                    #打开时计算  
	PartOfShareFormula        #这个啥意思：部分共享公式？求指点  

一个例子：

	from pyExcelerator import *  
	import random  
	  
	w = Workbook()  
	ws = w.add_sheet('sheet')  
	ws.frmla_opts=RecalcAlways  
	#ws.show_formulas = True  
	  
	#写入一些数据  
	for i in range(30):  
	    for j in range(20):  
	        ws.write(i, j, random.randint(1, 100))  
	          
	#这里写公式          
	ws.write(30, 0, Formula("SUM(A1:A30)"))  
	ws.write(30, 2, Formula("SUM(B1:B30)*SUM(C1:C30)"))  

至于更复杂的公式就没尝试了，比如IF,SUMIF之类。其实我也不会写，不过在examples目录也有解析较复杂的例子

	f = ExcelFormula.Formula(  
	""" -((1.80 + 2.898 * 1)/(1.80 + 2.898))* 
	AVERAGE((1.80 + 2.898 * 1)/(1.80 + 2.898);  
	        (1.80 + 2.898 * 1)/(1.80 + 2.898);  
	        (1.80 + 2.898 * 1)/(1.80 + 2.898)) +  
	SIN(PI()/4)""")  