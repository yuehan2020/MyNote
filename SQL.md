[toc]

## 关系数据库概述
为什么需要数据库？

因为应用程序需要保存用户的数据，比如Word需要把用户文档保存起来，以便下次继续编辑或者拷贝到另一台电脑。

### 数据模型
数据库按照数据结构来组织、存储和管理数据，实际上，数据库一共有三种模型：

- 层次模型
   - 层次模型就是以“上下级”的层次关系来组织数据的一种方式，层次模型的数据结构看起来就像一颗树。
- 网状模型
  - 网状模型把每个数据节点和其他很多节点都连接起来，它的数据结构看起来就像很多城市之间的路网
- **关系模型**（最常用）
  - 关系模型把数据看作是一个二维表格，任何数据都可以通过行号+列号来唯一确定，它的数据模型看起来就是一个Excel表：
  
### 数据类型
对于一个关系表，除了定义每一列的名称外，还需要定义每一列的数据类型。

选择数据类型的时候，要根据业务规则选择合适的类型。通常来说，BIGINT能满足整数存储的需求，VARCHAR(N)能满足字符串存储的需求，这两种类型是使用最广泛的。

### 主流关系数据库
目前，主流的关系数据库主要分为以下几类：
- 商用数据库，例如：Oracle，SQL Server，DB2等；
- 开源数据库，例如：MySQL，PostgreSQL等；
- 桌面数据库，以微软Access为代表，适合桌面应用程序使用；
- 嵌入式数据库，以Sqlite为代表，适合手机应用和桌面程序。


### SQL
什么是SQL？SQL是**结构化查询语言**的缩写，用来访问和操作数据库系统。

各个数据库支持的各自扩展的功能，通常我们把它们称之为“方言”。

总的来说，SQL语言定义了这么几种操作数据库的能力：

DDL：Data Definition Language

DDL允许用户定义数据，也就是创建表、删除表、修改表结构这些操作。通常，DDL由数据库管理员执行。

DML：Data Manipulation Language

DML为用户提供添加、删除、更新数据的能力，这些是应用程序对数据库的日常操作。

DQL：Data Query Language

DQL允许用户查询数据，这也是通常最频繁的数据库日常操作。

**SQL关键字总是大写，以示突出，表名和列名均使用小写。**

## 安装SQL

MySQL是目前应用最广泛的开源关系数据库。MySQL接口和数据库引擎的关系就好比某某浏览器和浏览器引擎（IE引擎或Webkit引擎）的关系。对用户而言，切换浏览器引擎不影响浏览器界面，切换MySQL引擎不影响自己写的应用程序使用MySQL的接口。

使用MySQL时，不同的表还可以使用不同的数据库引擎。如果你不知道应该采用哪种引擎，记住总是选择==InnoDB==就好了

- PolarDB

由Alibaba改进的一个MySQL版本，专门提供给在阿里云托管的MySQL用户，号称6倍的性能提升。

所以使用MySQL就带来了一个巨大的好处：可以在自己的电脑上安装免费的Community Edition版本，进行学习、开发、测试，部署的时候，可以选择付费的高级版本，或者云服务商提供的兼容版本，而不需要对应用程序本身做改动。

- 运行MySQL
MySQL安装后会自动在后台运行。为了验证MySQL安装是否正确，我们需要通过mysql这个命令行程序来连接MySQL服务器。

在命令提示符下输入mysql -u root-p，然后输入口令，如果一切正确，就会连接到MySQL服务器，同时提示符变为mysql>。

输入exit退出MySQL命令行。注意，MySQL服务器仍在后台运行

## 关系模型
我们已经知道，关系数据库是建立在关系模型上的。而关系模型本质上就是**若干个存储数据的二维表**，可以把它们看作很多Excel表。

- 表的每一行称为记录（Record），记录是一个逻辑意义上的数据。
- 表的每一列称为字段（Column），同一个表的每一行记录都拥有相同的若干字段。

字段定义了数据类型（整型、浮点型、字符串、日期等），以及是否允许为NULL。注意NULL表示字段数据不存在。一个整型字段如果为NULL不表示它的值为0，同样的，一个字符串型字段为NULL也不表示它的值为空串''。

> 通常情况下，字段应该避免允许为NULL。不允许为NULL可以简化查询条件，加快查询速度，也利于应用程序读取数据后无需判断是否为NULL。

和Excel表有所不同的是，关系数据库的表和表之间需要建立“一对多”，“多对一”和“一对一”的关系，这样才能够按照应用程序的逻辑来组织和存储数据。

在关系数据库中，关系是通过**主键**和**外键**来维护的。

### 主键

在关系数据库中，一张表中的每一行数据被称为一条记录。一条记录就是由多个字段组成的。

对于关系表，有个很重要的约束，就是任意两条记录不能重复。不能重复不是指两条记录不完全相同，而是指能够通过某个字段唯一区分出不同的记录，这个字段被称为**主键**。

对主键的要求，**最关键的一点是记录一旦插入到表中**，**主键最好不要再修改**，因为主键是用来唯一定位记录的，修改了主键，会造成一系列的影响。

所以，选取主键的一个基本原则是：**不使用任何业务相关的字段作为主键**。

因此，**身份证号**、**手机号**、**邮箱地址**这些看上去可以唯一的字段，均不可用作主键。

作为主键最好是完全业务无关的字段，我们一般把这个字段命名为id。

- 自增整数类型：数据库会在插入数据时自动为每一条记录分配一个自增整数，这样我们就完全不用担心主键重复，也不用自己预先生成主键；

- 全局唯一GUID类型：使用一种全局唯一的字符串作为主键，类似8f55d96b-8acc-4636-8cb8-76bf8abc2f57。GUID算法通过网卡MAC地址、时间戳和随机数保证任意计算机在任意时间生成的字符串都是不同的，大部分编程语言都内置了GUID算法，可以自己预算出主键。 

对于大部分应用来说，通常**自增类型的主键**就能满足需求。

> 如果使用INT自增类型，那么当一张表的记录数超过2147483647（约21亿）时，会达到上限而出错。使用BIGINT自增类型则可以最多约922亿亿条记录。

### 联合主键

关系数据库实际上还允许通过多个字段唯一标识记录，**即两个或更多的字段都设置为主键**，这种主键被称为联合主键。

没有必要的情况下，我们尽量不使用联合主键，因为它给关系表带来了复杂度的上升。


---

- 小结

主键是关系表中记录的唯一标识。主键的选取非常重要：主键不要带有业务含义，而应该使用BIGINT自增或者GUID类型。主键也不应该允许NULL。

可以使用多个列作为联合主键，但联合主键并不常用。

### 外键
在students表中，通过class_id的字段，可以把数据与另一张表关联起来，这种列称为外键。
外键并不是通过列名实现的，而是通过定义外键约束实现的。

```
ALTER TABLE students
ADD CONSTRAINT fk_class_id
FOREIGN KEY (class_id)
REFERENCES classes (id);
```
其中，外键约束的名称fk_class_id可以任意，FOREIGN KEY (class_id)指定了class_id作为外键，REFERENCES classes(id)指定了这个外键将关联到classes表的id列（即classes表的主键）

通过定义外键约束，关系数据库可以保证无法插入无效的数据。即如果classes表不存在id=99的记录，students表就无法插入class_id=99的记录。

由于外键约束会降低数据库的性能，大部分互联网应用程序为了追求速度，并不设置外键约束，而是仅靠应用程序自身来保证逻辑的正确性。这种情况下，class_id仅仅是一个普通的列，只是它起到了外键的作用而已。

要删除一个外键约束，也是通过ALTER TABLE实现的：


```
ALTER TABLE students
DROP FOREIGN KEY fk_class_id;
```
> 注意：删除外键约束并没有删除外键这一列。删除列是通过DROP COLUMN ...实现的。

### 多对多

通过一个表的外键关联到另一个表，我们可以定义出一对多关系。有些时候，还需要定义“多对多”关系。例如，一个老师可以对应多个班级，一个班级也可以对应多个老师，因此，班级表和老师表存在多对多关系。

**多对多关系实际上是通过两个一对多关系实现的**，即通过一个中间表，关联两个一对多关系，就形成了多对多关系：

### 一对一

一对一关系是指，一个表的记录对应到另一个表的唯一一个记录。

例如，students表的每个学生可以有自己的联系方式，如果把联系方式存入另一个表contacts，我们就可以得到一个“一对一”关系：


id | student_id | mobile| 
---|---|---|---
1 | 1 | 135xxxx6300 | 
2 | 2 | 135xxxx6300 | 

还有一些应用会把一个大表拆成两个一对一的表，目的是把经常读取和不经常读取的字段分开，以获得更高的性能。

例如，把一个大的用户表分拆为用户基本信息表user_info和用户详细信息表user_profiles，大部分时候，只需要查询user_info表，并不需要查询user_profiles表，这样就提高了查询速度。

---

- 小结

关系数据库通过外键可以实现一对多、多对多和一对一的关系。外键既可以通过数据库来约束，也可以不设置约束，仅依靠应用程序的逻辑来保证。

### 索引
在关系数据库中，如果有上万甚至上亿条记录，在查找记录的时候，想要获得非常快的速度，就需要使用索引。

索引是关系数据库中对某一列或多个列的值进行预排序的数据结构。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录，这样就大大加快了查询速度。

如果要经常根据score列进行查询，就可以对score列创建索引：


```
ALTER TABLE students
ADD INDEX idx_score (score);
```
使用ADD INDEX idx_score (score)就创建了一个名称为idx_score，使用列score的索引。索引名称是任意的，**索引如果有多列，可以在括号里依次写上**，例如：


```
ALTER TABLE students
ADD INDEX idx_name_score (name, score);
```
索引的效率取决于索引列的值是否散列，即该列的值如果越互不相同，那么索引效率越高。反过来，如果记录的列存在大量相同的值，例如gender列，大约一半的记录值是M，另一半是F，因此，对该列创建索引就没有意义。

可以对一张表创建多个索引。索引的优点是提高了查询效率，缺点是在插入、更新和删除记录时，需要同时修改索引，因此，索引越多，插入、更新和删除记录的速度就越慢。

> 对于主键，关系数据库会自动对其创建主键索引。使用主键索引的效率是最高的，因为主键会保证绝对唯一。

### 唯一索引
在设计关系数据表的时候，看上去唯一的列，例如身份证号、邮箱地址等，因为他们具有业务含义，因此不宜作为主键。

但是，这些列根据业务要求，又具有唯一性约束：即不能出现两条记录存储了同一个身份证号。这个时候，就可以给该列添加一个唯一索引。例如，我们假设students表的name不能重复：


```
ALTER TABLE students
ADD UNIQUE INDEX uni_name (name);
```
通过==UNIQUE==关键字我们就添加了一个唯一索引。


也可以只对某一列添加一个唯一约束而不创建唯一索引。


```
ALTER TABLE students
ADD CONSTRAINT uni_name UNIQUE (name)
```
这种情况下，name列没有索引，但仍然具有唯一性保证。

无论是否创建索引，对于用户和应用程序来说，使用关系数据库不会有任何区别。这里的意思是说，当我们在数据库中查询时，如果有相应的索引可用，数据库系统就会自动使用索引来提高查询效率，如果没有索引，查询也能正常执行，只是速度会变慢。因此，索引可以在使用数据库的过程中逐步优化。


---
- 小结

通过对数据库表创建索引，可以提高查询速度。

通过创建唯一索引，可以保证某一列的值具有唯一性。

数据库索引对于用户和应用程序来说都是透明的。



## 查询数据
在关系数据库中，最常用的操作就是查询。
### 准备数据

id | class_id|name|gender|score
---|---|---|---|---
1 | 1 |小明|m|90
2 | 1 |小红|f|95


id | name
---|---
1 | 一班
2 | 二班

请注意，和MySQL的持久化存储不同的是，由于我们使用的是AlaSQL内存数据库，两张表的数据在页面加载时导入，并且只存在于浏览器的内存中，因此，刷新页面后，数据会重置为上述初始值。

如果你想用MySQL练习，可以下载这个SQL脚本，然后在命令行运行：

```
$ mysql -u root -p < init-test-data.sql
```
### 基本查询
要查询数据库表的数据，我们使用如下的SQL语句：

```
SELECT * FROM <表名>
```

使用SELECT * FROM students时，==SELECT==是关键字，表示将要执行一个查询，‘*’ 表示“所有列”，==FROM==表示将要从哪个表查询。

> 注意：查询结果也是一个二维表，它包含列名和每一行的数据。

不带FROM子句的SELECT语句有一个有用的用途，就是用来判断当前到数据库的连接是否有效。许多检测工具会执行一条SELECT 1;来测试数据库连接。

### 条件查询

例如，查询分数在80分以上的学生记录。在一张表有数百万记录的情况下，获取所有记录不仅费时，还费内存和网络带宽。

SELECT语句可以通==过WHERE==条件来设定查询条件，查询结果是满足查询条件的记录。

例如，要指定条件“分数在80分或以上的学生”，写成WHERE条件就是SELECT * FROM students WHERE score >= 80。


```
SELECT * FROM students WHERE score >= 80
```
因此，条件查询的语法就是：

```
SELECT * FROM <表名> WHERE <条件表达式>
```
条件表达式可以用<条件1> AND <条件2>表达满足条件1并且满足条件2。

> 注意gender列存储的是字符串，需要用单引号括起来。

第二种条件是<条件1> OR <条件2>，表示满足条件1或者满足条件2。

第三种条件是NOT <条件>，表示“不符合该条件”的记录。例如，写一个“不是2班的学生”这个条件，可以先写出“是2班的学生”：class_id = 2，再加上NOT：NOT class_id = 2：

上述NOT条件NOT class_id = 2其实等价于class_id <> 2，因此，NOT查询不是很常用。

要组合三个或者更多的条件，就需要用小括号()表示如何进行条件运算

例如，编写一个复杂的条件：分数在80以下或者90以上，并且是男生


```
SELECT * FROM students WHERE (score < 80 OR score > 90) AND gender = 'M';
```
如果不加括号，条件运算按照NOT、AND、OR的优先级进行，即NOT优先级最高，其次是AND，最后是OR。加上括号可以改变优先级。

### 投影查询

如果我们只希望返回某些列的数据，而不是所有列的数据，我们可以用SELECT 列1, 列2, 列3 FROM ...，让结果集仅包含指定列。这种操作称为投影查询。

使用SELECT 列1, 列2, 列3 FROM ...时，还可以给每一列起个别名，这样，结果集的列名就可以与原表的列名不同。它的语法是SELECT 列1 别名1, 列2 别名2, 列3 别名3 FROM ...。


---

- 小结

使用SELECT *表示查询表的所有列，使用SELECT 列1, 列2, 列3则可以仅返回指定列，这种操作称为投影。

SELECT语句可以对结果集的列进行重命名。

### 排序
我们使用SELECT查询时，细心的读者可能注意到，查询结果集通常是按照id排序的，也就是根据主键排序。这也是大部分数据库的做法。如果我们要根据其他条件排序怎么办？可以加上==ORDER BY==子句。例如按照成绩从低到高进行排序：

例如按照成绩从低到高进行排序：

```
SELECT id, name, gender, score FROM students ORDER BY score;

```
如果要反过来，按照成绩从高到底排序，我们可以加上DESC表示“倒序”：

如果score列有相同的数据，要进一步排序，可以继续添加列名。例如，使用ORDER BY score DESC, gender表示先按score列倒序，如果有相同分数的，再按gender列排序：


```
SELECT id, name, gender, score FROM students ORDER BY score DESC, gender;

```
默认的排序规则是ASC：“升序”，即从小到大。ASC可以省略，即ORDER BY score ASC和ORDER BY score效果一样。

如果有WHERE子句，那么ORDER BY子句要放到WHERE子句后面。例如，查询一班的学生成绩，并按照倒序排序：

**先条件 后排序**

```
SELECT id, name, gender, score
FROM students
WHERE class_id = 1
ORDER BY score DESC;

```


---

- 小结
使用ORDER BY可以对结果集进行排序；
可以对多列进行升序、倒序排序。

### 分页查询
使用SELECT查询时，如果结果集数据量很大，比如几万行数据，放在一个页面显示的话数据量太大，不如分页显示，每次显示100条。

要实现分页功能，实际上就是从结果集中显示第1~100条记录作为第1页，显示第101~200条记录作为第2页，以此类推。

因此，分页实际上就是从结果集中“截取”出第M~N条记录。这个查询可以通过LIMIT <M> OFFSET <N>子句实现。我们先把所有学生按照成绩从高到低进行排序：


```
SELECT id, name, gender, score FROM students ORDER BY score DESC;

```
现在，我们把结果集分页，每页3条记录。要获取第1页的记录，可以使用LIMIT 3 OFFSET 0：

```
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 0;
```
分页查询的关键在于，首先要确定每页需要显示的结果数量pageSize（这里是3），然后根据当前页的索引pageIndex（从1开始），确定LIMIT和OFFSET应该设定的值：

- LIMIT总是设定为pageSize；
- OFFSET计算公式为pageSize * (pageIndex - 1)。


---

OFFSET是可选的，如果只写LIMIT 15，那么相当于LIMIT 15 OFFSET 0。

在MySQL中，LIMIT 15 OFFSET 30还可以简写成LIMIT 30, 15

使用LIMIT <M> OFFSET <N>分页时，随着N越来越大，查询效率也会越来越低。

---

- 小结

使用LIMIT <M> OFFSET <N>可以对结果集进行分页，每次查询返回结果集的一部分；

分页查询需要先确定每页的数量和当前页数，然后确定LIMIT和OFFSET的值

### 聚合查询

如果我们要统计一张表的数据量，例如，想查询students表一共有多少条记录，难道必须用SELECT * FROM students查出来然后再数一数有多少行吗？

这个方法当然可以，但是比较弱智。对于统计总数、平均数这类计算，SQL提供了专门的聚合函数，使用聚合函数进行查询，就是聚合查询，它可以快速获得结果。

仍然以查询students表一共有多少条记录为例，我们可以使用SQL内置的COUNT()函数查询：



```
SELECT COUNT(*) FROM students;

```
COUNT(*)表示查询所有列的行数，要注意聚合的计算结果虽然是一个数字，但查询的结果仍然是一个二维表，只是这个二维表只有一行一列，并且列名是COUNT(*)


通常，使用聚合查询时，我们应该给列名设置一个别名，便于处理结果


```
SELECT COUNT(*) num FROM students;
```
COUNT(*)和COUNT(id)实际上是一样的效果。**另外注意，聚合查询同样可以使用WHERE条件**，因此我们可以方便地统计出有多少男生、多少女生、多少80分以上的学生等：

除了COUNT()函数外，SQL还提供了如下聚合函数：


函数 | 说明
---|---
sum | 计算某一列的合计值，该列必须为数值类型
AVG | 计算某一列的平均值，该列必须为数值类型
MAX | 计算某一列的最大值
MIN | 计算某一列的最小值

> 注意，MAX()和MIN()函数并不限于数值类型。如果是字符类型，MAX()和MIN()会返回排序最后和排序最前的字符。


```
SELECT AVG(score) average FROM students; //使用聚合查询计算男生平均成绩:
```

> 要特别注意：如果聚合查询的WHERE条件没有匹配到任何行，COUNT()会返回0，而SUM()、AVG()、MAX()和MIN()会返回NULL：


如果我们要统计一班的学生数量，我们知道，可以用SELECT COUNT(*) num FROM students WHERE class_id = 1;。如果要继续统计二班、三班的学生数量，难道必须不断修改WHERE条件来执行SELECT语句吗？

对于聚合查询，SQL还提供了“分组聚合”的功能。

GROUP BY子句指定了按class_id分组，因此，执行该SELECT语句时，会把class_id相同的列先分组，再分别计算，因此，得到了3行结果
```
SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;

```
聚合查询的列中，只能放入分组的列。

---

使用SQL提供的聚合查询，我们可以方便地计算总数、合计值、平均值、最大值和最小值；

聚合查询也可以添加WHERE条件。

### 多表查询

SELECT查询不但可以从一张表查询数据，还可以从多张表同时查询数据。查询多张表的语法是：SELECT * FROM <表1> <表2>。

例如，同时从students表和classes表的“乘积”，即查询数据，可以这么写：


```
SELECT * FROM students, classes;

```

这种一次查询两个表的数据，查询的结果也是一个二维表，它是students表和classes表的“乘积”，即students表的每一行与classes表的每一行都两两拼在一起返回。结果集的列数是students表和classes表的列数之和，行数是students表和classes表的行数之积。

这种多表查询又称**笛卡尔查询**，使用笛卡尔查询时要非常小心，由于结果集是目标表的行数乘积，对两个各自有100行记录的表进行笛卡尔查询将返回1万条记录，对两个各自有1万行记录的表进行笛卡尔查询将返回1亿条记录。


要解决这个问题，我们仍然可以利用投影查询的“设置列的别名”来给两个表各自的id和name列起别名：


```
SELECT
    students.id sid,
    students.name,
    students.gender,
    students.score,
    classes.id cid,
    classes.name cname
FROM students, classes;

```
注意，多表查询时，要使用表名.列名这样的方式来引用列和设置别名，这样就避免了结果集的列名重复问题。但是，用表名.列名这种方式列举两个表的所有列实在是很麻烦，所以SQL还允许给表设置一个别名，让我们在投影查询中引用起来稍微简洁一点：


```
SELECT
    s.id sid,
    s.name,
    s.gender,
    s.score,
    c.id cid,
    c.name cname
FROM students s, classes c;

```

---
- 小结

使用多表查询可以获取M x N行记录；

多表查询的结果集可能非常巨大，要小心使用。

### 连接查询

连接查询是另一种类型的多表查询。连接查询对多个表进行JOIN运算，简单地说，就是先确定一个主表作为结果集，然后，把其他表的行有选择性地“连接”在主表结果集上。

例如，我们想要选出students表的所有学生信息，可以用一条简单的SELECT语句完成：


```
SELECT s.id, s.name, s.class_id, s.gender, s.score FROM students s;

```
但是，假设我们希望结果集同时包含所在班级的名称，上面的结果集只有class_id列，缺少对应班级的name列。

现在问题来了，存放班级名称的name列存储在classes表中，只有根据students表的class_id，找到classes表对应的行，再取出name列，就可以获得班级名称。


这时，连接查询就派上了用场。我们先使用最常用的一种内连接——INNER JOIN来实现：


```
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
FROM students s
INNER JOIN classes c
ON s.class_id = c.id;

```
注意INNER JOIN查询的写法是：

1. 先确定主表，仍然使用FROM <表1>的语法；

2. 再确定需要连接的表，使用INNER JOIN <表2>的语法；

3. 然后确定连接条件，使用ON <条件...>，这里的条件是s.class_id = c.id，表示students表的class_id列与classes表的id列相同的行需要连接；

4. 可选：加上WHERE子句、ORDER BY等子句。

使用别名不是必须的，但可以更好地简化查询语句。

---

JOIN查询需要先确定主表，然后把另一个表的数据“附加”到结果集上；

INNER JOIN是最常用的一种JOIN查询，它的语法是SELECT ... FROM <表1> INNER JOIN <表2> ON <条件...>；

JOIN查询仍然可以使用WHERE条件和ORDER BY排序。


## 修改数据

关系数据库的基本操作就是增删改查，即CRUD：Create、Retrieve、Update、Delete。其中，对于查询，我们已经详细讲述了SELECT语句的详细用法。

而对于增、删、改，对应的SQL语句分别是：

- INSERT：插入新记录；
- UPDATE：更新已有记录；
- DELETE：删除已有记录。

## INERT（插入）
当我们需要向数据库表中插入一条新记录时，就必须使用INSERT语句

==INSERT==语句的基本语法是：


```
INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);
```
例如，我们向students表插入一条新记录，先列举出需要插入的字段名称，然后在VALUES子句中依次写出对应字段的值：


```
INSERT INTO students (class_id, name, gender, score) VALUES (2, '大牛', 'M', 80);
-- 查询并观察结果:
SELECT * FROM students;

```
注意到我们并没有列出id字段，也没有列出id字段对应的值，这是因为id字段是一个自增主键，它的值可以由数据库自己推算出来。此外，如果一个字段有默认值，那么在INSERT语句中也可以不出现。


要注意，字段顺序不必和数据库表的字段顺序一致，但值的顺序必须和字段顺序一致。也就是说，可以写INSERT INTO students (score, gender, name, class_id) ...，但是对应的VALUES就得变成(80, 'M', '大牛', 2)。

还可以一次性添加多条记录，只需要在VALUES子句中指定多个记录值，每个记录是由(...)包含的一组值：


```
//多次添加
INSERT INTO students (class_id, name, gender, score) VALUES
  (1, '大宝', 'M', 87),
  (2, '二宝', 'M', 81);

SELECT * FROM students;

```

### UPDATE （更新数据）
UPDATE语句的基本语法是：

```
UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;
```

例如，我们想更新students表id=1的记录的name和score这两个字段，先写出UPDATE students SET name='大牛', score=66，然后在WHERE子句中写出需要更新的行的筛选条件id=1：
```
UPDATE students SET name='大牛', score=66 WHERE id=1;
-- 查询并观察结果:
SELECT * FROM students WHERE id=1;

```
注意到UPDATE语句的WHERE条件和SELECT语句的WHERE条件其实是一样的，因此完全可以一次更新多条记录：


```
UPDATE students SET name='小牛', score=77 WHERE id>=5 AND id<=7;
-- 查询并观察结果:
SELECT * FROM students;

```
在UPDATE语句中，更新字段时可以使用表达式。例如，把所有80分以下的同学的成绩加10分：


```
UPDATE students SET score=score+10 WHERE score<80;
-- 查询并观察结果:
SELECT * FROM students;

```
最后，要特别小心的是，UPDATE语句可以没有WHERE条件。

这时，整个表的所有记录都会被更新。所以，在执行UPDATE语句时要非常小心，最好先用SELECT语句来测试WHERE条件是否筛选出了期望的记录集，然后再用UPDATE更新。

### DELETE
DELETE语句的基本语法是：


```
DELETE FROM <表名> WHERE ...;
```
例如，我们想删除students表中id=1的记录，就需要这么写：


```
DELETE FROM students WHERE id=1;
-- 查询并观察结果:
SELECT * FROM students;

```
注意到DELETE语句的WHERE条件也是用来筛选需要删除的行，因此和UPDATE类似，DELETE语句也可以一次删除多条记录：


```
DELETE FROM students WHERE id>=5 AND id<=7;
-- 查询并观察结果:
SELECT * FROM students;

```
最后，要特别小心的是，和UPDATE类似，不带WHERE条件的DELETE语句会删除整个表的数据

## 管理mysql

要管理MySQL，可以使用可视化图形界面MySQL Workbench

MySQL Workbench可以用可视化的方式查询、创建和修改数据库表，但是，归根到底，MySQL Workbench是一个图形客户端，它对MySQL的操作仍然是发送SQL语句并执行。因此，本质上，MySQL Workbench和MySQL Client命令行都是客户端，和MySQL交互，唯一的接口就是SQL。

因此，MySQL提供了大量的SQL语句用于管理。虽然可以使用MySQL Workbench图形界面来直接管理MySQL，但是，很多时候，通过SSH远程连接时，只能使用SQL命令，所以，了解并掌握常用的SQL管理操作是必须的。

### 数据库

在一个运行MySQL的服务器上，实际上可以创建多个数据库（Database）。要列出所有数据库，使用命令


```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| shici              |
| sys                |
| test               |
| school             |
+--------------------+
```
其中，information_schema、mysql、performance_schema和sys是系统库，不要去改动它们。其他的是用户创建的数据库。

要创建一个新数据库，使用命令：

```
mysql> CREATE DATABASE test;
Query OK, 1 row affected (0.01 sec)
```
要删除一个数据库，使用命令：

```
mysql> DROP DATABASE test;
Query OK, 0 rows affected (0.01 sec)
```
注意：删除一个数据库将导致该数据库的所有表全部被删除。

对一个数据库进行操作时，要首先将其切换为当前数据库：


```
mysql> USE test;
Database changed
```

### 表

列出当前数据库的所有表，使用命令：

```
mysql> SHOW TABLES;
+---------------------+
| Tables_in_test      |
+---------------------+
| classes             |
| statistics          |
| students            |
| students_of_class1  |
+---------------------+
```
要查看一个表的结构，使用命令：


```
mysql> DESC students;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | bigint(20)   | NO   | PRI | NULL    | auto_increment |
| class_id | bigint(20)   | NO   |     | NULL    |                |
| name     | varchar(100) | NO   |     | NULL    |                |
| gender   | varchar(1)   | NO   |     | NULL    |                |
| score    | int(11)      | NO   |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)
```

还可以使用以下命令查看创建表的SQL语句：

```
mysql> SHOW CREATE TABLE students;
+----------+-------------------------------------------------------+
| students | CREATE TABLE `students` (                             |
|          |   `id` bigint(20) NOT NULL AUTO_INCREMENT,            |
|          |   `class_id` bigint(20) NOT NULL,                     |
|          |   `name` varchar(100) NOT NULL,                       |
|          |   `gender` varchar(1) NOT NULL,                       |
|          |   `score` int(11) NOT NULL,                           |
|          |   PRIMARY KEY (`id`)                                  |
|          | ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 |
+----------+-------------------------------------------------------+
1 row in set (0.00 sec)
```
创建表使用CREATE TABLE语句，而删除表使用DROP TABLE语句

修改表就比较复杂。如果要给students表新增一列birth，使用：

```
ALTER TABLE students ADD COLUMN birth VARCHAR(10) NOT NULL;
```
要修改birth列，例如把列名改为birthday，类型改为VARCHAR(20)


```
ALTER TABLE students CHANGE COLUMN birth birthday VARCHAR(20) NOT NULL;
```
要删除列，使用：


```
ALTER TABLE students DROP COLUMN birthday;
```
退出MySQL


```
mysql> EXIT
Bye
```
## 实用sql

在编写SQL时，灵活运用一些技巧，可以大大简化程序逻辑。

- 插入或替换


如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就先删除原记录，再插入新记录。此时，可以使用REPLACE语句，这样就不必先查询，再决定是否先删除再插入：


```
REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
```
若id=1的记录不存在，REPLACE语句将插入新记录，否则，当前id=1的记录将被删除，然后再插入新记录。

- 插入或更新

如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就更新该记录，此时，可以使用INSERT INTO ... ON DUPLICATE KEY UPDATE ...语句：


```
INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99) ON DUPLICATE KEY UPDATE name='小明', gender='F', score=99;
```
- 插入或忽略
- 
如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就啥事也不干直接忽略，此时，可以使用INSERT IGNORE INTO ...语句：


```
INSERT IGNORE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
```
- 快照

如果想要对一个表进行快照，即复制一份当前表的数据到一个新表，可以结合CREATE TABLE和SELECT：

```
-- 对class_id=1的记录进行快照，并存储为新表students_of_class1:
CREATE TABLE students_of_class1 SELECT * FROM students WHERE class_id=1;
```
新创建的表结构和SELECT使用的表结构完全一致。

- 写入查询结果集

如果查询结果集需要写入到表中，可以结合INSERT和SELECT，将SELECT语句的结果集直接插入到指定表中。

例如，创建一个统计成绩的表statistics，记录各班的平均成绩：


```
INSERT INTO statistics (class_id, average) SELECT class_id, AVG(score) FROM students GROUP BY class_id;
```
- 强制使用指定索引

在查询的时候，数据库系统会自动分析查询语句，并选择一个最合适的索引。但是很多时候，数据库系统的查询优化器并不一定总是能使用最优索引。如果我们知道如何选择索引，可以使用==FORCE INDEX==强制查询使用指定的索引。例如：


```
 SELECT * FROM students FORCE INDEX (idx_class_id) WHERE class_id = 1 ORDER BY id DESC;
```
## 事务
在执行SQL语句的时候，某些业务要求，一系列操作必须全部执行，而不能仅执行一部分。例如，一个转账操作


```
-- 从id=1的账户给id=2的账户转账100元
-- 第一步：将id=1的A账户余额减去100
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
-- 第二步：将id=2的B账户余额加上100
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
```
这两条SQL语句必须全部执行，或者，由于某些原因，如果第一条语句成功，第二条语句失败，就必须全部撤销。

这种把多条语句作为一个整体进行操作的功能，被称为数据库**事务**。数据库事务可以确保该事务范围内的所有操作都可以全部成功或者全部失败。如果事务失败，那么效果就和没有执行这些SQL一样，不会对数据库数据有任何改动。

可见，数据库事务具有ACID这4个特性：
- A：Atomic，原子性，将所有SQL作为原子工作单元执行，要么全部执行，要么全部不执行；
- C：Consistent，一致性，事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户则必定加上了100；
- I：Isolation，隔离性，如果有多个事务并发执行，每个事务作出的修改必须与其他事务隔离；
- D：Duration，持久性，即事务完成后，对数据库数据的修改被持久化存储。

对于单条SQL语句，数据库系统自动将其作为一个事务执行，这种事务被称为隐式事务。

使用==BEGIN==开启一个事务，使用==COMMIT==提交一个事务，这种事务被称为显式事务。


```
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```
有些时候，我们希望主动让事务失败，这时，可以用ROLLBACK回滚事务，整个事务会失败：

- 隔离级别

对于两个并发执行的事务，如果涉及到操作同一条记录的时候，可能会发生问题。因为并发操作会带来数据的不一致性，包括脏读、不可重复读、幻读等。数据库系统提供了隔离级别来让我们有针对性地选择事务的隔离级别，避免数据不一致的问题。

SQL标准定义了4种隔离级别，分别对应可能出现的数据不一致的情况：

SQL标准定义了4种隔离级别，分别对应可能出现的数据不一致的情况：

- Read Uncommitted

Read Uncommitted是隔离级别最低的一种事务级别。在这种隔离级别下，一个事务会读到另一个事务更新后但未提交的数据，如果另一个事务回滚，那么当前事务读到的数据就是脏数据，这就是**脏读**（Dirty Read）。


- Read Committed

在Read Committed隔离级别下，一个事务可能会遇到不可重复读（Non Repeatable Read）的问题。

不可重复读是指，在一个事务内，多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么，在第一个事务中，两次读取的数据就可能不一致。

我们仍然先准备好students表的数据：

- Repeatable Read

在Repeatable Read隔离级别下，一个事务可能会遇到幻读（Phantom Read）的问题。

幻读是指，在一个事务中，第一次查询某条记录，发现没有，但是，当试图更新这条不存在的记录时，竟然能成功，并且，再次读取同一条记录，它就神奇地出现了。

我们仍然先准备好students表的数据：

- Repeatable Read

在Repeatable Read隔离级别下，一个事务可能会遇到幻读（Phantom Read）的问题。

幻读是指，在一个事务中，第一次查询某条记录，发现没有，但是，当试图更新这条不存在的记录时，竟然能成功，并且，再次读取同一条记录，它就神奇地出现了。

可见，幻读就是没有读到的记录，以为不存在，但其实是可以更新成功的，并且，更新成功后，再次读取，就出现了。

- Serializable

Serializable是最严格的隔离级别。在Serializable隔离级别下，所有事务按照次序依次执行，因此，脏读、不可重复读、幻读都不会出现。

虽然Serializable隔离级别下的事务具有最高的安全性，但是，由于事务是串行执行，所以效率会大大下降，应用程序的性能会急剧降低。如果没有特别重要的情景，一般都不会使用Serializable隔离级别。

如果没有指定隔离级别，数据库就会使用默认的隔离级别。在MySQL中，如果使用InnoDB，默认的隔离级别是Repeatable Read。