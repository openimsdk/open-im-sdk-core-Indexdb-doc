# Js实现db接口简要说明(待更新)
## 表结构以及需要实现的接口说明
>所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回
### 1.用户表
- 表名：local_users
```json
CREATE TABLE `local_users` (`user_id` varchar(64),`name` varchar(255),`face_url` varchar(255),`gender` integer,`phone_number` varchar(32),`birth` integer,`email` varchar(64),`create_time` integer,`app_manger_level` integer,`ex` varchar(1024),`attached_info` varchar(1024),`global_recv_msg_opt` integer,PRIMARY KEY (`user_id`))
```
#### 接口说明：
- GetLoginUser
  
| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| userID      | string                                          | 用户ID ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果获取不到用户信息也需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | LocalUser（用户表对象数据） |对象转换成string|

- InsertLoginUser
  
| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| user       object                                          | LocalUser（用户表对象数据） ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||
   
- UpdateLoginUserByMap
  
| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| userID     |string                                       |  用户ID ||
| args     |object                                       |  更新字段参数对象 |内部是kv，k为字段名，v为需要更新的字段内容|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|如果没有更新到一行，需要返回失败|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||
   

### 2.消息表(写扩散消息表)
- 表名：local_chat_logs
```
CREATE TABLE `local_chat_logs` (`client_msg_id` char(64),`server_msg_id` char(64),`send_id` char(64),`recv_id` char(64),`sender_platform_id` integer,`sender_nick_name` varchar(255),`sender_face_url` varchar(255),`session_type` integer,`msg_from` integer,`content_type` integer,`content` varchar(1000),`is_read` numeric,`status` integer,`seq` integer DEFAULT 0,`send_time` integer,`create_time` integer,`attached_info` varchar(1024),`ex` varchar(1024),PRIMARY KEY (`client_msg_id`))
```
#### 接口说明：
GetMessage
SetChatLogFailedStatus
GetNormalMsgSeq
GetAbnormalMsgSeq
UpdateMessageTimeAndStatus
BatchUpdateMessageList
UpdateMessage
BatchInsertMessageList
InsertMessage
GetMultipleMessage

### 3.会话表
- 表名：local_conversations
```
CREATE TABLE `local_conversations` (`conversation_id` char(128),`conversation_type` integer,`user_id` char(64),`group_id` char(128),`show_name` varchar(255),`face_url` varchar(255),`recv_msg_opt` integer,`unread_count` integer,`group_at_type` integer,`latest_msg` varchar(1000),`latest_msg_send_time` integer,`draft_text` text,`draft_text_time` integer,`is_pinned` numeric,`is_private_chat` numeric,`is_not_in_group` numeric,`update_unread_count_time` integer,`attached_info` varchar(1024),`ex` varchar(1024),PRIMARY KEY (`conversation_id`))
```
#### 接口说明：
GetAllConversationList
GetConversation
GetAllConversationListToSync
BatchInsertConversationList
UpdateConversationForSync
BatchUpdateConversationList
BatchInsertConversationList
GetHiddenConversationList


### 4. 读扩散大群表
- 表名：local_super_groups
```
CREATE TABLE `local_super_groups` (`group_id` varchar(64),`name` text,`notification` varchar(255),`introduction` varchar(255),`face_url` varchar(255),`create_time` integer,`status` integer,`creator_user_id` varchar(64),`group_type` integer,`owner_user_id` varchar(64),`member_count` integer,`ex` varchar(1024),`attached_info` varchar(1024),`need_verification` integer,`look_member_info` integer,`apply_member_friend` integer,`notification_update_time` integer,`notification_user_id` text,PRIMARY KEY (`group_id`))
```
#### 接口说明：
GetJoinedSuperGroupList
InsertSuperGroup
UpdateSuperGroup
DeleteSuperGroup
GetReadDiffusionGroupIDList
GetSuperGroupInfoByGroupID

### 5. 读扩散消息表
- 表名：local_sg_chat_logs_3592883556
>注：读扩散表为动态生成，表名也是，规则为local_sg_chat_logs_+groupID，
原有代码中，是在SuperGroupGetMessageListNoTime和SuperGroupGetMessage这两个函数中进行判断，如果没有就动态生成该表
```
CREATE TABLE `local_sg_chat_logs_3592883556` (`client_msg_id` char(64),`server_msg_id` char(64),`send_id` char(64),`recv_id` char(64),`sender_platform_id` integer,`sender_nick_name` varchar(255),`sender_face_url` varchar(255),`session_type` integer,`msg_from` integer,`content_type` integer,`content` varchar(1000),`is_read` numeric,`status` integer,`seq` integer DEFAULT 0,`send_time` integer,`create_time` integer,`attached_info` varchar(1024),`ex` varchar(1024),PRIMARY KEY (`client_msg_id`))
```
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


