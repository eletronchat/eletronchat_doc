# 数据的缓存
> 为了提供给用极致的体验，数据的缓存能避免磁盘的`IO`瓶颈,当下内存缓存是不二的选择。
虽然`redis`带来了数据的高速调用，却不能提供`mysql`那种多索引的灵活，
所以为了完成的业务上的检索数据，不得不把缓存结构多样化设计来满足数据检索的需求
。
  缓存数据结构是为本应用的即时业务提供即时的数据而设计的，避免磁盘的`IO`带来了毫
  秒级响应。

## redis数据库布局图

```plantuml
@startuml

    database database0 [
        数据库0
    ----
        member_...
    ----
        member_...
    ----
        member_...
    ----
        ...
    ]
    note top of database0 
        save the data for online member
        type: hash
        key:  member_<token>
        value: {
            key1:val1, key2:val2, ...
        }
        ...
    end note

    database database1 [
        数据库1
        ---
        fingerprint1
        ---
        fingerprint1
        ---
        fingerprint1
        ---
        ...
    ]
    note top of database1 
        cache 2000 guest data order by create_time
        type: hash
        key: <fingerprint>
        value: {
            "ip":"0.0.0.0", "name":"foo", ...
        }
    end note
   
    database database2 [
        数据库2
        ---
        fingerprint1
        ---
        fingerprint1
        ---
        fingerprint1
        ---
        ...
    ]
    note top of database2 
        cache the log for 2000 guest  order by create_time
        type: set
        key: <fingerprint>
        value: { }
    end note
    
    database database3 {
        folder chat_connect {
            file chat_connect [
                cach all chat connect
                type: hash,
                field: client_id
                velue: db0_key
            ]
        }
    }
   note right
        用于保存当前客户连接
        field: client_id,
        value: member_token
   end note
@enduml

```
