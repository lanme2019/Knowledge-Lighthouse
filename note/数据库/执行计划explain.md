|列名	|描述|

id	在一个大的查询语句中每个SELECT关键字都对应一个唯一的id

select_type	SELECT关键字对应的那个查询的类型

table	表名

partitions	匹配的分区信息

type	针对单表的访问方法

possible_keys	可能用到的索引

key	实际上使用的索引

key_len	实际使用到的索引长度

ref	当使用索引列等值查询时，与索引列进行等值匹配的对象信息

rows	预估的需要读取的记录条数

filtered	某个表经过搜索条件过滤后剩余记录条数的百分比

Extra	一些额外的信息


## type列

system，const，eq_ref，ref，fulltext，ref_or_null，index_merge，unique_subquery，index_subquery，range，index，ALL

system：当表中只有一条记录并且该表使用的存储引擎的统计数据是精确的，比如MyISAM、Memory，那么对该表的访问方法就是system

const：使用等值查询主键或者唯一索引

eq_ref:在连接查询时，如果被驱动表是通过主键或者唯一二级索引列等值匹配的方式进行访问的（如果该主键或者唯一二级索引是联合索引的话，所有的索引列都必须进行等值比较），则对该被驱动表的访问方法就是eq_ref

ref:二级索引等值匹配

index_merge：索引合并  在某些场景下可以使用Intersection、Union、Sort-Union这三种索引合并的方式来执行查询

range：范围查找

index：当我们可以使用索引覆盖，但需要扫描全部的索引记录时，该表的访问方法就是index

ALL:全表扫描