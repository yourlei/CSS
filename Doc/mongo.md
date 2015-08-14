1、show dbs 显示已有数据库
2、use name 使用数据库（name为数据库名）
3、db.dropDatabase() 删除数据库
4、db.collection.insert({key: value}): 向表中插入一条记录（collection为表名）
5、db.collection.find() 查看表中数据
6、db.collection.find().count() 查看表中插入的记录数
7、db.collection.update({key1: value1}, {key1: value2}) 更新数据
8、update(要查找的字段， 更新的数据， 查找的字段不存在时是否插入新的记录，是否更新多条匹配的记录);
9、db.collection.drop() 删除collection表


10、全文索引(每张表只能有一个全文索引)
--mongon2.4.6

 （1）开启全文索引功能： 
	# db.adminCommand({setParameter: 1, textSearchEnabled: true});

 （2）创建全文索引：
	# db.collection.ensureIndex({"song":"text", "lyrics":"text"})
	# db.collection.ensureIndex({"$**": "text"})
	-- $**表示在所有的字符串字段上创建一个全文索引,可实现所有字段内查找
 
 (3) 全文索引命令： 
	# db.collection.runCommand("text", {search: "string1 string2"});
	-- 查找多个值时，使用的是或匹配;
	-- 在要搜索的string前加 “-”，表示不包含该词的记录;

 (4) 删除索引
 	# db.collection.dropIndex('IndexName')

一、关于索引
1、创建索引的命令： db.collection.ensureIndex({key: value}, {name: indexName, unique: false/true})

- name属性：设置索引的名字
- unique：唯一性，即设置后该字段的记录不可以有重复，如：
  --  db.collection.ensureIndex({x: 1, y: 1}, {name: "normal", unique: true});
  --  db.collection.insert({x: 1, y: 2}); //该条记录只能插入一次，如果重复插入该条记录，则会报错


二、命令行下通过for插入数据
- 当插入的数据具有序列性时，可以通过for循环快速的将数据插入
-- for(var i=10; i<70; i+=10){
--   db.collection.insert({score: i})
--}	

三、查找-find
调用find方法不加参数时，将会输出表中的所有数据，添加参数后，会查找与条件匹配的数据并输出

- 在user表中根据score字段查找大于或小于50的记录
-- db.user.find({socre:{$gt: 50}});
-- db.user.find({socre:{$lt: 50}});

- sort方法可以对查询的结果进行升、降序排序
-- db.user.find({score: {$lt: 50}}).sort({socre: 1});
-- db.user.find({score: {$lt: 50}}).sort({socre: -1});

四、删除数据-remvoe
直接调用remove方法，不添加参数时，会删除表中全部数据，添加参数后，会按参数查找表中数据，并删除匹配的记录

- 删除user表中数据
-- db.user.remove();