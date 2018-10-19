## MySQL索引
  MySQL索引的作用主要是将原来线性排列的数据，转换成树型结构。查询的时候Key值就像字典里的拼音一样，通过key来查询value，想想一下，那么多的汉字，用前面几页拼音就可以全部查询出来了。例如：我们查询“苏”这个汉字，在拼音表里直接查询s开始的拼音，首先就排除掉了其他拼音，减少了查询的次数。
###  MySQL索引管理
#### 索引分类
1. 单列索引：  
* 普通索引：```ALTER TABLE `table_name` ADD INDEX index_name ( `column` )```  
    普通索引（由关键字KEY或INDEX定义的索引）的唯一任务是加快对数据的访问速度。因此，应该只为那些最经常出现在查询条件（WHEREcolumn=）或排序条件（ORDERBYcolumn）中的数据列创建索引。只要有可能，就应该选择一个数据最整齐、最紧凑的数据列（如一个整数类型的数据列）来创建索引。
* 唯一索引：```ALTER TABLE `table_name` ADD UNIQUE ( `column` )```   
    唯一索引,与普通索引类似，但是不同的是唯一索引要求所有的类的值是唯一的，这一点和主键索引一样.但是他允许有空值。
* 主键索引：```ALTER TABLE `table_name` ADD PRIMARY KEY ( `column` )```  
    主键索引，不允许有空值，主键索引建立的规则是 int优于varchar,一般在建表的时候创建,最好是与表的其他字段不相关的列或者是业务不相关的列.一般会设为 int 而且是 AUTO_INCREMENT自增类型的。
* 外键索引：```ALTER TABLE `table_name` ADD FULLTEXT ( `column`)```  
    如果为某个外键字段定义了一个外键约束条件，MySQL就会定义一个内部索引来帮助自己以最有效率的方式去管理和使用外键约束条件。
2. 复合索引：```ALTER TABLE `table_name` ADD INDEX index_name (`column`(length),`column`(length),...)```   
    复合索引是创建一个拥有多个字段但是公用一个名称的索引，如果你创建了一个索引，创建语句如下：  
    ```
    ALTER TABLE `table` ADD INDEX ( `col1`, `col2`, `col3`);
    ```
    事实上是创建了三个索引:(col1) (col1, col2) (col1, col2, col3)。 在使用查询的时候遵循MySQL组合索引的“最左前缀”，所以在使用时，我们应将使用最频繁子列放在最左边。
    > 最左前缀：顾名思义，就是最左优先匹配，在where语句中想要让索引生效的话，只能以索引最左列开始查询。例如： 
    > ```
    > select col1, col2, col3, ...
    > from table
    > where col1 = '...' and col2 = '...'
    > ```
    > 当查询某个列中含有范围查询的时候，其右侧所有列都无法时候索引查询。例如：   
    > ```
    > select col1, col2, ...
    > from table
    > where col1 = '...', col2 like '...%', col3 = '...', ...
    > ```
    > 由于col2使用了范围查询like，所以只使用到了 (col1, col2)索引。  
    > 查询的时候跳过了某一字段进行查询，也会导致索引失效， such as：   
    > ```
    > select col1, col2, ...
    > from table
    > where col1 = '...', col3 = '...', ...
    > ```
    > 以上语句中仅使用到了 (col1) 索引。  
3. 全文索引：```ALTER TABLE tablename ADD FULLTEXT(column1, column2)```  
    使用全文索引，可以用SELECT查询命令去检索那些包含着一个或多个给定单词的数据记录。从而避免了使用普通索引时使用范围查找导致的索引失效：  
    ```
    ELECT * 
    FROM table
    WHERE MATCH(col1, col2) AGAINST('xxx', 'sss', 'ddd');
    ```
    以上语句可以将 col1 与 col2 字段里有 'xxx', 'sss', 'ddd' 的数据全部查询出来。
### 参考资料：
> [细说mysql索引](https://www.cnblogs.com/chenshishuo/p/5030029.html)  
> [导致索引失效的一些情况](https://www.cnblogs.com/areyouready/p/7802885.html)
