---
title: 使用 FMDB 管理 SQLite 数据库
date: 2016-03-17 11:06:46
categories: blog
tags: [FMDB,Objective-C]
---

 *版权声明：本文为 muhlenXi 原创文章，欢迎转载，转载请注明来源: [http://muhlenxi.com/2016/03/17/FMDB](http://muhlenxi.com/2016/03/17/FMDB)*

### 导语：

> FMDB 是一个面向对象的管理数据库的轻量级框架，它用 Obective-C 语言对数据库 SQLite 的 C 语言API 进行了封装，并且它对多线程的并发操作进行了处理，是线程安全的!

　点击阅读全文来深入了解 FMDB 的如何使用。

<!-- more -->

本文，我会以一个小 demo (同学录)的方式讲在项目中如何使用FMDB的。假如我们有一个老师，他要保存一个班级的所有同学的个人信息，他可以添加学生、删除学生、根据条件查找学生，比如性别、名字等 和 修改学生信息。

如图所示：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/main.PNG" width="320" height="568" alt="选项列表图"/>
</div>


*下载完整  [Demo](https://github.com/muhlenXi/DatabaseDemo) 一起交流学习，记得给 star 支持博主的努力哦！*


### FMDB的安装

* 方式一：使用 `CocoaPods` 安装。
* 方式二：直接去 GitHub 下载，拖入到项目中使用。

[FMDB 传送梦](https://github.com/ccgus/fmdb)

*注意，如果使用第二种方式，需要导入系统依赖库 `sqlite3.0.tbd` 后，才能使用。*

### FMDB 的使用(线程安全的)

在一个项目中，我们往往是通过单例的模式去管理数据库中的，也就是说整个项目中只有一个数据库管理员(DatabaseManager)。

首先我们要创建要管理的对象 (Model)，本文中是人 (Person)。

#### 创建人的模型 （XYJPerson）

*新建一个继承自 `NSObject` 的 `XYJPerson` 的类，用来保存人的相关信息。*

在`XYJPerson.h `中，声明我们所需要的信息。

```objc
@property (nonatomic,copy)  NSString * name;  //!<  姓名
@property (nonatomic,assign) NSInteger  age;  //!<  年龄
@property (nonatomic,copy) NSString *  sex;  //!<  性别
@property (nonatomic,copy) NSString *  QQnumber;  //!<  qq号
@property (nonatomic,copy) NSString *  phoneNumber;  //!<  手机号
@property (nonatomic,copy) NSString *  weixinNumber;  //!<  微信号
@property (nonatomic,copy) NSString * headImagePath;  //!<  头像
@property (nonatomic,assign) NSTimeInterval updateDate; //!<  添加的时间
```

*在 `XYJPerson.m` 中覆写 `description` 方法，可以方便我们查看 Person 的详细信息。*

```objc
- (NSString *)description
{
    return [NSString stringWithFormat:@"name == %@ \n age == %ld \n sex == %@ \n QQnumber == %@ \n phoneNumber == %@ \n weixinNumber == %@ \n headImagePath == %@ \n updateDate == %f",self.name,self.age,self.sex,self.QQnumber,self.phoneNumber,self.weixinNumber,self.headImagePath,self.updateDate];
    
}
```

其次，我们要新建一个 XYJDatabaseManage r用来管理数据库中 Perso n数据的 `增`、`删`、`改`、`查`。


#### 创建 （XYJDatabaseManager）

*新建一个继承自 `NSObject` 的 `XYJDatabaseManager` 的类不是很难。*

第一步：在 `XYJDatabaseManager.h` 文件中导入FMDB的头文件，声明相关属性和单例入口

```objc
#import <FMDB/FMDB.h>
```
```objc
@property (nonatomic,strong,readonly) FMDatabaseQueue * databaseQueue;  //!< 用户数据库操作的队列，线程安全的
/**
 *  单例入口
 */
+ (instancetype) shareManager;
```

第二步：在 `XYJDatabaseManager.m` 文件中创建数据库和实现单例。

*单例方法的实现：*

```objc
+ (instancetype)shareManager
{static XYJDatabaseManager * manager;
    
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        
        manager = [[XYJDatabaseManager alloc] init];
    });
    return manager;
}
```

*覆写 init 方法，在 init 方法中创建数据库和表*

```objc
- (instancetype)init
{
    if (self = [super init]) {
        
        //数据库存放路径
        NSString * libDirPath = [NSSearchPathForDirectoriesInDomains(NSLibraryDirectory, NSUserDomainMask, YES) firstObject];
        NSString * dbPath = [libDirPath stringByAppendingPathComponent:@"databaseDemo.sqlite"];
        NSLog(@"dbpath == %@",dbPath);
        
        //创建数据库
        _databaseQueue = [FMDatabaseQueue databaseQueueWithPath:dbPath];
        if (_databaseQueue == nil) {
            
            NSLog(@"数据库创建失败");
            
            [NSException raise:NSInternalInconsistencyException format:@"数据库创建异常"];
        }
        else
        {
            //创建一个表
            NSString *createTablSQL = @"CREATE TABLE IF NOT EXISTS T_PersonList (name text PRIMARY KEY NOT NULL, age integer NOT NULL,sex text,qqNumber text,phoneNumber text,weixinNumber text,headImagePath text,updateDate double)";
            
            [_databaseQueue inDatabase:^(FMDatabase *db) {
                
                BOOL ret = [db executeUpdate:createTablSQL];
                if (ret)
                {
                    NSLog(@"创建T_PersonList 表成功");
                }
                else
                {
                    NSLog(@"创建T_PersonList 表失败");
                }
            }];
        }
    }
    return self;
}
```

*在 XYJDatabaseManager.m 中也可以写对 Model（Person）数据的增、删、改、查方法，但是为了更加方便一些，我们创建一个 Person 的类别，在这个类别中一次实现上述方法。*

#### 创建的XYJPerson类别 （XYJPerson+database）

创建方法：Xcode -> File -> New -> File... 
选择iOS Source Objective-C File -> Next

![](http://7xvffo.com1.z0.glb.clouddn.com/category.png)

然后Next下去就好了。

在 `XYJPerson+database.h` 中声明常用对象的操作方法。

*首先得导入`#import "XYJDatabaseManager.h"`文件。*

```objc
/**
 *  添加或更新 一条数据到数据库中
 *
 *  @return 成功或失败
 */
- (BOOL) saveToDataBase;


/**
 *  插入一条数据到数据库中
 *
 *  @return 成功或失败
 */
- (BOOL) insertToDataBase;


/**
 *  根据名字修改数据库中的那条数据
 *
 *  @param lastName 修改之前的名字
 *
 *  @return 成功或失败
 */
- (BOOL) updateToDataBaseWithName:(NSString *) lastName;


/**
 *  从数据库中读出所有的人的信息
 *
 *  @return 所有的人数组
 */
+ (NSArray *) getAllPersonFromDataBase;


/**
 *  根据名字从数据库中查找人的信息
 *
 *  @param name 名字
 *
 *  @return 人的数组
 */
+ (NSArray *) getPersonFromDataBasewithName:(NSString *) name;


/**
 *  根据性别从数据库中查找人的信息
 *
 *  @param sex 性别
 *
 *  @return 人的数组
 */
+ (NSArray *) getPersonFromDataBasewithSex:(NSString *) sex;


/**
 *  根据名字从数据库中删除信息
 *
 *  @param name 要删除的名字
 *
 *  @return 成功或失败
 */
+ (BOOL) deleteFromDataBaseByName:(NSString *) name;
```

接着是去 `XYJPerson+database.m` 中文件中实现前面声明的方法：

##### 增加记录 

*将对象保存到数据库的表中，每个对象都是表中的一条记录！注意执行语句的关键词 `REPLACE INTO` *

*如果表中存在这条数据，就将这条数据替换掉，如果没有，则把这条数据加入到表中。*

```objc
- (BOOL)saveToDataBase
{
    NSString * replaceSQl = @"REPLACE INTO T_PersonList(name, age, sex, qqNumber, phoneNumber, weixinNumber, headImagePath ,updateDate) VALUES(?, ?, ?, ?, ?, ?, ?, ?)";
    
    __block BOOL ret = NO;
    
    [[XYJDatabaseManager shareManager].databaseQueue inDatabase:^(FMDatabase *db) {
        
        ret = [db executeUpdate:replaceSQl,self.name,@(self.age),self.sex,self.QQnumber,self.phoneNumber,self.weixinNumber,self.headImagePath,@(self.updateDate)];
    }];
    
    return ret;
}
```

*插入一条数据到数据库的表中;注意执行语句的关键词 `INSERT INTO`*

```objc
- (BOOL)insertToDataBase
{
    NSString * replaceSQl = @"INSERT INTO T_PersonList(name, age, sex, qqNumber, phoneNumber, weixinNumber, headImagePath ,updateDate) VALUES(?, ?, ?, ?, ?, ?, ?, ?)";
    
    __block BOOL ret = NO;
    
    [[XYJDatabaseManager shareManager].databaseQueue inDatabase:^(FMDatabase *db) {
        
        ret = [db executeUpdate:replaceSQl,self.name,@(self.age),self.sex,self.QQnumber,self.phoneNumber,self.weixinNumber,self.headImagePath,@(self.updateDate)];
    }];
    
    return ret;

}
```
##### 删除记录 

*删除根据名字查找到的那条数据;注意执行语句的关键词 `DELETE FROM...WHERE`*

```objc
+ (BOOL)deleteFromDataBaseByName:(NSString *) name
{
    NSString * deleteSQl = @"DELETE FROM T_PersonList WHERE name = ?";
    
    __block BOOL ret = NO;
    
    [[XYJDatabaseManager shareManager].databaseQueue inDatabase:^(FMDatabase *db) {
        
        ret = [db executeUpdate:deleteSQl,name];
    }];
    
    return ret;

}
```


##### 修改记录 

*修改根据名字查找到的那条数据;注意执行语句的关键词 `UPDATE...SET...WHERE`*

```objc
- (BOOL)updateToDataBaseWithName:(NSString *)lastName
{
    NSString * replaceSQl = @"UPDATE T_PersonList SET name = ?, age = ?, sex = ?, qqNumber = ?, phoneNumber = ?, weixinNumber  = ?, headImagePath = ? ,updateDate= ? WHERE name = ?";
    
    __block BOOL ret = NO;
    
    [[XYJDatabaseManager shareManager].databaseQueue inDatabase:^(FMDatabase *db) {
        
        ret = [db executeUpdate:replaceSQl,self.name,@(self.age),self.sex,self.QQnumber,self.phoneNumber,self.weixinNumber,self.headImagePath,@(self.updateDate),lastName];
    }];
    
    return ret;

}
```
##### 查询记录 

*查询到的结果是放在一个 FMResultSet（结果集）中的，遍历这个结果集，将相关数据添加到 Person 对象中，最后以数组的方式返回。*

*根据名字查询数据;注意执行语句的关键词 `SELECT * FROM...WHERE`*

```objc
+ (NSArray *)getPersonFromDataBasewithName:(NSString *)name
{
    NSString * querrySQL = @"SELECT * FROM T_PersonList WHERE name = ?";
    
    NSMutableArray * result = [NSMutableArray array];
    
    [[XYJDatabaseManager shareManager].databaseQueue inDatabase:^(FMDatabase *db) {
        
        FMResultSet * rs = [db executeQuery:querrySQL,name];
        
        while ([rs next]) {
            
            XYJPerson * person = [[XYJPerson alloc] init];
            
            //给模型赋值
            person.name =   [rs stringForColumn:@"name"];
            person.age  =   [rs intForColumn:@"age"];
            person.sex = [rs stringForColumn:@"sex"];
            person.QQnumber = [rs stringForColumn:@"qqNumber"];
            person.phoneNumber = [rs stringForColumn:@"phoneNumber"];
            person.weixinNumber = [rs stringForColumn:@"weixinNumber"];
            person.updateDate = [rs doubleForColumn:@"updateDate"];
            person.headImagePath = [rs stringForColumn:@"headImagePath"];
            
            [result addObject:person];
        }
        
    }];
    
    return result;
}
```

*根据性别查询数据*

```objc
+ (NSArray *)getPersonFromDataBasewithSex:(NSString *)sex
{
    
    NSString * querrySQL = @"SELECT * FROM T_PersonList WHERE sex = ?";
    
    NSMutableArray * result = [NSMutableArray array];
    
    [[XYJDatabaseManager shareManager].databaseQueue inDatabase:^(FMDatabase *db) {
        
        FMResultSet * rs = [db executeQuery:querrySQL,sex];
        
        while ([rs next]) {
            
            XYJPerson * person = [[XYJPerson alloc] init];
            
            //给模型赋值
            person.name =   [rs stringForColumn:@"name"];
            person.age  =   [rs intForColumn:@"age"];
            person.sex = [rs stringForColumn:@"sex"];
            person.QQnumber = [rs stringForColumn:@"qqNumber"];
            person.phoneNumber = [rs stringForColumn:@"phoneNumber"];
            person.weixinNumber = [rs stringForColumn:@"weixinNumber"];
            person.updateDate = [rs doubleForColumn:@"updateDate"];
            person.headImagePath = [rs stringForColumn:@"headImagePath"];
            
            [result addObject:person];
        }
        
    }];
    
    return result;

}
```

*查询表中的所有数据；并根据添加的时间先后顺序排序*

```objc
+ (NSArray *)getAllPersonFromDataBase
{
    //根据时间先后顺序排序
    //ASC 升序 DESC 降序
    NSString * querrySQL = @"SELECT * FROM T_PersonList ORDER BY updateDate ASC";
    
    NSMutableArray * result = [NSMutableArray array];
    
    [[XYJDatabaseManager shareManager].databaseQueue inDatabase:^(FMDatabase *db) {
        
        FMResultSet * rs = [db executeQuery:querrySQL];
        
        while ([rs next]) {
            
            XYJPerson * person = [[XYJPerson alloc] init];
            
            //给模型赋值
            person.name =   [rs stringForColumn:@"name"];
            person.age  =   [rs intForColumn:@"age"];
            person.sex = [rs stringForColumn:@"sex"];
            person.QQnumber = [rs stringForColumn:@"qqNumber"];
            person.phoneNumber = [rs stringForColumn:@"phoneNumber"];
            person.weixinNumber = [rs stringForColumn:@"weixinNumber"];
            person.updateDate = [rs doubleForColumn:@"updateDate"];
            person.headImagePath = [rs stringForColumn:@"headImagePath"];
            
            [result addObject:person];
        }
        
    }];
    
    return result;
}
```

*到这里，我们基本的方法都已经写完了，接下来就剩下来调用了。搭建个简易的 UI 界面测试一下*

#### FMDB 的测试

搭建一个 Input 界面：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/add.PNG" width="320" height="568" alt="选项列表图"/>
</div>


*添加数据的调用方法：*

```objc
BOOL ret = [person insertToDataBase];
            
//这样也可以
//BOOL ret = [person saveToDataBase];
            
if (ret) {   
   NSLog(@"插入数据 到数据库成功");              
} else {
   NSLog(@"插入数据 到数据库失败");
}
```

搭建一个修改信息的界面：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/change.PNG" width="320" height="568" alt="选项列表图"/>
</div>

*修改数据的调用方法：*

```objc
if ([person updateToDataBaseWithName:self.lastName]) {
	  NSLog(@"更新数据 到数据库成功");
}
else
{
	  NSLog(@"更新数据 到数据库失败");
}
```

将TableViewCell向左滑可以删除信息：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/delete.PNG" width="320" height="568" alt="选项列表图"/>
</div>

*删除数据的调用方法：*

```objc
//删除数据库数据
[XYJPerson deleteFromDataBaseByName:person.name];
```

*查询所有数据的调用方法*

```objc
//从数据库中取出所有的用户
NSArray * allPerson = [XYJPerson getAllPersonFromDataBase ];
```

一个根据性别查询信息的界面：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/searchsex.PNG" width="320" height="568" alt="选项列表图"/>
</div>

*根据性别查找数据库的调用方法*

```objc
array = [XYJPerson getPersonFromDataBasewithSex:@"女"];
```

一个根据名字查询信息的界面：

<div align=center>
<img src="http://7xvffo.com1.z0.glb.clouddn.com/searchname.PNG" width="320" height="568" alt="选项列表图"/>
</div>

*根据名字查找数据库的调用方法*


```objc
array =  [XYJPerson getPersonFromDataBasewithName:@"要查询的名字"];
```

### FMDB 的使用(线程不是安全的)

*这个暂未更新，后续会补上的...*

