## MySQL引擎  
### ISAM引擎：  
  ISAM引擎（ISAM, Indexed Sequential Access Method，索引顺序存取方法）是用于创建，维护和操作从随机数据文件记录中提取，以实现所需要的文件记录快速检索关键场的索引的方法，IBM为大型计算机开发了ISAM 。IBM在IBM上更换了一种称为VSAM（虚拟存储访问方法）的方法。后来，IBM开发了DB2，从2004年开始，IBM推出了DB2作为主要的数据库管理系统。VSAM是DB2中使用的物理访问方法。  
  ISAM是一个定义明确且历经时间考验的数据表格管理方法，它在设计之时就考虑到 数据库被查询的次数要远大于更新的次数。因此，ISAM执行读取操作的速度很快，而且不占用大量的内存和存储资源。ISAM的两个主要不足之处在于，它不 支持事务处理，也不能够容错：如果你的硬盘崩溃了，那么数据文件就无法恢复了。如果你正在把ISAM用在关键任务应用程序里，那就必须经常备份你所有的实 时数据，通过其复制特性，MYSQL能够支持这样的备份应用程序。
> #### ISAM树：  
  > ISAM树是一个静态索引结构，它在文件不频繁修改的情况下非常有效，但它不适合频繁增长和缩小的数据文件。叶子页包含数据项。为了方便起见，将把拥有搜索码值k的数据项表示为 k\*。非叶子页包含索引项，其形式为<搜索码值，页标识符>，它用于直接搜索需要的数据项（存储在叶子页中）。  
  > 索引顺序存取方法（ISAM）的数据结构如图所示。  
  > ![](http://img.my.csdn.net/uploads/201301/22/1358856930_6598.jpg)  
### MyISAM引擎：  
  MyISAM是MySQL的默认数据库引擎（5.5版之前），由早期的ISAM所改良，是MySQL的ISAM扩展格式和缺省的数据库引擎。虽然性能极佳，但却有一个缺点：不支持事务处理（transaction）。不过，在这几年的发展下，MySQL也导入了InnoDB（另一种数据库引擎），以强化参照完整性与并发违规处理机制，后来就逐渐取代MyISAM。   
  MyISAM除了提供ISAM里所没有的索引和字段管理的大量功能，MyISAM还使用一种表格锁定的机制，来优化多个并发的读写操作，其代价是你需要经常运行OPTIMIZE TABLE命令，来恢复被更新机制所浪费的空间。  
  MyISAM还有一些有用的扩展，例如用来修复数据库文件的MyISAMCHK工具和用来恢复浪费空间的 MyISAMPACK工具。MYISAM强调了快速读取操作，这可能就是为什么MySQL受到了WEB开发如此青睐的主要原因：在WEB开发中你所进行的大量数据操作都是读取操作。所以，大多数虚拟主机提供商和INTERNET平台提供商只允许使用MYISAM格式。MyISAM格式的一个重要缺陷就是不能在表损坏后恢复数据。
  每个MyISAM数据表，皆由存储在硬盘上的3个文件所组成，每个文件都以数据表名称为文件主名，并搭配不同扩展名区分文件类型：   
> .frm－－存储数据表定义，此文件非MyISAM引擎的一部分。  
> .MYD－－存放真正的数据。  
> .MYI－－存储索引信息。 

> #### MyISAM索引实现:  
> 使用B+Tree作为索引结构，叶节点data域存放数据记录的地址，如图所示。  
> ![](https://upload-images.jianshu.io/upload_images/4685968-3c528e050ba99593.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/664/format/webp)
> 从上图可以看出MyISAM的索引文件仅仅保存数据记录的地址，设Col1为主键，则表格部分是一个MyISAM表的主索引（Primary key）示例。  
> 在MyISAM中，主/辅索引在结构上没有任何区别，只是主索引要求key唯一，而辅索引key可重复,如果我们在Col2上建立一个辅索引。  
> ![](https://upload-images.jianshu.io/upload_images/4685968-ca234341ecc259d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/664/format/webp)  
> 同样也是一颗B+Tree，data域保存数据记录的地址。因此，MyISAM中索引检索的算法为首先按照B+Tree搜索算法搜索索引，如果指定的Key存在，则取出其data域的值，然后以data域的值为地址，读取相应数据记录。  
> MyISAM这种索引方式叫做“非聚集索引”，与InnoDB的“聚集索引”相对应。  
### HEAP引擎：  
  HEAP允许只驻留在内存里的临时表格。驻留在内存里让HEAP要比ISAM和MYISAM都快，但是它所管理的数据是不稳定的，而且如果在关机之前没有进行保存，那么所有的数据都会丢失。在数据行被删除的时候，HEAP也不会浪费大量的空间。HEAP表格在你需要使用SELECT表达式来选择和操控数据的时候非常有用。要记住，在用完表格之后就删除表格。  
### InnoDB引擎：  
  InnoDB数据库引擎都是造就MySQL灵活性的技术的直接产品，这项技术就是MYSQL+API。在使用MYSQL的时候，你所面对的每一个挑战几乎都源于ISAM和MyISAM数据库引擎不支持事务处理（transaction process）也不支持外来键。尽管要比ISAM和 MyISAM引擎慢很多，但是InnoDB包括了对事务处理和外来键的支持，这两点都是前两个引擎所没有的。如前所述，如果你的设计需要这些特性中的一者 或者两者，那你就要被迫使用后两个引擎中的一个了。 
  如果感觉自己的确技术高超，你还能够使用MySQL+API来创建自己的数据库引擎。这个API为你提供了操作字段、记录、表格、数据库、连接、安全帐号的功能，以及建立诸如MySQL这样DBMS所需要的所有其他无数功能。深入讲解API已经超出了本文的范围，但是你需要了解MySQL+API的存在及其可交换引擎背后的技术，这一点是很重要的。估计这个插件式数据库引擎的模型甚至能够被用来为MySQL创建本地的XML提供器（XML provider）。（任何读到本文的MySQL+API开发人员可以把这一点当作是个要求。）
> #### InnoDB索引实现：
> InnoDB也使用B+Tree作为索引结构，但具体实现方式却与MyISAM截然不同，区别如下：  
> * InnoDB的数据文件本身即是索引文件：  
> > 在MyISAM索引文件和数据文件是分离的，索引文件仅保存数据记录的地址。  
> > 而在InnoDB中，表数据文件本身就是按B+Tree组织的一个索引结构，这棵树的叶节点data域保存了完整的数据记录。这个索引的key是数据表的主键，因此InnoDB表数据文件本身就是主索引。如图所示：   
> ![](https://upload-images.jianshu.io/upload_images/4685968-36a10ea5ce6fdea4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/543/format/webp)
> > 可以看到叶节点包含了完整的数据记录。这种索引叫做"聚集索引"。由于InnoDB的数据文件本身要按主键聚集，所以InnoDB要求表必须有主键（MyISAM可以没有），如果没有显式指定，则MySQL系统会自动选择一个可以唯一标识数据记录的列作为主键，如果不存在这种列，则MySQL自动为InnoDB表生成一个隐含字段作为主键，这个字段长度为6个字节，类型为长整形。
> * InnoDB的辅索引data域存储相应记录主键的值而不是地址。换句话说，InnoDB的所有辅助索引都引用主键作为data域
> ![](https://upload-images.jianshu.io/upload_images/4685968-9e8eb904029b3bce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/543/format/webp)
> > 这里以英文字符的ASCII码作为比较准则，聚集索引这种实现方式使得按主键的搜索十分高效，但是辅助索引搜索需要检索两遍索引：
> > 1. 首先检索辅助索引获得主键
> > 2. 然后用主键到主索引中检索获得记录
> > 知道了InnoDB的索引实现后，就很容易明白为什么不建议使用过长的字段作为主键，因为所有辅索引都引用主索引，过长的主索引会令辅索引变得过大;再如，用非单调的字段作为主键在InnoDB中不是个好主意，因为InnoDB数据文件本身是一颗B+Tree，非单调的主键会造成在插入新记录时数据文件为了维持B+Tree的特性而频繁的分裂调整，十分低效，而使用自增字段作为主键则是一个很好的选择。



> 参考链接：  
> [MYSQL数据库引擎区别详解](https://www.cnblogs.com/zhangjinghe/p/7599988.html)  
> [MySql中B+索引和ISAM索引介绍](https://blog.csdn.net/imzoer/article/details/8531343)  
> [MySQL索引及其实现原理(基于MyISAM及InnoDB引擎)](https://www.jianshu.com/p/8a2ccdf4d32a)  
> [MySQL Memory(Heap)引擎](https://blog.csdn.net/tianlianchao1982/article/details/7642031)
