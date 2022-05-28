---
title: Prisma入门
date: 2022-05-16 16:24:19
permalink:
tags:
    - Prisma
    - NodeJS
    - Javasctipt
    - TypeScript
categories:
- 技术生活
  - 编程技术
    - JavaScript
---

# Prisma 是什么
Prisma是适用于Node.js和TypeScript的下一代开源ORM。ORM:对象关系映射（Object Relational Mapping）是一种程序设计技术。简单来说ORM可以将我们的底层数据库的各种操作进行一定的封装，我们就可以通过更加熟悉的开发语言来书写对应的数据库命令，ORM则可以将这些数据库操作命令转换为对应的SQL语句。GitHub上star数量很高，开发很活跃，社区也不错。关键是它的异步性，让JavaScript也拥有了高效查询db的能力。
Prisma它包含以下工具：
- Prisma Client—— 自动生成且类型安全的数据库客户端，这里边指定了语言和数据库啥的
- Prisma Migrate—— 声明式数据建模和可自定义的迁移，和laravel或者thinkphp中的迁移概念类似
- Prisma Studio—— 现代化的用户界面，可查看和编辑数据，可以想象为phhmyadmin




# Prisma 环境配置
前提，nodejs必须安装
在这个前提下安装prisma
```
npm install prisma typescript ts-node @types/node --save-dev
```


# Prisma 基本用法
## 创建项目
```
mkdir prisma
npm init -y 
```
上面的命会初始化一个npm项目，然后安装prisma依赖
```
npm install prisma -D 
```
安装之后，我们就可以使用npx命令了。 可以通过npx prisma init -h命令来查看prisma的命令
![20220516164320](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220516164320.png)

我们看到了prisma帮助信息中有一个init命令，这个就是我们用来初始化prisma的关键命令，这里我们初始化一个mysql的数据库文件

init指定数据库的格式是：*** DATABASE_URL="mysql://<USER>:<PASSWORD>@HOST:PORT/DATABASE" ***
```
npx prisma init --url mysql://root:123456@localhost:3306/mydb
```

来看看，执行之后，这个命令做了那些事情
1. 生成了一个.env文件
2. 生成了prisma文件夹，里面有一个schema.prisma这个文件是我们后面用到的关键
   
在根目录目前下面的文件大致如下
![20220516165016](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220516165016.png)


## prisma使用
### 使用 Prisma Migrate 创建数据表
目前schema.prisma文件长这样。定义了两个表模型，User和Profile ，这两个表通过userId关联
```
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}


model User {
  id      Int      @id @default(autoincrement())
  email   String   @unique
  name    String?
  profile Profile?
} 

model Profile {
  id     Int     @id @default(autoincrement())
  bio    String?
  user   User    @relation(fields: [userId], references: [id])
  userId Int     @unique
}


```

### 使用 Prisma
使用迁移文件初始化数据库
```
npx prisma migrate dev --name init

```

看看执行这个命令发生了什么吧
1. 生成数据库迁移文件
2. 执行数据库迁移文件
![20220516170012](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220516170012.png)

执行CRUD
在执行之前需要创建一个ts文件，这里我们创建一个index.ts的文件` 
1. 创建一个用户
```
  // create 
    const  c = await prismaClient.user.create({
       data:{
           name:'xx',
           email:'123@qqq.com',
           profile:{
               create:{bio:'im a girl'}
           }
       },
    });
```
2. 查询一个用户
```
    const user =  prismaClient.user.findFirst();
    console.dir(user, { depth: null }) 
```

完整代码
```
import {  PrismaClient } from '@prisma/client'
const prismaClient = new PrismaClient();

async function main() { 
    // create 
    const  c = await prismaClient.user.create({
       data:{
           name:'xx',
           email:'123@qqq.com',
           profile:{
               create:{bio:'im a girl'}
           }
       },
    });
    
    console.log('create user status = ' + c );
    const user =  prismaClient.user.findFirst();
    console.dir(user, { depth: null }) 

}


main()
  .catch((e) => {
    throw e
  }).finally(async () => {
    await prismaClient.$disconnect()
})

```



运行


```
npx ts-node index.ts 


```

数据库运行效果
![20220516173323](https://cdn.jsdelivr.net/gh/it114/blogcdn@master/blog/images20220516173323.png)