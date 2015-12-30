## 用 SQLite 和 FMDB 替代 Core Data

### FMDB

#### 概述

fmdb的官方源代码github地址是https://github.com/ccgus/fmdb，可以通过Cocoapod来安装它。

下面的三个类是fmdb中用的最多的:  

- **FMDatabase**－－－代表了轻量的SQLite数据库，执行数据库的那些语句去使用这个类。

- **FMDatabaseQueue**－－－如果你想在多线程中使用查询和更新操作，你会用到这个类。

- **FMResultSet**－－－代表了在FMDatabase中执行一个查询操作的结果。


#### 创建
一般是在特定的路径下创建FMDB。通常放在document文件夹下面。通过[FMDatabase databaseWithPath:]来创建。

	#define dataBasePath [[(NSSearchPathForDirectoriesInDomains(NSDocumentDirectory,NSUserDomainMask,YES)) lastObject]stringByAppendingPathComponent:dataBaseName]
	#define dataBaseName @"sanjunsheng.sqlite"

	FMDatabase *db[FMDatabase databaseWithPath:dataBasePath];

#### 打开

	if (![db open]) {
	    NSlog(@:"Could not open db")
	    return;
	}

#### FMDB操作
1. UPDATE操作:在FMDB中除了SELECT这样的语句其他的都用的是带update的执行方法：它包括
CREATE,UPDATE,INSERT,ALTER,COMMIT,BEGIN,DETACH,END,EXPLAIN,VACUUM和REPLACE等，
2. 基本上如果不是以SELECT开头的都是用update.执行update返回一个简单的BOOL的数值。如果是YES那么表明update 成功了。
3. Query操作:这个操作返回查询后的一个FMResultSet的结果`FMResultSet *s= [db executeQuery:@"select * from user"];    while ([s next]) {    }`这样你就能得到查询结果中的每一个值。用[s next]可以轮询query回来的资料，每一次的next可以得到一个row裡对应的数值，并用

		intForColumn:
		longForColumn:
		longLongIntForColumn:
		boolForColumn:
		doubleForColumn:
		stringForColumn:
		dateForColumn:
		dataForColumn:
		dataNoCopyForColumn:
		UTF8StringForColumnName:
		objectForColumnName:

这些方法把值转成Object-C的型态，然后再关闭.[db close];

### 实战
1. 创建表:首先先要判断这个表是否存在，FMDB提供了相应的方法tableExists，很简便。因此我们可以这样很简便地创建表： 

	+(void)createTable
	{
	    if ([db open]) {
	        if (![db tableExists :user]) {
	            if ([db tableExists executeUpdate:@"CREATE TABLE user (title text,originalPrice INTEGER,salePrice INTEGER,kindOfItem text,imgUrl text,goodsUrl text, merchantName text)"]) {
	                NSLog(@"create table success");
	            }else{
	                NSLog(@"fail to create table");
	            }
	        }else {
	           // NSLog(@"table is already exist");
	        }
	    }else{
	        NSLog(@"fail to open");
	    }
	}

2. 向表中插入数据:

		[db executeUpdate:@"insert into user (title,originalPrice,salePrice,kindOfItem,imgUrl,goodsUrl,merchantName) values(?,?,?,?,?,?,?)",item.title,[NSNumber numberWithInt:[item.originalPrice integerValue]] ,[NSNumber numberWithInt:[item.salePrice integerValue]],item.kindOfItem,item.imgUrl,item.goodsUrl,item.merchantName,nil];

3. 清空表中的数据：

		+ (void)clearTableData
		{
		    if ([[FmdbManager sharedInstance] executeUpdate:@"DELETE FROM user"]) {
		        
		    }else{
		        NSLog(@"fail to clear");
		    }
		}

4. FMDatabaseQueue和线程安全：首先创建队列：

		FMDatabaseQueue *queue = [FMDatabaseQueue databaseQueueWithPath:dataBasePath]
使用：

		[queue inDatabase:^(FMDatabase *db) {
		    [db executeUpdate:@"INSERT INTO myTable VALUES (?)", [NSNumber numberWithInt:1]];
		    [db executeUpdate:@"INSERT INTO myTable VALUES (?)", [NSNumber numberWithInt:2]];
		    [db executeUpdate:@"INSERT INTO myTable VALUES (?)", [NSNumber numberWithInt:3]];
		
		    FMResultSet *rs = [db executeQuery:@"select * from foo"];
		    while ([rs next]) {
		        …
		    }
		}];
处理事务：

		[queue inTransaction:^(FMDatabase *db, BOOL *rollback) {
		    [db executeUpdate:@"INSERT INTO myTable VALUES (?)", [NSNumber numberWithInt:1]];
		    [db executeUpdate:@"INSERT INTO myTable VALUES (?)", [NSNumber numberWithInt:2]];
		    [db executeUpdate:@"INSERT INTO myTable VALUES (?)", [NSNumber numberWithInt:3]];
		
		    if (whoopsSomethingWrongHappened) {
		        *rollback = YES;
		        return;
		    }
		    // etc…
		    [db executeUpdate:@"INSERT INTO myTable VALUES (?)", [NSNumber numberWithInt:4]];
		}];


### 原文链接
- [用 SQLite 和 FMDB 替代 Core Data](http://blog.csdn.net/majiakun1/article/details/38680147)
- [FMDB的使用](http://blog.csdn.net/sanjunsheng/article/details/38070519)
