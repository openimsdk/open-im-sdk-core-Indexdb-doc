# Js实现db接口简要说明(待更新)
## 表结构以及需要实现的接口说明
>所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回
### 会话表
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
- getAllConversationList

**无输入参数**

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | []LocalConversation（会话表对象数组数据） |对象数组转换成string|

**参考sql语句说明：**

```sql
SELECT * FROM `local_conversations` WHERE latest_msg_send_time > 0 ORDER BY case when is_pinned=1 then 0 else 1 end,max(latest_msg_send_time,draft_text_time) DESC;
```


- getConversation

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| conversationID      | string                                          | 会话ID ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果获取不到改会话信息也需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | LocalConversation（会话表对象） |对象转换成string|

**参考sql语句说明：**

```sql
SELECT * FROM `local_conversations` WHERE conversation_id = "single_2041671273" LIMIT 1;
```

- getAllConversationListToSync

**无输入参数**

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | []LocalConversation（会话表对象数组数据） |对象数组转换成string|

**参考sql语句说明：**

```sql
SELECT * FROM `local_conversations`;
```

BatchInsertConversationList
UpdateConversationForSync
BatchUpdateConversationList

-  getHiddenConversationList

**无输入参数**

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | []LocalConversation（会话表对象数组数据） |对象数组转换成string|

**参考sql语句说明：**

```sql
SELECT * FROM `local_conversations` WHERE latest_msg_send_time = 0;
```

- updateColumnsConversation

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| conversationID     |string                                       | 会话ID ||
| args     |object                                       |  更新字段参数对象 |内部是kv，k为字段名，v为需要更新的字段内容|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|如果没有更新到一行，需要返回失败|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||

**参考sql语句说明：**

```sql
 UPDATE `local_conversations` SET `latest_msg`="{\"clientMsgID\":\"985261c57242cf647753839854038154\",\"createTime\":1663658950833,\"sendTime\":1663658950833,\"sessionType\":1,\"sendID\":\"3045326383\",\"recvID\":\"2041671273\",\"msgFrom\":100,\"contentType\":101,\"platformID\":1,\"senderNickname\":\"Gordon111\",\"senderFaceUrl\":\"ic_avatar_01\",\"content\":\"Single chat test3045326383:2041671273:\",\"seq\":0,\"isRead\":false,\"status\":1,\"offlinePush\":{},\"pictureElem\":{\"sourcePicture\":{\"size\":0,\"width\":0,\"height\":0},\"bigPicture\":{\"size\":0,\"width\":0,\"height\":0},\"snapshotPicture\":{\"size\":0,\"width\":0,\"height\":0}},\"soundElem\":{\"dataSize\":0,\"duration\":0},\"videoElem\":{\"videoSize\":0,\"duration\":0,\"snapshotSize\":0,\"snapshotWidth\":0,\"snapshotHeight\":0},\"fileElem\":{\"fileSize\":0},\"mergeElem\":{},\"atElem\":{\"isAtSelf\":false},\"faceElem\":{\"index\":0},\"locationElem\":{\"longitude\":0,\"latitude\":0},\"customElem\":{},\"quoteElem\":{},\"notificationElem\":{},\"messageEntityElem\":{},\"attachedInfoElem\":{\"groupHasReadInfo\":{\"hasReadCount\":0,\"groupMemberCount\":0},\"isPrivateChat\":false,\"hasReadTime\":0,\"notSenderNotificationPush\":false,\"isEncryption\":false,\"inEncryptStatus\":false}}",`latest_msg_send_time`=1663658950833 WHERE `conversation_id` = "single_2041671273";
```

- decrConversationUnreadCount
>注：该函数需要用到事务，未读数减去后，需要读取一次该会话的未读数（SELECT * FROM `local_conversations` WHERE conversation_id = "single_2992126880" AND `local_conversations`.`conversation_id` = "single_2992126880" LIMIT 1
），如果该会话未读数<0,需要回滚并返回错误

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| conversationID     |string                                       | 会话ID ||
| count     |number                                       |  会话未读数减去数量 ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||

**参考sql语句说明：**

```sql
 UPDATE `local_conversations` SET `unread_count`=unread_count-3 WHERE `conversation_id` = "single_2992126880";
```

