# 整体架构

> 本应用是采用`thinkphp 6` + `workerman-gateway` 这2个框架的组合来开发的，`thinkphp`
负责处理`http`的请求，而`workerman-gateway` 负责处理`websocket`的长连接处理。

## 大致划分
``` BASH
数据库
   # 用于权限验证功能
       think_auth_group           权限用户组表
       think_auth_group_access    权限分组表
       think_auth_rule            规则表
       think_member               用户表
   # 用户系统配置 
       think_config               系统配置表
   # 用于成员分组功能
       think_member_group         成员用户分组表
       think_member_group_access  用户成员分组表
       (以及上面的用户表)

数据缓存 (用于提高数据查询性能)
     member_ 开头键名 用户缓存用户信息

服务器
   http 服务端 (基于 thinkphp 6.0)
     api 模块
   websocket 服务端 (基于 workerman-gateway )

UI
  管理控制台 （layuiadmin）
  客户工作台  （layuiadmin）
```
## 目录结构

```BASH
├── app
│   ├── api                     http api 应用
│   ├── build.php
│   ├── chat                    websocket 应用
│   ├── common.php
│   ├── event.php
│   ├── ExceptionHandle.php
│   ├── index
│   ├── lib
│   ├── middleware.php
│   ├── provider.php
│   └── Request.php
├── composer.json
├── composer.lock
├── config
│   ├── api
│   ├── app.php
│   ├── cache.php
│   ├── console.php
│   ├── cookie.php
│   ├── database.php        数据库配置文件
│   ├── filesystem.php
│   ├── lang.php
│   ├── log.php
│   ├── route.php
│   ├── session.php
│   ├── template.php
│   └── trace.php
├── extend
├── public
│   ├── admin                管理员控制台前端应用
│   ├── chat                 客户工作台前端应用
│   ├── favicon.ico
│   ├── index.php
│   ├── json
│   ├── robots.txt
│   ├── router.php
│   ├── static
│   └── test.html
├── route
│   ├── api                  api 应用路由配置文件
│   ├── index
│   └── route.php
├── runtime
│   ├── api
│   ├── index
│   ├── services.php
│   └── session
├── start.php               websocket 服务器启动文件
├── tags
├── think
└── vendor
```

## 客户连接UML图示
```plantuml
@startuml
actor Guest
Guest->Event业务处理层:http请求建立websocket连接
activate Guest #FFBBBB
activate Event业务处理层

Event业务处理层->Event业务处理层:当前是否有客服当值
activate Event业务处理层 #DarkSalmon

Event业务处理层 o-> Guest客户处理类:有客服当值，请求对接
activate Guest客户处理类

Guest客户处理类 -> Guest客户处理类: 当前是否有空闲座席
activate Guest客户处理类 #DarkSalmon

Guest客户处理类 o-> Guest: 有空闲座席，对接成功

Guest客户处理类 -> Guest客户处理类:无空闲座席，进入等待队列
destroy Guest客户处理类

Guest客户处理类 --> 打杂进程类: 打杂的，收集这个客户信息并持久化

Guest客户处理类 -> Guest: 当前正在等待人数为n人
deactivate Guest客户处理类

Event业务处理层 x-> Guest客户处理类:缓存在错失队列
activate Guest客户处理类

Guest客户处理类 -> Guest:告知当前没有人当值
deactivate Guest客户处理类 
deactivate Guest

@enduml
```
