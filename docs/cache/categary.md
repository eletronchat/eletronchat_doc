#### 数据的分类缓存

先定下：  
数据库[0] 存成员登录信息  
数据库[1] 存客户连接信息  
数据库[1] 存客户连接信息  
数据库[2] 存聊天记录  
数据库[3] 存各种消息处理队列   
  &nbsp;客户等待队列 (waiting__guest__queue) (存client_id, 指向数据库1)   
  &nbsp;客户错过队列 (guest_be__missing) (存client_id, 指向数据库1) 当客户连接时没人在客服工作台工作中就放入这里。
  &nbsp;客户座席队列(chat_waiting_group)  
  &nbsp;客户当前正在聊天的用户(hash) 记录用户的座席和用户,离开后把坐席入队    
  &nbsp;当前在客服线集合 chat__servers (uid 集合)   
数据库[4] 存客服连接指针（存member_token）  
数据库[5] 存管理员连接指针（存member_token）  

### 消息订阅
>为了不其它费时的处理且将次要的业务，占用客户连接主进程,影响用户的响应时间，这类型的处理业务将
异步推送给其它专门的监听进程来处理，缩小用户的响应时间。 
>同收发消息的格式工一致。如:

#### 1  客户到连接层时需要更新一些数据就向监听进程推送消息。格式如下
``` PHP
'{
    "from"  : "/service/connect/guest/7f0000010b5700000002",       //由连接层发出要数据持久化更新的推送
    "to"    : "/service/listening/db/guest",                       //由于监听进程的db方法来持久化到guest表中 
    "time"  : 毫秒时间戳,
    "method": "PUT",                                               //是更新
    "data"  : [],
    "OR"    : "POST"                                               // 选项参数 如果更新失败就新增
    "key"   : "fingerprint"                                        // 选项参数 标注guest表的查找索引是fingerpring参数 
}'
```



  
  

