---
layout: post
title: Linux中find与grep命令使用小结
tagline: [linux] 
group: python
categories: [Linux]
---
{% include codepiano/setup %}

## find基本语法

    find path -option [-print] [-exec -ok command] {} \;
    
## 命令参数：

- path: 查找的目录。如`.`表示当前目录，`/`表示系统根目录。
- -print： 将匹配的文件输出到标准输出。
- -exec： 对匹配的文件执行该参数所给出的shell命令。相应命令的形式为'command' { } \;，注意{}和\；之间的空格。
- -ok： 和-exec的作用相同，只不过以一种更为安全的模式来执行该参数所给出的shell命令，在执行每一个命令之前，都会给出提示，让用户来确定是否执行。

## 常用的option参数
<table>
<tr><td>-name   filename</td><td>查找名为filename的文件</td></tr>
<tr><td>-perm</td><td>按执行权限来查找</td></tr>
<tr><td>-user    username</td><td>按文件属主来查找</td></tr>
<tr><td>-mtime   -n +n</td><td>按文件更改时间来查找文件，-n指n天以内，+n指n天以前</td></tr>
<tr><td>-atime    -n +n</td><td>按文件访问时间来查GIN: 0px"></td></tr>
<tr><td>-ctime    -n +n</td><td>按文件创建时间来查找文件，-n指n天以内，+n指n天以前</td></tr>
<tr><td>-size      n[c]</td><td>查长度为n块[或n字节]的文件</td></tr>
<tr><td>-type    b/d/c/p/l/f</td><td>查是块设备、目录、字符设备、管道、符号链接、普通文件</td></tr>
</table>

##一些例子

- `find . -name "*.log" -print` 在当前目录下查找以.log结尾的文件
- `find . -name "ab[cd]" -exec rm -rf {} \;` 在当前目录下查找文件abc或abd并将找到的文件删除
- `find . -name "ab?"` 在当前目录下查找文件ab开头并且文件名只有3个字符
- `find /etc -size +10k` 在目录`/etc`查找大于10k的文件
- `find /etc -size 10k` 在目录`/etc`查找等于10k的文件
- `find /etc -size +10k -a -size -1M` 在目录`/etc`查找大于10k并小于1M的文件,注意k与M大小写
- `find /etc -size +30k -a -size -50k -exec ls -lh {} \;`
- `find . -name "*.log" -atime 10 -exec rm -rf {} \;`查找当前目录下以`log`结尾的文件并且最后在10天前访问过的文件。找到后将其删除


## grep基本用法

    grep [OPTIONS] PATTERN [FILE...]
    
grep是在指定的文件中找到匹配的字符，并返回所在的行。如`grep "size" anaconda-ks.cfg `在该文件中找到包含字符`size`的行,当然也支持正则匹配。

## 常用的option参数

- -i 不区分大小写
- -v 反向查找
- -n 打印出行号

更多的时候和其他的命令一起使用

    more install.log |grep "lib"
    ps -ef|grep java