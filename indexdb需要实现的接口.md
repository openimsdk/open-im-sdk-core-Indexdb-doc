# Js实现db接口简要说明(待更新)
## 表结构以及需要实现的接口说明
>所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回
### 1.用户表
- 表名：local_users
```sqlite
CREATE TABLE `local_users` (`user_id` varchar(64),`name` varchar(255),`face_url` varchar(255),`gender` integer,`phone_number` varchar(32),`birth` integer,`email` varchar(64),`create_time` integer,`app_manger_level` integer,`ex` varchar(1024),`attached_info` varchar(1024),`global_recv_msg_opt` integer,PRIMARY KEY (`user_id`))
```

#### 接口说明：
- getLoginUser
  
| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| userID      | string                                          | 用户ID ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果获取不到用户信息也需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | LocalUser（用户表对象数据） |对象转换成string|

**参考sql语句说明：**

```sql
SELECT * FROM `local_users` WHERE user_id = "3045326383"  LIMIT 1;
```
- insertLoginUser
  
| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| user                                               | string  |LocalUser（用户表对象数据）|对象转换成string

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||
**参考sql语句说明：**

```sql
INSERT INTO `local_users` (`user_id`,`name`,`face_url`,`gender`,`phone_number`,`birth`,`email`,`create_time`,`app_manger_level`,`ex`,`attached_info`,`global_recv_msg_opt`) VALUES ("3045326383","Gordon","ic_avatar_01",0,"18349115126",0," ",0,1,"","",0)

```
- updateLoginUserByMap
  
| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| userID     |string                                       |  用户ID ||
| args     |object                                       |  更新字段参数对象 |内部是kv，k为字段名，v为需要更新的字段内容|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|如果没有更新到一行，需要返回失败|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||
**参考sql语句说明：**

```sql
UPDATE `local_users` SET `app_manger_level`=1,`attached_info`="",`birth`=0,`create_time`=0,`email`=" ",`ex`="",`face_url`="ic_avatar_01",`gender`=0,`global_recv_msg_opt`=0,`name`="Gordon111",`phone_number`="18349115126" WHERE `user_id` = "3045326383";
```


### 2.消息表(写扩散消息表)
- 表名：local_chat_logs
```sqlite
CREATE TABLE `local_chat_logs` (`client_msg_id` char(64),`server_msg_id` char(64),`send_id` char(64),`recv_id` char(64),`sender_platform_id` integer,`sender_nick_name` varchar(255),`sender_face_url` varchar(255),`session_type` integer,`msg_from` integer,`content_type` integer,`content` varchar(1000),`is_read` numeric,`status` integer,`seq` integer DEFAULT 0,`send_time` integer,`create_time` integer,`attached_info` varchar(1024),`ex` varchar(1024),PRIMARY KEY (`client_msg_id`))
```

- 表结构特别说明
  
  | 字段名     | 类型                                                         | 说明 |
  | --------- | ------------------------------------------------------------ | ----- |
  | is_read      | bool                                         | true已读，false 未读|

  

#### 接口说明：
- getMessage

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| clientMsgID      | string                                          | 消息ID ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果获取不到消息也需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | LocalChatLog（消息表对象数据） |对象转换成string|
**参考sql语句说明：**

```sql
SELECT * FROM `local_chat_logs` WHERE client_msg_id = "063031b86f8e503c6038efb2b835f216" LIMIT 1

```


[comment]: <> "- setChatLogFailedStatus"
- getSendingMessageList

 **无输入参数**

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          |  []LocalChatLog（消息表对象数组数据） |对象转换成string|

**参考sql语句说明：**

```sql
select * from local_chat_logs where status = 1;
-- 消息状态：
-- 	MsgStatusSending     = 1
-- 	MsgStatusSendSuccess = 2
-- 	MsgStatusSendFailed  = 3
-- 	MsgStatusHasDeleted  = 4
-- 	MsgStatusRevoked     = 5
-- 	MsgStatusFiltered    = 6
```

- getNormalMsgSeq
  

**无输入参数**

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | number                                    | 消息表中最大seq ||

**参考sql语句说明：**

```sql
SELECT IFNULL(max(seq),0) FROM `local_chat_logs`;
```


- updateMessageTimeAndStatus

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| clientMsgID | string                                     | 客户端消息ID       ||
| serverMsgID | string | 服务器消息ID ||
| sendTime | number | 消息发送时间，毫秒 ||
| status | number | 消息状态，参照前面 ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果没更新到任何一行消息也返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sql
 UPDATE `local_chat_logs` SET `server_msg_id`="75dee2fbd6c4f28e7895f8410be4984f",`status`=2,`send_time`=1663658950513 WHERE client_msg_id="985261c57242cf647753839854038154" And seq=0;
```

BatchUpdateMessageList

- updateMessage

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| clientMsgID | string                                     | 客户端消息ID       ||
| args        | object | 更新字段参数对象 |内部是kv，k为字段名，v为需要更新的字段内容Js实现db接口简要说明|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果没更新到任何一行消息也返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sql
 UPDATE `local_chat_logs` SET `server_msg_id`="8c6dc2ace8ff5706880018de43916c39",`recv_id`="2041671273",`session_type`=1,`status`=2,`seq`=14 WHERE `client_msg_id` = "6edad80249cc0cf626edb88e64f8fb6d";
```



BatchInsertMessageList

 INSERT INTO `local_chat_logs` (`client_msg_id`,`server_msg_id`,`send_id`,`recv_id`,`sender_platform_id`,`sender_nick_name`,`sender_face_url`,`session_type`,`msg_from`,`content_type`,`content`,`is_read`,`status`,`seq`,`send_time`,`create_time`,`attached_info`,`ex`) VALUES ("01d22adafe309482391fc54f728c96a7","5cb3ecb6092843e9376fd229a72b24e0","3045326383","openIMAdmin",0,"","",1,200,1303,"{\"detail\":\"CgozMDQ1MzI2Mzgz\",\"defaultTips\":\"remove a blocked user\",\"jsonDetail\":\"{\\"userID\\":\\"3045326383\\"}\"}",false,6,1,1663557189653,1663557189652,"",""),("3492347bc55280d3c5c32398f78eae50","2b0a01eff07f502da31751fb459966bc","3045326383","openIMAdmin",0,"","",1,200,1303,"{\"detail\":\"CgozMDQ1MzI2Mzgz\",\"defaultTips\":\"remove a blocked user\",\"jsonDetail\":\"{\\"userID\\":\\"3045326383\\"}\"}",false,6,2,1663557324330,1663557324329,"","");

InsertMessage

 INSERT INTO `local_chat_logs` (`client_msg_id`,`server_msg_id`,`send_id`,`recv_id`,`sender_platform_id`,`sender_nick_name`,`sender_face_url`,`session_type`,`msg_from`,`content_type`,`content`,`is_read`,`status`,`seq`,`send_time`,`create_time`,`attached_info`,`ex`) VALUES ("6edad80249cc0cf626edb88e64f8fb6d","","3045326383","2041671273",1,"Gordon111","ic_avatar_01",1,100,101,"Single chat test3045326383:2041671273:",false,1,0,1663658716992,1663658716992,"","")

GetMultipleMessage

### 3.会话表
- 表名：local_conversations
```sqlite
CREATE TABLE `local_conversations` (`conversation_id` char(128),`conversation_type` integer,`user_id` char(64),`group_id` char(128),`show_name` varchar(255),`face_url` varchar(255),`recv_msg_opt` integer,`unread_count` integer,`group_at_type` integer,`latest_msg` varchar(1000),`latest_msg_send_time` integer,`draft_text` text,`draft_text_time` integer,`is_pinned` numeric,`is_private_chat` numeric,`is_not_in_group` numeric,`update_unread_count_time` integer,`attached_info` varchar(1024),`ex` varchar(1024),PRIMARY KEY (`conversation_id`))
```
- 表结构特别说明

  | 字段名     | 类型                                                         | 说明 |
    | --------- | ------------------------------------------------------------ | ----- |
  | is_pinned      | bool                                         | 是否置顶，true置顶，false 取消置顶|
  | is_private_chat      | bool                                         | 是否开启阅后即焚，true开启，false关闭|
  | is_not_in_group      | bool                                         | 是否不在群里，true不在群里，false 在群里，暂时未启|
#### 接口说明：
GetAllConversationList

SELECT * FROM `local_conversations` WHERE latest_msg_send_time > 0 ORDER BY case when is_pinned=1 then 0 else 1 end,max(latest_msg_send_time,draft_text_time) DESC



GetConversation

SELECT * FROM `local_conversations` WHERE conversation_id = "single_2041671273" LIMIT 1

GetAllConversationListToSync

SELECT * FROM `local_conversations`

BatchInsertConversationList
UpdateConversationForSync
BatchUpdateConversationList
GetHiddenConversationList

SELECT * FROM `local_conversations` WHERE latest_msg_send_time = 0



```
UpdateColumnsConversation
```

 UPDATE `local_conversations` SET `latest_msg`="{\"clientMsgID\":\"985261c57242cf647753839854038154\",\"createTime\":1663658950833,\"sendTime\":1663658950833,\"sessionType\":1,\"sendID\":\"3045326383\",\"recvID\":\"2041671273\",\"msgFrom\":100,\"contentType\":101,\"platformID\":1,\"senderNickname\":\"Gordon111\",\"senderFaceUrl\":\"ic_avatar_01\",\"content\":\"Single chat test3045326383:2041671273:\",\"seq\":0,\"isRead\":false,\"status\":1,\"offlinePush\":{},\"pictureElem\":{\"sourcePicture\":{\"size\":0,\"width\":0,\"height\":0},\"bigPicture\":{\"size\":0,\"width\":0,\"height\":0},\"snapshotPicture\":{\"size\":0,\"width\":0,\"height\":0}},\"soundElem\":{\"dataSize\":0,\"duration\":0},\"videoElem\":{\"videoSize\":0,\"duration\":0,\"snapshotSize\":0,\"snapshotWidth\":0,\"snapshotHeight\":0},\"fileElem\":{\"fileSize\":0},\"mergeElem\":{},\"atElem\":{\"isAtSelf\":false},\"faceElem\":{\"index\":0},\"locationElem\":{\"longitude\":0,\"latitude\":0},\"customElem\":{},\"quoteElem\":{},\"notificationElem\":{},\"messageEntityElem\":{},\"attachedInfoElem\":{\"groupHasReadInfo\":{\"hasReadCount\":0,\"groupMemberCount\":0},\"isPrivateChat\":false,\"hasReadTime\":0,\"notSenderNotificationPush\":false,\"isEncryption\":false,\"inEncryptStatus\":false}}",`latest_msg_send_time`=1663658950833 WHERE `conversation_id` = "single_2041671273";




### 4. 读扩散大群表
- 表名：local_super_groups
```sqlite
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

