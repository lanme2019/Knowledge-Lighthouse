## redo log与binlog

为什么要有redo日志？
> innodb以页为单位进行磁盘交互的，一个事物可能只修改一个数据页里的几个字节，这时将完整的页一起刷回磁盘很浪费资源


1.redo 是对物理日志（体现在记录数据行）  binlog是逻辑日志记录sql语句
2.


## undo log
