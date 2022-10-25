# Js实现db接口简要说明(待更新)
## 表结构以及需要实现的接口说明
>所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回
### 未读消息表
- 表名：local_conversation_unread_messages
```sqlite
CREATE TABLE `local_conversation_unread_messages` (`conversation_id` char(128),`client_msg_id` char(64),`send_time` integer,`ex` varchar(1024),PRIMARY KEY (`conversation_id`,`client_msg_id`));
```
#### 接口说明：
- batchInsertConversationUnreadMessageList

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| messageList                                     | string  | []LocalConversationUnreadMessage（未读消息表对象数组数据）|对象数组转换成string

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sql
INSERT INTO `local_conversation_unread_messages` (`conversation_id`,`client_msg_id`,`send_time`,`ex`) VALUES ("single_2992126880","c77b0374e6b2c86067a73ee9cef77c46",1665225331858,"");
```

- deleteConversationUnreadMessageList

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| conversationID      | string                                          | 会话ID ||
| sendTime      | number                                          | 消息发送时间，毫秒  ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | number                                    | 删除影响的条数 ||

```sql
DELETE FROM `local_conversation_unread_messages` WHERE conversation_id = "single_2992126880" and send_time <= 1665225362597;
```

