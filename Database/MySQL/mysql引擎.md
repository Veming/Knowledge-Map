## MySQL引擎  
#### ISAM引擎：  
  ISAM引擎（ISAM, Indexed Sequential Access Method，索引顺序存取方法）是用于创建，维护和操作从随机数据文件记录中提取，以实现所需要的文件记录快速检索关键场的索引的方法，IBM为大型计算机开发了ISAM 。IBM在IBM上更换了一种称为VSAM（虚拟存储访问方法）的方法。后来，IBM开发了DB2，从2004年开始，IBM推出了DB2作为主要的数据库管理系统。VSAM是DB2中使用的物理访问方法。  
> ###### ISAM树：  
  > ISAM树是一个静态索引结构，它在文件不频繁修改的情况下非常有效，但它不适合频繁增长和缩小的数据文件。叶子页包含数据项。为了方便起见，将把拥有搜索码值k的数据项表示为 k\*。非叶子页包含索引项，其形式为<搜索码值，页标识符>，它用于直接搜索需要的数据项（存储在叶子页中）。  
  > 索引顺序存取方法（ISAM）的数据结构如图所示。  
  > ![](http://img.my.csdn.net/uploads/201301/22/1358856930_6598.jpg)  
  


> 参考链接：  
> [MYSQL数据库引擎区别详解](https://www.cnblogs.com/zhangjinghe/p/7599988.html)  
> [MySql中B+索引和ISAM索引介绍](https://blog.csdn.net/imzoer/article/details/8531343)
