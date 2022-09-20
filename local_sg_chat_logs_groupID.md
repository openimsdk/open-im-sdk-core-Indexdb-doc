# Js实现db接口简要说明(待更新)
## 表结构以及需要实现的接口说明
>所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回
###  读扩散消息表
- 表名：local_sg_chat_logs_3592883556
>注：读扩散表为动态生成，表名也是，规则为local_sg_chat_logs_+groupID，
原有代码中，是在SuperGroupGetMessageListNoTime和SuperGroupGetMessage这两个函数中进行判断，如果没有就动态生成该表
```sqlite
CREATE TABLE `local_sg_chat_logs_3592883556` (`client_msg_id` char(64),`server_msg_id` char(64),`send_id` char(64),`recv_id` char(64),`sender_platform_id` integer,`sender_nick_name` varchar(255),`sender_face_url` varchar(255),`session_type` integer,`msg_from` integer,`content_type` integer,`content` varchar(1000),`is_read` numeric,`status` integer,`seq` integer DEFAULT 0,`send_time` integer,`create_time` integer,`attached_info` varchar(1024),`ex` varchar(1024),PRIMARY KEY (`client_msg_id`))
```

- 表结构特别说明

  | 字段名     | 类型                                                         | 说明 |
        | --------- | ------------------------------------------------------------ | ----- |
  | is_read      | bool                                         | true已读，false 未读|

#### 接口说明：
GetSuperGroupNormalMsgSeq
GetSuperGroupAbnormalMsgSeq
InitSuperLocalChatLog
SuperGroupGetMessage
SuperGroupUpdateMessage
SuperGroupBatchInsertMessageList    
SuperGroupInsertMessage
SuperGroupGetMultipleMessage
SuperGroupUpdateMessageTimeAndStatus












SuperBatchInsertExceptionMsg
BatchInsertExceptionMsg

