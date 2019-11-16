![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# DBCC ShrinkFile解决方法 2005
#### DBCC ShrinkFile Workaround 2005
**发布-日期: 2017年11月10日 (评论)**

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
搜索条件：
DBCC SHRINKFILE无法正常工作。 这是解决方案。
DBCC SHRINKFILE不起作用。 这是解决方案。

## English
Search Terms:
DBCC SHRINKFILE not working. Here is the solution.
DBCC SHRINKFILE doesn’t work. Here is the solution.


---
## Logic
```SQL
--This script WILL shrink the transaction log file. Works every time!
此脚本将收缩事务日志文件。 每次都有效！
--even if you were having trouble with the Enterprise Manager shrinkfile function, or if you find the
--dbcc shrinkfile just isn't cutting it, or if you are using the dbcc shrinkfile and you are getting the
--error "...log files are in use".
--即使你在使用企业管理shrinkfile功能时遇到问题，或者你发现dbcc shrinkfile只是没有删除它，或者你正在使用dbcc shrinkfile时收到报错：“. ...log files are in use”。
-------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------
--Change to the database you want to use
--转到要使用的数据库
Use [enter your database here]
Use [此处输入你的数据库]
go
 
--Turn off auto truncate log
--关闭auto truncate log

sp_dboption 'your database here', 'trunc. log on chkpt.', false
sp_dboption '你的数据库', 'trunc. log on chkpt.', false

go
 
	Truncate the transaction log
	截断事务日志
Backup log [your database here] with truncate_only
Backup log [你的数据库] with truncate_only

go
 
--Create a temp table to create bogus transactions
--创建临时表格以创建虚假事务
create table t1(f1 int)
go
 
--Load the temp table. This will cause the transaction log to to fill a tiny bit.
--Load 临时表格。 这将使 transaction log 填充一些。
--Enough so you can checkpoint, and shrink the tran log.
--现在你可以 checkpoint, 然后压缩 tran log.

declare @i int
 
set @i= 1
while @i < 10000
Begin
Insert t1
Select @i
set @i = @i + 1
End
 
Update t1
Set f1 = f1 + 1
go
 
--To get the logical name of the tlog file just use sp_helpfile under the database you want to shrink.
--要获取tlog文件的逻辑名称，只需在要缩小的数据库下使用sp_helpfile。
dbcc shrinkfile(logical_filename_Log)
go
 
--Truncate the log again. This will cause the file to shrink.
--再次Truncate日志。 这将导致文件缩小。
Backup log [your database here] with truncate_only
Backup log [你的数据库] with truncate_only
go
 
--Reactivate auto truncate
--重新激活自动截断
sp_dboption 'your database here', 'trunc. log on chkpt.', true
sp_dboption '你的数据库', 'trunc. log on chkpt.', true

go
 
--Drop the temp table
--删掉temp table
Drop table t1
go
-------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------
/*
--I've used this time, and time again. it's a nice clean/safe resolution to large transaction logs.
我已经用过这段时间了。 对于大型事务日志来说，这是一个很好的清洁/安全解决方案。
--WORKS GOOD! Shrinks space Way way down regardless of how BIG transaction log
--size is already. I use this often in emergency situations all the time. Especially when disk space
--is a factor. Recently I used this on a 20Gb Transaction Log file, and 1 minute later it was done.
运行良好，不论事务日志有多大，压缩空间都已经缩小。我经常在紧急情况下使用它，尤其是磁盘空间不够时。最近我在一个20Gb事务日志文件中使用了这个，1分钟后就完成了。

--I think you'll be pleased.
我想你会喜欢它！
*/

```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

