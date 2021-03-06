## 事务



## 索引

mysql对索引的是用是由mysql的server层的优化器决定的

### Mysql各种索引区别：

普通索引：最基本的索引，没有任何限制

唯一索引：与"普通索引"类似，不同的就是：索引列的值必须唯一，但允许有空值。

主键索引：它 是一种特殊的唯一索引，不允许有空值。

全文索引：仅可用于 MyISAM 表，针对较大的数据，生成全文索引很耗时好空间。

组合索引：为了更多的提高mysql效率可建立组合索引，遵循”最左前缀“原则。


### 普通索引和唯一索引

**查询过程**

![](https://pic.imgdb.cn/item/6222128b5baa1a80aba5bacc.png)

ID为主键索引，k为普通索引

假设，执行查询的语句是 select id from T where k=5。

这个查询语句在索引树上查找的过程，先是通过 B+ 树从树根开始，按层搜索到叶子节点，也就是图中右下角的这个数据页，然后可以认为数据页内部通过二分法来定位记录。对于普通索引来说，查找到满足条件的第一个记录 (5,500) 后，需要查找下一个记录，直到碰到第一个不满足 k=5 条件的记录。对于唯一索引来说，由于索引定义了唯一性，查找到第一个满足条件的记录后，就会停止继续检索。

那么，这个不同带来的性能差距会有多少呢？答案是，微乎其微。



普通索引和唯一索引的查询性能几乎一样, 但是写性能是普通索引快, 因为可以用到change buffer



### MySQL WAL

Write-Ahead Logging

MySQL 里经常说到的 WAL技术，也就是先写日志，再写磁盘。

**当内存数据页跟磁盘数据页内容不一致的时候，我们成这个内存页为“脏页”。内存数据写入磁盘后，内存和磁盘上的数据页内容就一致了，称为“干净页”。**



### change-buffer和redo log

change buffer记录的只是针对不在内存中的数据，redo log不管数据在不在内存中，都记录。



### 优化器的逻辑

选择索引是优化器的工作。而优化器选择索引的目的，是找到一个最优的执行方案，并用最小的代价去执行语句。在数据库里面，扫描行数是影响执行代价的因素之一。扫描的行数越少，意味着访问磁盘数据的次数越少，消耗的 CPU 资源越少。当然，扫描行数并不是唯一的判断标准，优化器还会结合是否使用临时表、是否排序等因素进行综合判断。



MySQL 在真正开始执行语句之前，并不能精确地知道满足这个条件的记录有多少条，而只能根据统计信息来估算记录数。这个统计信息就是索引的“区分度”。显然，一个索引上不同的值越多，这个索引的区分度就越好。而一个索引上不同的值的个数，我们称之为“基数”（cardinality）。也就是说，这个基数越大，索引的区分度越好。我们可以使用 show index 方法，看到一个索引的基数



MySQL 选错索引，这件事儿还得归咎到没能准确地判断出扫描行数



使用错了索引。

1.  可以使用force index(key)进行校正。
2.   通过修改sql语句诱导优化器选择正确的索引，因为优化器选择索引会考虑三个因素，扫描行数、临时表和排序。 
3.   重新建立一个更合适的索引。



### 给字符串加索引

邮箱存储

```mysql
mysql> alter table SUser add index index1(email);
或
mysql> alter table SUser add index index2(email(6));
```

第一个语句创建的 index1 索引里面，包含了每个记录的整个字符串；而第二个语句创建的 index2 索引里面，对于每个记录都是只取前 6 个字节(前缀索引)

使用前缀索引后，可能会导致查询语句读数据的次数变多。

使用前缀索引，定义好长度，就可以做到既节省空间，又不用额外增加太多的查询成本。

实际上，我们在建立索引时关注的是区分度，区分度越高越好。因为区分度越高，意味着重复的键值越少

使用前缀索引就用不上覆盖索引对查询性能的优化了



身份证号存储

用前缀索引区分度低，长度大

第一种方式是使用**倒序存储**

第二种方式是使用 **hash 字段**。



总结：

1.  直接创建完整索引，这样可能比较占用空间；
2.  创建前缀索引，节省空间，但会增加查询扫描次数，并且不能使用覆盖索引；
3.  倒序存储，再创建前缀索引，用于绕过字符串本身前缀的区分度不够的问题；
4.  创建 hash 字段索引，查询性能稳定，有额外的存储和计算消耗，跟第三种方式一样，都不支持范围扫描。



### 脏页

当内存数据页跟磁盘数据页内容不一致的时候，我们称这个内存页为“脏页”。内存数据写入到磁盘后，内存和磁盘上的数据页的内容就一致了，称为“干净页”。



### 删除数据问题

使用delete删除数据时，只是标记为已删除，而不是真正的表空间数据回收，所以数据依然是存在的，只是不能被再次查询出来。

也就是说，通过 delete 命令是不能回收表空间的。这些可以复用，而没有被使用的空间，看起来就像是“空洞”

不止是删除数据会造成空洞，插入数据也会

也就是说，经过大量增删改的表，都是可能是存在空洞的。所以，如果能够把这些空洞去掉，就能达到收缩表空间的目的。而重建表，就可以达到这样的目的。



### count(*) 问题

MyISAM 引擎把一个表的总行数存在了磁盘上，因此执行 count(*) 的时候会直接返回这个数，效率很高

而 InnoDB 引擎就麻烦了，它执行 count(*) 的时候，需要把数据一行一行地从引擎里面读出来，然后累积计数

![](https://pic.imgdb.cn/item/622717925baa1a80ab40a2c4.png)

你会看到，在最后一个时刻，三个会话 A、B、C 会同时查询表 t 的总行数，但拿到的结果却不同。这和 InnoDB 的事务设计有关系，可重复读是它默认的隔离级别，在代码上就是通过多版本并发控制，也就是 MVCC 来实现的。每一行记录都要判断自己是否对这个会话可见，因此对于 count(*) 请求来说，InnoDB 只好把数据一行一行地读出依次判断，可见的行才能够用于计算“基于这个查询”的表的总行数。

按照效率排序的话，count(字段)<count(主键 id)<count(1)≈count(*)，所以我建议你，尽量使用 count(*)。



## 锁

### 当前读

带 lock in share mode 的 SQL 语句，是当前读，可以读到最新值



### 幻读

幻读指的是一个事务在前后两次查询同一个范围的时候，后一次查询看到了前一次查询没有看到的行

```sql
CREATE TABLE `t` (  `id` int(11) NOT NULL,  
                  `c` int(11) DEFAULT NULL, 
                  `d` int(11) DEFAULT NULL,  
                  PRIMARY KEY (`id`),  
                  KEY `c` (`c`)) ENGINE=InnoDB;
insert into t values(0,0,0),(5,5,5),(10,10,10),(15,15,15),(20,20,20),(25,25,25);

begin;
select * from t where d=5 for update;
commit;
```

比较好理解的是，这个语句会命中 d=5 的这一行，对应的主键 id=5，因此在 select 语句执行完成后，id=5 这一行会加一个**写锁**，而且由于两阶段锁协议，这个写锁会在执行 commit 语句的时候释放。

注意区分快照读和当前读。 当前读指的是select for update或者select in share mode，指的是在更新之前必须先查寻当前的值

delete，insert和update都是写操作，因此会自动加排他锁。而select默认情况是不加锁的。但是如果在select后面加in share model共享锁和for update排他锁，就是加锁查询



产生幻读的原因是，行锁只能锁住行，但是新插入记录这个动作，要更新的是记录之间的“间隙”。因此，为了解决幻读问题，InnoDB 只好引入新的锁，也就是间隙锁 (Gap Lock)。

顾名思义，间隙锁，锁的就是两个值之间的空隙。插入了 6 个记录，这就产生了 7 个间隙。

![](https://pic.imgdb.cn/item/622864355baa1a80ab1bc63a.png)

当你执行 select * from t where d=5 for update 的时候，就不止是给数据库中已有的 6 个记录加上了行锁，还同时加了 7 个间隙锁。这样就确保了无法再插入新的记录。也就是说这时候，在一行行扫描的过程中，不仅将给**行加上了行锁，还给行两边的空隙，也加上了间隙锁。**



两种行锁的兼容性关系

![](https://pic.imgdb.cn/item/622864c15baa1a80ab1c1db2.png)



间隙锁和行锁合称 **next-key lock**，每个 next-key lock 是前开后闭区间。也就是说，我们的表 t 初始化以后，如果用 select * from t for update 要把整个表所有记录锁起来，就形成了 7 个 next-key lock，分别是 (-∞,0]、(0,5]、(5,10]、(10,15]、(15,20]、(20, 25]、(25, +supremum]。



自己加的间隙锁只能允许自己进行插入操作，而不允许其他人进行插入操作，从而在当前读下读不到其他人新插入的记录，解决了幻读





### 加锁

原则 1：加锁的基本单位是 next-key lock。希望你还记得，next-key lock 是前开后闭区间。

原则 2：查找过程中访问到的对象才会加锁。

优化 1：索引上的等值查询，给唯一索引加锁的时候，next-key lock 退化为行锁。

优化 2：索引上的等值查询，向右遍历时且最后一个值不满足等值条件的时候，next-key lock 退化为间隙锁。一个 bug：唯一索引上的范围查询会访问到不满足条件的第一个值为止。