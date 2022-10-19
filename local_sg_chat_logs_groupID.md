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

- getSuperGroupNormalMsgSeq


| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID | string                                     | 群ID       ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | number                                    | 大群消息表中最大seq ||

**参考sql语句说明：**

```sql
SELECT IFNULL(max(seq),0) FROM `local_sg_chat_logs_812146266`;
```

[comment]: <> (GetSuperGroupAbnormalMsgSeq)

[comment]: <> (InitSuperLocalChatLog)

- superGroupGetMessage

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID | string                                     | 群ID       ||
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

- superGroupUpdateMessage

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID | string                                     | 群ID       ||
| clientMsgID | string                                     | 客户端消息ID       ||
| args        | object | 更新字段参数对象 |内部是kv，k为字段名，v为需要更新的字段内容Js实现db接口简要说明|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果没更新到任何一行消息也返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sql
UPDATE `local_sg_chat_logs_4280368097` SET `recv_id`="4280368097",`session_type`=3,`is_read`=true WHERE `client_msg_id` = "d9ef1e4e63394b856b8a781ef0234b49";
```

- superGroupBatchInsertMessageList

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| messageList                                     | string  | []LocalChatLog（消息表对象数组数据）|对象数组转换成string
| groupID | string                                     | 群ID       ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sql
INSERT INTO `local_sg_chat_logs_2203019508` (`client_msg_id`,`server_msg_id`,`send_id`,`recv_id`,`sender_platform_id`,`sender_nick_name`,`sender_face_url`,`session_type`,`msg_from`,`content_type`,`content`,`is_read`,`status`,`seq`,`send_time`,`create_time`,`attached_info`,`ex`) VALUES ("8c47b6a578bedd859f6349655408a398","40092d7eb666d7d564230c0c38f4b6d1","4137580800","2203019508",0,"","",3,0,1501,"{\"detail\":\"CjUKCjIyMDMwMTk1MDgSBTIyMjExMgo0MTM3NTgwODAwOLXq0JkGQAJaCjQxMzc1ODA4MDBgAhJPCgoyMjAzMDE5NTA4Ego0MTM3NTgwODAwGAIgterQmQYqAzEyMzIMaWNfYXZhdGFyXzA2OAFAAkoKNDEzNzU4MDgwMGIKNDEzNzU4MDgwMBpQCgoyMjAzMDE5NTA4EgozMzU5MzAzNDA3GAEgterQmQYqBkdvcmRvbjIMaWNfYXZhdGFyXzA2QAJKCjQxMzc1ODA4MDBiCjQxMzc1ODA4MDAaTQoKMjIwMzAxOTUwOBIKNDEzNzU4MDgwMBgCILXq0JkGKgMxMjMyDGljX2F2YXRhcl8wNkACSgo0MTM3NTgwODAwYgo0MTM3NTgwODAwKk0KCjIyMDMwMTk1MDgSCjQxMzc1ODA4MDAYAiC16tCZBioDMTIzMgxpY19hdmF0YXJfMDZAAkoKNDEzNzU4MDgwMGIKNDEzNzU4MDgwMA==\",\"defaultTips\":\"123 create the group\",\"jsonDetail\":\"{\\"group\\":{\\"groupID\\":\\"2203019508\\",\\"groupName\\":\\"22211\\",\\"ownerUserID\\":\\"4137580800\\",\\"createTime\\":1664365877,\\"memberCount\\":2,\\"creatorUserID\\":\\"4137580800\\",\\"groupType\\":2},\\"opUser\\":{\\"groupID\\":\\"2203019508\\",\\"userID\\":\\"4137580800\\",\\"roleLevel\\":2,\\"joinTime\\":1664365877,\\"nickname\\":\\"123\\",\\"faceURL\\":\\"ic_avatar_06\\",\\"appMangerLevel\\":1,\\"joinSource\\":2,\\"operatorUserID\\":\\"4137580800\\",\\"inviterUserID\\":\\"4137580800\\"},\\"memberList\\":[{\\"groupID\\":\\"2203019508\\",\\"userID\\":\\"3359303407\\",\\"roleLevel\\":1,\\"joinTime\\":1664365877,\\"nickname\\":\\"Gordon\\",\\"faceURL\\":\\"ic_avatar_06\\",\\"joinSource\\":2,\\"operatorUserID\\":\\"4137580800\\",\\"inviterUserID\\":\\"4137580800\\"},{\\"groupID\\":\\"2203019508\\",\\"userID\\":\\"4137580800\\",\\"roleLevel\\":2,\\"joinTime\\":1664365877,\\"nickname\\":\\"123\\",\\"faceURL\\":\\"ic_avatar_06\\",\\"joinSource\\":2,\\"operatorUserID\\":\\"4137580800\\",\\"inviterUserID\\":\\"4137580800\\"}],\\"groupOwnerUser\\":{\\"groupID\\":\\"2203019508\\",\\"userID\\":\\"4137580800\\",\\"roleLevel\\":2,\\"joinTime\\":1664365877,\\"nickname\\":\\"123\\",\\"faceURL\\":\\"ic_avatar_06\\",\\"joinSource\\":2,\\"operatorUserID\\":\\"4137580800\\",\\"inviterUserID\\":\\"4137580800\\"}}\"}",false,2,1,1664365877269,1664365877257,"",""),("4f91bd118e41b0315c7f5bf68d6ada55","1cd53d049430909b67a955fbfb8f4ac1","4137580800","2203019508",2,"123","ic_avatar_06",3,100,101,"111",false,2,2,1664365884857,1664365882267,"{\"groupHasReadInfo\":{\"hasReadCount\":0,\"groupMemberCount\":2},\"isPrivateChat\":false,\"hasReadTime\":0,\"notSenderNotificationPush\":false,\"isEncryption\":false,\"inEncryptStatus\":false}","")

```

- superGroupInsertMessage

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| message                                     | string  | LocalChatLog（消息表对象）|对象转换成string
| groupID                                     | string  | 群ID|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sql
 INSERT INTO `local_chat_logs` (`client_msg_id`,`server_msg_id`,`send_id`,`recv_id`,`sender_platform_id`,`sender_nick_name`,`sender_face_url`,`session_type`,`msg_from`,`content_type`,`content`,`is_read`,`status`,`seq`,`send_time`,`create_time`,`attached_info`,`ex`) VALUES ("6edad80249cc0cf626edb88e64f8fb6d","","3045326383","2041671273",1,"Gordon111","ic_avatar_01",1,100,101,"Single chat test3045326383:2041671273:",false,1,0,1663658716992,1663658716992,"","");
```
    


- superGroupGetMultipleMessage

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| msgIDList                                     | string  | 消息ID列表|对象转换成string
| groupID                                     | string  | 群ID|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sql
SELECT * FROM `local_sg_chat_logs_4280368097` WHERE client_msg_id IN ("d9ef1e4e63394b856b8a781ef0234b49","00da0b5471f0bca5a41c92e09419f9dc") ORDER BY send_time DESC;
```

- superGroupUpdateMessageTimeAndStatus

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID                                     | string  | 群ID|
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
 UPDATE `local_sg_chat_logs_4280368097` SET `server_msg_id`="75dee2fbd6c4f28e7895f8410be4984f",`status`=2,`send_time`=1663658950513 WHERE client_msg_id="985261c57242cf647753839854038154" And seq=0;
```


- SuperGroupGetMessageListNoTime

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID                                     | string  | 群ID|
| sessionType | number                                     | 会话类型，单聊1、读扩散群2、大群为3      ||
| count | number | 获取消息的数量 ||
| isReverse | boolean | 消息为正向拉取还是反向拉取|默认情况为false，即为正向拉取（从新消息到老消息），order by 后面的排序规则为send_time DESC 降序排列，当为true的情况，即为反向拉取，order by 后面的排序规则为send_time ASC 升序排列|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | []LocalSuperGroupChatLogs（会话表对象数组数据） ||

**参考sql语句说明：**

```sql
SELECT * FROM `local_sg_chat_logs_812146266` WHERE recv_id = "812146266" AND status <=3 And session_type = 3  ORDER BY send_time DESC LIMIT 30;
-- 注：其中status固定为3
-- 	MsgStatusSending     = 1
-- 	MsgStatusSendSuccess = 2
-- 	MsgStatusSendFailed  = 3
-- 	MsgStatusHasDeleted  = 4
-- 	MsgStatusRevoked     = 5
-- 	MsgStatusFiltered    = 6
```

- SuperGroupGetMessageList

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID                                     | string  | 群ID|
| sessionType | number                                     | 会话类型，单聊1、读扩散群2、大群为3      ||
| count | number | 获取消息的数量 ||
| startTime | number | 消息发送时间，毫秒 ||
| isReverse | boolean | 消息为正向拉取还是反向拉取|默认情况为false，即为正向拉取（从新消息到老消息），order by 后面的排序规则为send_time DESC 降序排列，send_time为 <;当为true的情况，即为反向拉取，order by 后面的排序规则为send_time ASC 升序排列,send_time为 >|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | []LocalSuperGroupChatLogs（会话表对象数组数据） ||

**参考sql语句说明：**

```sql
SELECT * FROM `local_sg_chat_logs_812146266` WHERE  recv_id = "812146266" AND status <=3 And session_type = 3 And send_time < 1664357584025 ORDER BY send_time DESC LIMIT 30;
-- 注：其中status固定为3
```
- superGroupGetNormalMinSeq


| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID | string                                     | 群ID       ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | number                                    | 大群消息表中最小seq ||

**参考sql语句说明：**

```sql
SELECT IFNULL(min(seq),0) FROM `local_sg_chat_logs_812146266` WHERE seq >0;
```












