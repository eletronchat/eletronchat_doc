### 整体架构

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
