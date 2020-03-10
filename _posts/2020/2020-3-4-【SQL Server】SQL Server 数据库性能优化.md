---
layout: post
category: web
title: 【SQL Server】SQL Server 数据库性能优化
tagline: by 恬静的小魔龙
tag: Other
---

## 分析比较执行时间计划读取情况
### 1. 查看执行时间和cpu

```sql
set statistics time on
select * from Bus_DevHistoryData
set statistics time off
```
执行后在消息里可以看到

![在这里插入图片描述](https://images0.cnblogs.com/i/352036/201405/041306553613519.png)
### 2. 查看查询对I/O的操作情况

```sql
set statistics io on
select * from Bus_DevHistoryData
set statistics io off
```
执行之后的结果：
![在这里插入图片描述](https://images0.cnblogs.com/i/352036/201405/041308114866875.png)
扫描计数：索引和表执行次数

逻辑读取：数据缓存中读取的页数

物理读取：从磁盘中读取的页数

预读：查询过程中，从磁盘放入缓存的页数

lob逻辑读取：从数据缓存中读取image、text、ntext或大型数据的页数

lob物理读取：从磁盘中读取image、text、ntext或大型数据的页数

lob预读：查询过程中，从磁盘放入缓存的image、text、ntext或大型数据的页数

如果物理读取次数和预计次数比较多，可以使用索引进行优化。

上述两种信息的查看如果不想写sql，可以通过设置完成：

工具->选项

![在这里插入图片描述](https://images0.cnblogs.com/i/352036/201405/041314077205239.png)
### 3. 查看执行计划
选中查询语句，点击
![在这里插入图片描述](https://images0.cnblogs.com/i/352036/201405/041315180029077.png)

-----
## 一、数据库设计优化
>1、不要使用游标。 

使用游标不仅占用内存，而且还用不可思议的方式锁定表，它们可以使DBA所能做的一切性能优化等于没做。游标里每执行一次fetch就等于执行一次select。

> 2、创建适当的索引

每当为一个表添加一个索引，select会更快，可insert和delete却大大变慢，因为创建了维护索引需要许多额外的工作。

 （1）采用函数处理的字段不能利用索引 

 （2）条件内包括了多个本表的字段运算时不能进行索引 

> 3、使用事务 

对于一些耗时的操作，使用事务可以达到很好的优化效果。 

> 4、小心死锁 

按照一定的次序来访问你的表。如果你先锁住表A，再锁住表B，那么在所有的存储过程中都要按照这个顺序来锁定它们。 如果某个存储过程先锁定表B，再锁定表A，这可能会导致一个死锁。

> 5、不要打开大的数据集 

>6、不要使用服务器端游标 

与服务器端游标比起来，客户端游标可以减少服务器和网络的系统开销，并且还减少锁定时间。 

>7、不要忽略同时修改同一记录的问题

有时候，两个用户会同时修改同一记录，这样，后一个修改者修改了前一个修改者的操作，某些更新就会丢失。处理这种情况，创建一个timestamp字段，在写入前检查它，如果允许，就合并修改，如果存在冲突，提示用户。

>8、尽量不要使用text数据类型 

除非使用text处理一个很大的数据，否则不要使用它。因为它不易于查询，速度慢，用的不好还会浪费大量的空间。一般varchar可以更好的处理数据。 

>9、避免在索引列上使用计算  

where子句中，如果索引列是函数的一部分，优化器将不使用索引而使用全表扫描。例如： 

（低效）select ... from [dept] where [sal]*12>25000; 

（高效）select ... from [dept] where [sal]>25000/12;

>10、不同类型的索引效能是不一样的，应尽可能先使用效能高的

数字类型的索引查找效率高于字符串类型，定长字符串char、nchar的索引效率高于变长字符串varchar、nvarchar的索引。

（低效）select ... from tableName where username='张三' and age>=21

（高效）select ... from tableName where age>=21 and username='张三'

## 二、SQL语句优化 

>1、不要使用select * 

在select中指定所需要的列，将带来的好处： 

（1）减少内存耗费和网络的带宽 

（2）更安全

（3）给查询优化器机会从索引读取所有需要的列

>2、使用参数查询 

主要是防止SQL注入，提高安全性。 

>3、使用exists或not exists代替in或not in 

（高效）select * from [emp] where [empno]>0 and exists (select 'X' from [dept] where [dept].[deptno]=[emp].[deptno] and [loc]='MELB');

（低效）select * from [emp] where [empno]>0 and [deptno] in (select [deptno] from [dept] where [loc]='MELB');

>4、is null或is not null操作

判断字段是否为空一般是不会应用索引的，因为索引不索引空值。不能用null作索引，任何包含null值的列都将不会被包含在索引中。也就是说如果某列存在空值，即使对该列建索引也不会提高性能。任何在where子句中使用is null或is not null的语句优化器都不允许使用索引。 

推荐方案：用其他相同功能的操作运算代替，如：a is not null改为a>0或a>''等。 

>5、<及>操作 

大于或小于一般情况不用调整，因为它有索引就会采用索引查找，但有的情况下可以对它进行优化。如一个表有100万记录，那么执行>2与>=3的效果就有很大区别了。

（低效）select * from [emp] where [deptno]>2;

（高效）select * from [emp] where [deptno]>=3;

>6、like操作 

like操作可以应用通配符查询，里面的通配符组合可能达到几乎是任意的查询，但是如果用不好则会产生性能上的问题，如lide '%5400%' 这种查询不会引用索引，而like 'X5400%' 则会引用范围索引。

>7、where后面的条件顺序影响 

where子句后面的条件顺序对大数据量表的查询会产生直接的影响。如：

select * from zl_yhjbqk where dy_dj='1KV以下' and xh_bz=1; 

select * from zl_yhjbqk where dy_dj=1 and dy_dj='1KV以下'; 

以上两个查询，两个字段都没进行索引，所以执行的时候都是全表扫描，第一条SQL的dy_dj='1KV以下'条件在记录集内比率为99%，而xh_bz=1的比率只为0.5%，在进行第一条SQL的时候99%条记录都进行dy_dj及xh_bz的比较。而在进行第二条SQL的时候0.5%条记录都进行dy_dj及xh_bz的比较，以此可以得出第二条SQL的CPU占用率明显比第一条低。 

>8、用union替换or（适用于索引列） 

通常情况下，用union替换where子句中的or将会起到较好的效果。对索引列使用or将造成全表扫描。注意：这个规则只针对多个索引列有效。如果有column没有被索引，查询效率可能会因为你没有选择or而降低。下面的例子中loc_id和region上都有建索引。

（低效）select loc_id,loc_desc,begion from location where loc_id=10 or begion='MELBOURNE'; 

（高效）select loc_id,loc_desc,begion from location where loc_id=10

            union

           select loc_id,loc_desc_begion from location where begion='MELBOURNE'; 

>9、优化group by 

提高group by语句的效率，可以通过将不需要的记录在group by之前过滤掉。

（低效）select [job],avg([sal]) from [emp] group by [job] having job='PRESIDENT' or job='MANAGER'; 

（高效）select [job],avg([sal]) from [emp] where [job]='PRESIDENT' or job='MANAGER' group by [job];

>10、使用存储过程 

可以考虑使用存储过程封装那些复杂的SQL语句或业务逻辑，这样有几个好处：

（1）存储过程的执行计划可以被缓存在内存中较长的时间，减少了重新编译的时间。

（2）存储过程减少了客户端和服务器的繁复交互。 

（3）如果程序发布后需要做某些改变你可以直接修改存储过程而不用修改程序，避免需要重新安装部署程序。  

>11、用sp_configure 'query governor cost limit'或者SET QUERY_GOVERNOR_COST_LIMIT来限制查询消耗的资源。当评估查询消耗的资源超出限制时，服务器自动取消查询,在查询之前就扼杀掉。SET LOCKTIME设置锁的时间。

>12、使用select top或set rowcount来限制操作的行。

>13、如果使用了in或or等时发现查询没有走索引，使用显式申明指定索引： SELECT * FROM PersonMember (INDEX = IX_Title) WHERE processid IN ('男'，'女')。

>14、如果要插入大的二进制值到Image列，使用存储过程，千万不要用内嵌insert来插入(不知JAVA是否)。因为这样应用程序首先将二进制值转换成字符串(尺寸是它的两倍)，服务器受到字符后又将他转换成二进制值。存储过程就没有这些动作: 方法：Create procedure p_insert as insert into table(Fimage) values (@image), 在前台调用这个存储过程传入二进制参数，这样处理速度明显改善。

>15、分析select emp_name form employee where salary>3000 在此语句中若salary是Float类型的，则优化器对其进行优化为Convert(float,3000)，因为3000是个整数，我们应在编程时使用3000.0而不要等运行时让DBMS进行转化。同样字符和整型数据的转换。

## 三、处理百万级以上数据提高查询速度的方法

>1、尽量避免在where子句中使用!=或<>操作符，否则将使引擎放弃使用索引而进行全表扫描。

>2、应考虑在where及order by涉及的列上建立索引。 

>3、尽量避免在where子句中对字段进行null值判断，否则将导致全表扫描。 

>4、就是避免在where子句中使用or来连接条件，否则将导致全表扫描。

select id from t where num=10 or num=20  改写为

select id from t  where num=10

union all 

select id from t where num=20 

>5、尽量避免使用前置百分号。

select id from t where name like '%abc%' 

>6、in 和not in也要慎用，很多时候可以用exists和not exists，否则会导致全表扫描。

>7、如果在where子句中使用参数，也会导致全表扫描。 

select id from t where num=@num  可以改为强制查询使用索引 

select id from t with(index(索引名)) where num=@num 

>8、尽量避免在where子句中对字段进行表达式操作，否则将导致全表扫描。 

select id from t where num/2=100  

应改为： 

select id from t where num=100*2 

>9、尽量避免在where子句中对字段进行函数操作，否则将导致全表扫描。

select id from t where substring(name,1,3)='abc' 

应改为： 

select id from t where name like 'abc%' 

>10、并不是所有索引对查询都有效，SQL根据表中数据来进行查询优化，当索引列有大量数据重复时，SQL查询可能不会去利用索引。

>11、索引并不是越多越好，索引提交了select效率，但是降低了insert和update的效率。一个表的索引数最好不要超过6个。 

>12、尽量使用数字型字段，若只含数值信息的字段尽量不要设计为字符型，这会降低查询和连接的性能，并会增加存储开销。因为引擎在处理查询和连接时会逐个比较字符串中每个字符，而对于数字型而言只需要比较一次就够了。 

>13、尽可能使用varchar/nvarchar代替char/nchar，因为首先变长字段存储空间小，可以节省存储空间；其次对于查询来说，在一个相对较小的字段内搜索效率显然要高些。 

>14、任何地方都不要使用select *，用具体的字段列表代替*，不要返回用不到的字段。 

>15、尽量避免使用游标，因为游标的效率较差，如果游标操作的数据超过1万行，那么就考虑改写。 

>16、尽量避免大事务操作，提高系统并发能力。 

>17、利用set rowcount实现高性能的分页。

```sql
Declare @ID int 
Declare @MoveRecords int 

--@CurrentPage和@PageSize是传入参数 
Set @MoveRecords=@CurrentPage * @PageSize+1 
 
--下面两行实现快速滚动到我们要取的数据的行，并把ID记录下来 
Set Rowcount @MoveRecords 
Select @ID=ID from Table1 Order by ID 
 
Set Rowcount @PageSize 
Select * From Table1 Where ID>=@ID Order By ID 
Set Rowcount 0
```

## 四、数据库主键选取
常见的数据库主键选取方式有：
         ●自动增长字段
         ●Uniqueidentifier
         ●“COMB（Combine）”类型
>1、自动增长字段

优点：
      （1）简单、效率高。
缺点：
      （1）自增一般使用int型，有数据条数的限制。
      （2）在数据库进行数据合并时会比较麻烦。
>2、GUID

优点：
      （1）安全，保证唯一性。
      （2）不会产生自增字段那样数据合并时的问题。
缺点：
      （1）它的长度是16字节，占用大量存储空间。
      （2）该数据类型毫无规律，要在上面建立索引很耗时，所以效率要比使用自增字段低。
>3、COMB

   考虑到上面两种主键类型的优缺点，这里使用COMB类型可以为两者找到了一个平衡点。它的设计思路是这样的：既然GUID类型无规律可言造成索引效率低下，影响系统的性能，那么能不能通过组合的方式，保留GUID前10个字节，用后6个字节表示GUID生成的时间，这样即保证了唯一性同时增加了有序性，以此来提高索引效率。后6字节的时间精度可以达到1/300秒，两个COMB类型数据完全相同的可能性是在这1/300秒内生成的两个GUID前10个字节完全相同，这几乎是不可能的。
（1）SQL Server中SQL命令实现这一思路的方式：
DECLARE @aGuid UNIQUEIDENTIFIER 
SET @aGuid = CAST(CAST(NEWID() AS BINARY(10))+ CAST(GETDATE() AS BINARY(6)) AS UNIQUEIDENTIFIER)
（2）实现COMB数据的C#方式：

```csharp
///<summary>
/// 返回 GUID 用于数据库操作，特定的时间代码可以提高检索效率
/// </summary>
/// <returns>COMB (GUID 与时间混合型) 类型 GUID 数据</returns>
public static Guid NewComb() 
{ 
     byte[] guidArray = System.Guid.NewGuid().ToByteArray(); 
     DateTime baseDate = new DateTime(1900,1,1); 
     DateTime now = DateTime.Now; 
     // Get the days and milliseconds which will be used to build the byte string 
     TimeSpan days = new TimeSpan(now.Ticks - baseDate.Ticks); 
     TimeSpan msecs = new TimeSpan(now.Ticks - (new DateTime(now.Year, now.Month, now.Day).Ticks)); 
     // Convert to a byte array 
     // Note that SQL Server is accurate to 1/300th of a millisecond so we divide by 3.333333 
     byte[] daysArray = BitConverter.GetBytes(days.Days); 
     byte[] msecsArray = BitConverter.GetBytes((long)(msecs.TotalMilliseconds/3.333333)); 
     // Reverse the bytes to match SQL Servers ordering 
     Array.Reverse(daysArray); 
     Array.Reverse(msecsArray); 
     // Copy the bytes into the guid 
     Array.Copy(daysArray, daysArray.Length - 2, guidArray, guidArray.Length - 6, 2); 
     Array.Copy(msecsArray, msecsArray.Length - 4, guidArray, guidArray.Length - 4, 4); 
     return new System.Guid(guidArray); 
} 
/// <summary>
/// 从 SQL SERVER 返回的 GUID 中生成时间信息
/// </summary>
/// <param name="guid">包含时间信息的 COMB </param>
/// <returns>时间</returns>
public static DateTime GetDateFromComb(System.Guid guid) 
{ 
     DateTime baseDate = new DateTime(1900,1,1); 
     byte[] daysArray = new byte[4]; 
     byte[] msecsArray = new byte[4]; 
     byte[] guidArray = guid.ToByteArray(); 
     // Copy the date parts of the guid to the respective byte arrays. 
     Array.Copy(guidArray, guidArray.Length - 6, daysArray, 2, 2); 
     Array.Copy(guidArray, guidArray.Length - 4, msecsArray, 0, 4); 
     // Reverse the arrays to put them into the appropriate order 
     Array.Reverse(daysArray); 
     Array.Reverse(msecsArray); 
     // Convert the bytes to ints 
     int days = BitConverter.ToInt32(daysArray, 0); 
     int msecs = BitConverter.ToInt32(msecsArray, 0); 
     DateTime date = baseDate.AddDays(days); 
     date = date.AddMilliseconds(msecs * 3.333333); 
     return date; 
}
```

