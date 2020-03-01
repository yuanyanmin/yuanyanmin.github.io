---
title: mongodb-创建用户
date: 2020-02-29 15:27:21
tags: mongodb
categories:
toc:
---
前言：

平日只是对表的增删，很少建库进行操作（本地一般没bug,公司一般没权限），今日踩坑。

踩坑：在使用 mongodb 连接某个特定的数据库的时候，提示 Error: Authentication failed.

原因：未创建用户

<!--more-->

**错误信息**
![](/photo/mongodb/error_1.jpg)

**解决方法**
```
// 1. 先创建数据库（此处已经创建）
// 2. 创建用户
use test(数据库名)
db.createUser({
  user: "admin",
  pwd: "admin",
  roles: [{
    role: "readWrite",
    db: "test"
  }]
})

// 成功后的信息
Successfully added user: {
  "user" : "test",
  "roles" : [
    {
      "role" : "readWrite",
      "db" : "test"
    }
  ]
}
// 输入用户名、密码，重新连接
```

**补充**

```
db.createUser(user, writeConcern) 
```
* user: 这个文档创建关于用户的身份认证和访问信息；
* writeConcern: 这个文档描述保证MongoDB提供写操作的成功报告

**user 文档**
```
{
  user: "name",
  pwd: "pwd",
  customData: {},     // 任意内容，例如可以为用户全名介绍
  roles: [
    {role: "role", db: "database"} | "role",
    ...
  ]
}
```

MongoDB 内置角色如下：
1. 数据库用户角色：read、readWrite;
2. 数据库管理角色：dbAdmin、dbOwner、userAdmin;
3. 集群管理角色：clusterAdmin、clusteraManager、clusterMonitor、hostManager;
4. 备份恢复角色：backup、restore;
5. 所有数据库角色：readAnyDatabase、readWriteDatabase、userAdminDatabase、dbAdminAnyDatabase
6. 超级用户角色：root(这里还有几个间接或直接提供了系统超级用户的访问（dbOwner、userAdmin、userAdminAnyDatabase）)
7. 内部角色：__system

**实例**

例如：在products数据库创建用户accountAdmin01，并给该用户admin数据库上clusterAdmin和readAnyDatabase的角色，products数据库上readWrite角色。

```
use products
db.createUser( { "user" : "accountAdmin01",
                 "pwd": "cleartext password",
                 "customData" : { employeeId: 12345 },
                 "roles" : [ { role: "clusterAdmin", db: "admin" },
                             { role: "readAnyDatabase", db: "admin" },
                             "readWrite"
                             ] },
               { w: "majority" , wtimeout: 5000 } )
```

**验证**

```
mongo -u accountAdmin01 -p yourpassword --authenticationDatabase products
```