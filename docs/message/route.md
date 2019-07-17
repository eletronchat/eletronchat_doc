#### 说明 

##### 服务器端消息路由
!> 服务器端的路由都是`/service/`开头的
`websocket`的服务器业务处理分为3个部分。分别是:连接处理层，消息处理层和断开处理层。  
而每个层的都有对应的开头路由。分别是连接层`/service/connnect`, 消息处理层`/service/message`和断开处理
层`/service/close`。对应的处理层的相关服务处理，以对应的路由开头。以下是路由对应的业务列表。

| 路由                                 | 对应的服务 | 业务层名 | 备注 |
| ---                                  | ---        | ---      | ---  |
| `/service/connect/guest/<client_id>` | 新客户连接 | 连接层   |      |
| `/local/chat/index/message/onlineList` |  客服工作台聊天用户在线列表 |   客服工作台   |      |
