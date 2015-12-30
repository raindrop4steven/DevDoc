## CoreData的使用，增删改查

### 添加CoreData框架
导入#import<CoreData/CoreData.h>
贴代码之前需要了解6个对象：

	1、NSManagedObjectContext
	管理对象，上下文，持久性存储模型对象
	2、NSManagedObjectModel
	被管理的数据模型，数据结构
	3、NSPersistentStoreCoordinator
	连接数据库的
	4、NSManagedObject
	被管理的数据记录
	5、NSFetchRequest
	数据请求
	6、NSEntityDescription
	表格实体结构
	此外还需要知道.xcdatamodel文件编译后为.momd或者.mom文件

### 封装好的CoreData管理类CoreDataManager

*CoreDataManager.h*

	#import <Foundation/Foundation.h>
	#import "News.h"
	#define TableName @"News"
	
	@interface CoreDateManager : NSObject
	
	@property (readonly, strong, nonatomic) NSManagedObjectContext *managedObjectContext;
	@property (readonly, strong, nonatomic) NSManagedObjectModel *managedObjectModel;
	@property (readonly, strong, nonatomic) NSPersistentStoreCoordinator *persistentStoreCoordinator;
	
	- (void)saveContext;
	- (NSURL *)applicationDocumentsDirectory;
	
	//插入数据
	- (void)insertCoreData:(NSMutableArray*)dataArray;
	//查询
	- (NSMutableArray*)selectData:(int)pageSize andOffset:(int)currentPage;
	//删除
	- (void)deleteData;
	//更新
	- (void)updateData:(NSString*)newsId withIsLook:(NSString*)islook;
	
	@end

*CoreDateManager.m*

	//
	//  CoreDateManager.m
	//  SQLiteTest
	//
	//  Created by rhljiayou on 14-1-8.
	//  Copyright (c) 2014年 rhljiayou. All rights reserved.
	//
	
	#import "CoreDateManager.h"
	
	@implementation CoreDateManager
	
	@synthesize managedObjectContext = _managedObjectContext;
	@synthesize managedObjectModel = _managedObjectModel;
	@synthesize persistentStoreCoordinator = _persistentStoreCoordinator;
	
	- (void)saveContext
	{
	    NSError *error = nil;
	    NSManagedObjectContext *managedObjectContext = self.managedObjectContext;
	    if (managedObjectContext != nil) {
	        if ([managedObjectContext hasChanges] && ![managedObjectContext save:&error]) {
	            // Replace this implementation with code to handle the error appropriately.
	            // abort() causes the application to generate a crash log and terminate. You should not use this function in a shipping application, although it may be useful during development.
	            NSLog(@"Unresolved error %@, %@", error, [error userInfo]);
	            abort();
	        }
	    }
	}
	
	#pragma mark - Core Data stack
	
	// Returns the managed object context for the application.
	// If the context doesn't already exist, it is created and bound to the persistent store coordinator for the application.
	- (NSManagedObjectContext *)managedObjectContext
	{
	    if (_managedObjectContext != nil) {
	        return _managedObjectContext;
	    }
	    
	    NSPersistentStoreCoordinator *coordinator = [self persistentStoreCoordinator];
	    if (coordinator != nil) {
	        _managedObjectContext = [[NSManagedObjectContext alloc] init];
	        [_managedObjectContext setPersistentStoreCoordinator:coordinator];
	    }
	    return _managedObjectContext;
	}
	
	// Returns the managed object model for the application.
	// If the model doesn't already exist, it is created from the application's model.
	- (NSManagedObjectModel *)managedObjectModel
	{
	    if (_managedObjectModel != nil) {
	        return _managedObjectModel;
	    }
	    NSURL *modelURL = [[NSBundle mainBundle] URLForResource:@"NewsModel" withExtension:@"momd"];
	    _managedObjectModel = [[NSManagedObjectModel alloc] initWithContentsOfURL:modelURL];
	    return _managedObjectModel;
	}
	
	// Returns the persistent store coordinator for the application.
	// If the coordinator doesn't already exist, it is created and the application's store added to it.
	- (NSPersistentStoreCoordinator *)persistentStoreCoordinator
	{
	    if (_persistentStoreCoordinator != nil) {
	        return _persistentStoreCoordinator;
	    }
	    
	    NSURL *storeURL = [[self applicationDocumentsDirectory] URLByAppendingPathComponent:@"NewsModel.sqlite"];
	    
	    NSError *error = nil;
	    _persistentStoreCoordinator = [[NSPersistentStoreCoordinator alloc] initWithManagedObjectModel:[self managedObjectModel]];
	    if (![_persistentStoreCoordinator addPersistentStoreWithType:NSSQLiteStoreType configuration:nil URL:storeURL options:nil error:&error]) {
	        NSLog(@"Unresolved error %@, %@", error, [error userInfo]);
	        abort();
	    }
	    
	    return _persistentStoreCoordinator;
	}
	
	#pragma mark - Application's Documents directory
	
	// Returns the URL to the application's Documents directory.获取Documents路径
	- (NSURL *)applicationDocumentsDirectory
	{
	    return [[[NSFileManager defaultManager] URLsForDirectory:NSDocumentDirectory inDomains:NSUserDomainMask] lastObject];
	}
	
	//插入数据
	- (void)insertCoreData:(NSMutableArray*)dataArray
	{
	    NSManagedObjectContext *context = [self managedObjectContext];
	    for (News *info in dataArray) {
	        News *newsInfo = [NSEntityDescription insertNewObjectForEntityForName:TableName inManagedObjectContext:context];
	        newsInfo.newsid = info.newsid;
	        newsInfo.title = info.title;
	        newsInfo.imgurl = info.imgurl;
	        newsInfo.descr = info.descr;
	        newsInfo.islook = info.islook;
	        
	        NSError *error;
	        if(![context save:&error])
	        {
	            NSLog(@"不能保存：%@",[error localizedDescription]);
	        }
	    }
	}
	
	//查询
	- (NSMutableArray*)selectData:(int)pageSize andOffset:(int)currentPage
	{
	    NSManagedObjectContext *context = [self managedObjectContext];
	    
	    // 限定查询结果的数量
	    //setFetchLimit
	    // 查询的偏移量
	    //setFetchOffset
	    
	    NSFetchRequest *fetchRequest = [[NSFetchRequest alloc] init];
	
	    [fetchRequest setFetchLimit:pageSize];
	    [fetchRequest setFetchOffset:currentPage];
	    
	    NSEntityDescription *entity = [NSEntityDescription entityForName:TableName inManagedObjectContext:context];
	    [fetchRequest setEntity:entity];
	    NSError *error;
	    NSArray *fetchedObjects = [context executeFetchRequest:fetchRequest error:&error];
	    NSMutableArray *resultArray = [NSMutableArray array];
	    
	    for (News *info in fetchedObjects) {
	        NSLog(@"id:%@", info.newsid);
	        NSLog(@"title:%@", info.title);
	        [resultArray addObject:info];
	    }
	    return resultArray;
	}
	
	//删除
	-(void)deleteData
	{
	    NSManagedObjectContext *context = [self managedObjectContext];
	    NSEntityDescription *entity = [NSEntityDescription entityForName:TableName inManagedObjectContext:context];
	
	    NSFetchRequest *request = [[NSFetchRequest alloc] init];
	    [request setIncludesPropertyValues:NO];
	    [request setEntity:entity];
	    NSError *error = nil;
	    NSArray *datas = [context executeFetchRequest:request error:&error];
	    if (!error && datas && [datas count])
	    {
	        for (NSManagedObject *obj in datas)
	        {
	            [context deleteObject:obj];
	        }
	        if (![context save:&error])
	        {
	            NSLog(@"error:%@",error);  
	        }  
	    }
	}
	//更新
	- (void)updateData:(NSString*)newsId  withIsLook:(NSString*)islook
	{
	    NSManagedObjectContext *context = [self managedObjectContext];
	
	    NSPredicate *predicate = [NSPredicate
	                              predicateWithFormat:@"newsid like[cd] %@",newsId];
	    
	    //首先你需要建立一个request
	    NSFetchRequest * request = [[NSFetchRequest alloc] init];
	    [request setEntity:[NSEntityDescription entityForName:TableName inManagedObjectContext:context]];
	    [request setPredicate:predicate];//这里相当于sqlite中的查询条件，具体格式参考苹果文档
	    
	    //https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/Predicates/Articles/pCreating.html
	    NSError *error = nil;
	    NSArray *result = [context executeFetchRequest:request error:&error];//这里获取到的是一个数组，你需要取出你要更新的那个obj
	    for (News *info in result) {
	        info.islook = islook;
	    }
	    
	    //保存
	    if ([context save:&error]) {
	        //更新成功
	        NSLog(@"更新成功");
	    }
	}
	@end

### 使用

	NSURL *storeURL = [[self applicationDocumentsDirectory] URLByAppendingPathComponent:@"NewsModel.sqlite"];
生成以后，你可以在Documents下面看到三个文件：

那么你可以打开NewsModel.sqlite文件看一下里面的表格：

- Z_METADATA里面记录了一个本机的UUID。
- Z_PRIMARYKEY里面是所有自己创建的表格的名字。
- ZNEWS是自己创建的表格，打开里面就是我们的数据记录。

使用时候只要：coreManager = [[CoreDateManageralloc]init];创建对象
增删改查：

	//增
	[coreManager insertCoreData:_resultArray];  
	//删  
	    [coreManager deleteData];  
	//改  
	    [coreManager updateData:info.newsid withIsLook:@"1"];  
	//查  
	    [coreManager selectData:10 andOffset:0];</span> 

### 连接
[自定义Cell](http://blog.csdn.net/rhljiayou/article/details/17506921)