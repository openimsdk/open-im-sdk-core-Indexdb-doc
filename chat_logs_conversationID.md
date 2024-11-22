# Js实现db接口简要说明(待更新)
## 表结构以及需要实现的接口说明
>所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回
### 消息表(写扩散消息表)
###  读扩散消息表
- 表名：chat_logs_si_7788_7789
>注：读扩散表为动态生成，表名也是，规则为chat_logs_+conversationID，
原有代码中，是在 `GetMessage`、`GetMessageBySeq`、`BatchInsertMessageList`、`GetConversationNormalMsgSeq`、`GetLatestActiveMessage` 和 `GetMessageListNoTime` 这六个函数中进行判断，如果没有就动态生成该表

```sqlite
CREATE TABLE `chat_logs_si_7788_7789` (
  `client_msg_id` char(32),
  `server_msg_id` char(32),
  `send_id` char(32),
  `recv_id` char(32),
  `sender_platform_id` smallint,
  `sender_nick_name` varchar(255),
  `sender_face_url` varchar(255),
  `session_type` smallint,
  `msg_from` smallint,
  `content_type` smallint,
  `content` varchar(1000),
  `is_read` tinyint(1),
  `status` smallint,
  `seq` int DEFAULT 0,
  `send_time` int,
  `create_time` int,
  `attached_info` varchar(1024),
  `ex` varchar(1024),
  `local_ex` varchar(1024),
  `is_react` tinyint(1),
  `is_external_extensions` tinyint(1),
  `msg_first_modify_time` int,
  PRIMARY KEY (`client_msg_id`)
);
```

- 表结构特别说明

  | 字段名     | 类型                                                         | 说明 |
    | --------- | ------------------------------------------------------------ | ----- |
  | is_read      | bool                                         | true已读，false 未读|



#### 接口说明：
- getMessage

| 输入参数     | 类型                                                         | 说明   |备注|
| --------- | ------------------------------------------------------------ |------|-----------------------|
| conversationID      | string                                          | 会话ID ||
| clientMsgID      | string                                          | 消息ID ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果获取不到消息也需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | LocalChatLog（消息表对象数据） |对象转换成string|

**参考sql语句说明：**

```sql
SELECT * FROM `chat_logs_si_7788_7789` WHERE client_msg_id = "063031b86f8e503c6038efb2b835f216" LIMIT 1
```
- getAlreadyExistSeqList

| 参数名称       | 类型     | 说明                       | 备注 |
| -------------- |--------|--------------------------| ---- |
| conversationID | string | 会话 ID                    |      |
| lostSeqList    | string | 丢失的序列号列表,为整型数组转换后的string |      |

#### 输出参数

| 参数名称 | 参数类型   | 参数说明                                  | 备注 |
| -------- |--------|---------------------------------------| ---- |
| errCode  | number | 自定义即可，0 表示成功，非 0 表示失败                 |      |
| errMsg   | string | 详细的错误信息                               |      |
| data     | string | 已经存在的序列号列表 整型数组转换后的string，如果没有则返回空字符串 |      |

```sql
SELECT seq FROM `chat_logs_si_7788_7789` WHERE seq IN (1,2,3,4);
```


- getMessageList

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| conversationID | string | 会话 ID                    |      |
| count | number | 获取消息的数量 ||
| startTime | number | 消息发送时间，毫秒 ||
| isReverse | boolean | 消息为正向拉取还是反向拉取|默认情况为false，即为正向拉取（从新消息到老消息），order by 后面的排序规则为send_time DESC 降序排列，send_time为 <;当为true的情况，即为反向拉取，order by 后面的排序规则为send_time ASC 升序排列,send_time为 >|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | []LocalChatLog（消息表对象数组数据） |对象转换成string|

**参考sql语句说明：**

```sql
#如果startTime>0
SELECT * FROM `chat_logs_si_7788_7789` WHERE send_time < 1664357584025 ORDER BY send_time DESC LIMIT 30;
#否则
SELECT * FROM `chat_logs_si_7788_7789`  ORDER BY send_time DESC LIMIT 30;
```



- getMessageBySeq

| 输入参数 | 类型   | 说明     | 备注 |
| -------- | ------ | -------- | ---- |
| conversationID | string | 会话 ID                    |      |
| seq      | number | 消息序列 |      |

| 返回参数 | 类型   | 说明                           | 备注                           |
| -------- | ------ | ------------------------------ | ------------------------------ |
| errCode  | number | 自定义即可，0成功，非0失败     | 如果获取不到消息也需要返回错误 |
| errMsg   | string | 详细的err信息                  |                                |
| data     | string | LocalChatLog（消息表对象数据） | 对象转换成string               |

参考 SQL 语句说明：

```sql
SELECT * FROM `chat_logs_si_7788_7789` WHERE seq = 1000 LIMIT 1;
```

- getMessageByUserID

| 输入参数           | 类型     | 说明    | 备注 |
|----------------|--------|-------| ---- |
| conversationID | string | 会话 ID |      |
| userID         | string | 用户ID  |      |

| 返回参数 | 类型   | 说明                           | 备注                           |
| -------- | ------ | ------------------------------ | ------------------------------ |
| errCode  | number | 自定义即可，0成功，非0失败     | 如果获取不到消息也需要返回错误 |
| errMsg   | string | 详细的err信息                  |                                |
| data     | string | []LocalChatLog（消息表对象数组数据）| 对象转换成string               |

参考 SQL 语句说明：

```sql
SELECT * FROM `chat_logs_si_7788_7789` WHERE user_id = "1552662" LIMIT 1;
```

- getMessagesByClientMsgIDs

| 输入参数       | 类型     | 说明    | 备注 |
| -------------- | -------- | ------- | ---- |
| conversationID | string   | 会话 ID |      |
| msgIDs         | string | 消息 ID | 消息id数组 转为String     |

| 返回参数 | 类型     | 说明                    | 备注                           |
| -------- |--------|-----------------------| ------------------------------ |
| errCode  | number | 自定义即可，0成功，非0失败        | 如果获取不到消息也需要返回错误 |
| errMsg   | string | 详细的err信息              |                                |
| data     | string | LocalChatLog（消息表对象数组） | 对象转换成 string              |

**参考sql语句说明：**

```
SELECT * FROM `chat_logs_si_7788_7789` WHERE client_msg_id IN ("063031b86f8e503c6038efb2b835f216","063031b86f8e503c6038efb2b835f217") ORDER BY send_time DESC
```



- getMessagesBySeqs

| 输入参数 | 类型     | 说明           | 备注 |
| -------- | -------- | -------------- | ---- |
| conversationID | string   | 会话 ID |      |
| seqs     | number[] | 消息序列号数组 |      |

| 返回参数 | 类型   | 说明                                 | 备注                                   |
| -------- | ------ | ------------------------------------ | -------------------------------------- |
| errCode  | number | 自定义即可，0成功，非0失败           | 获取不到的时候返回空数组不需要返回错误 |
| errMsg   | string | 详细的err信息                        |                                        |
| data     | string | []LocalChatLog（消息表对象数组数据） | 对象转换成string                       |

参考 SQL 语句说明：

```
SELECT * FROM `chat_logs_si_7788_7789` WHERE seq IN (1,2,3,4) ORDER BY send_time DESC;
```





- getConversationNormalMsgSeq

| 输入参数           | 类型                                                         | 说明                  |备注|
|----------------| ------------------------------------------------------------ |---------------------|-----------------------|
| conversationID | string  | 会话ID                |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | number                                    | 消息表中最大seq ||

**参考sql语句说明：**

```sql
SELECT IFNULL(max(seq),0) FROM `local_chat_logs`;
```

- CheckConversationNormalMsgSeq

| 输入参数           | 类型                                                         | 说明                  |备注|
|----------------| ------------------------------------------------------------ |---------------------|-----------------------|
| conversationID | string  | 会话ID                |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | number                                    | 消息表中最大seq ||

**参考sql语句说明：**

```sql
SELECT IFNULL(max(seq),0) FROM `local_chat_logs`;
```

- getConversationPeerNormalMsgSeq

| 输入参数       | 类型   | 说明 | 备注 |
| -------------- | ------ |--| ---- |
| conversationID | string | 会话ID |      |
| loginUserID | string | 用户ID |      |


| 返回参数 | 类型   | 说明              | 备注                           |
| -------- | ------ |-----------------| ------------------------------ |
| errCode  | number | 自定义即可，0成功，非0失败  | 如果获取不到消息也需要返回错误 |
| errMsg   | string | 详细的err信息        |                                |
| data     | number | int64（对方最大的seq） |                                |

**参考 sql 语句说明：**

```
SELECT IFNULL(MAX(seq), 0) FROM `chat_logs_si_7788_7789` WHERE  send_id != "7788";
```



[//]: # (- GetConversationAbnormalMsgSeq)
- 
- getSendingMessageList(暂时没用)

**无输入参数**

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          |  []LocalChatLog（消息表对象数组数据） |对象数组转换成string|

**参考sql语句说明：**

```sql
select * from chat_logs_si_7788_7789 where status = 1;
-- 消息状态：
-- 	MsgStatusSending     = 1
-- 	MsgStatusSendSuccess = 2
-- 	MsgStatusSendFailed  = 3
-- 	MsgStatusHasDeleted  = 4
-- 	MsgStatusRevoked     = 5
-- 	MsgStatusFiltered    = 6
```





- updateMessageTimeAndStatus

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| conversationID | string | 会话ID |      |
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
 UPDATE `chat_logs_si_7788_7789` SET `server_msg_id`="75dee2fbd6c4f28e7895f8410be4984f",`status`=2,`send_time`=1663658950513 WHERE client_msg_id="985261c57242cf647753839854038154" And seq=0;
```



- updateMessage

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| conversationID | string | 会话ID |      |
| clientMsgID | string                                     | 客户端消息ID       ||
| args        | object | 更新字段参数对象 |内部是kv，k为字段名，v为需要更新的字段内容|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果没更新到任何一行消息也返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sql
 UPDATE `chat_logs_si_7788_7789` SET `server_msg_id`="8c6dc2ace8ff5706880018de43916c39",`recv_id`="2041671273",`session_type`=1,`status`=2,`seq`=14 WHERE `client_msg_id` = "6edad80249cc0cf626edb88e64f8fb6d";
```

- updateMessageBySeq

| 输入参数           | 类型     | 说明       |备注|
|----------------|--------|----------|-----------------------|
| conversationID | string | 会话ID     |      |
| seq            | number | 消息的序列号   ||
| args           | object | 更新字段参数对象 |内部是kv，k为字段名，v为需要更新的字段内容|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果没更新到任何一行消息也返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sql
 UPDATE `chat_logs_si_7788_7789` SET `server_msg_id`="8c6dc2ace8ff5706880018de43916c39",`recv_id`="2041671273",`session_type`=1,`status`=2 WHERE `seq` = 5;
```

- batchInsertMessageList

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| conversationID | string | 会话ID |      |
| messageList                                     | string  | []LocalChatLog（消息表对象数组数据）|对象数组转换成string

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sql
 INSERT INTO `chat_logs_si_7788_7789` (`client_msg_id`,`server_msg_id`,`send_id`,`recv_id`,`sender_platform_id`,`sender_nick_name`,`sender_face_url`,`session_type`,`msg_from`,`content_type`,`content`,`is_read`,`status`,`seq`,`send_time`,`create_time`,`attached_info`,`ex`) VALUES ("01d22adafe309482391fc54f728c96a7","5cb3ecb6092843e9376fd229a72b24e0","3045326383","openIMAdmin",0,"","",1,200,1303,"{\"detail\":\"CgozMDQ1MzI2Mzgz\",\"defaultTips\":\"remove a blocked user\",\"jsonDetail\":\"{\\"userID\\":\\"3045326383\\"}\"}",false,6,1,1663557189653,1663557189652,"",""),("3492347bc55280d3c5c32398f78eae50","2b0a01eff07f502da31751fb459966bc","3045326383","openIMAdmin",0,"","",1,200,1303,"{\"detail\":\"CgozMDQ1MzI2Mzgz\",\"defaultTips\":\"remove a blocked user\",\"jsonDetail\":\"{\\"userID\\":\\"3045326383\\"}\"}",false,6,2,1663557324330,1663557324329,"","");
```

- insertMessage

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| conversationID | string | 会话ID |      |
| message                                     | string  | LocalChatLog（消息表对象）|对象转换成string

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sql
 INSERT INTO `chat_logs_si_7788_7789` (`client_msg_id`,`server_msg_id`,`send_id`,`recv_id`,`sender_platform_id`,`sender_nick_name`,`sender_face_url`,`session_type`,`msg_from`,`content_type`,`content`,`is_read`,`status`,`seq`,`send_time`,`create_time`,`attached_info`,`ex`) VALUES ("6edad80249cc0cf626edb88e64f8fb6d","","3045326383","2041671273",1,"Gordon111","ic_avatar_01",1,100,101,"Single chat test3045326383:2041671273:",false,1,0,1663658716992,1663658716992,"","");
```


- getMultipleMessage（暂未使用）

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| msgIDList                                     | string  | 消息ID列表|对象转换成string

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sql
SELECT * FROM `local_sg_chat_logs_4280368097` WHERE client_msg_id IN ("d9ef1e4e63394b856b8a781ef0234b49","00da0b5471f0bca5a41c92e09419f9dc") ORDER BY send_time DESC;
```

- searchMessageByKeyword

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| conversationID | string | 会话ID |      |
| contentType                                     | string | 消息类型列表，为整型数组转换后的string| 
| senderUserIDList |string| 发送者用户ID列表，为字符串数组转换后的string|
| keywordList | string | 关键字列表，为字符串数组转换后的string |
| keywordListMatchType | 0为or匹配, 1为and匹配 |  |
| startTime | number | 开始时间戳 |
| endTime | number | 结束时间戳 |
| offset | number | 偏移数 | 
| count | number | 获取总数 |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | []LocalChatLog（消息表对象数组数据）转换后的string ||

**参考sql语句说明：**

```sql
 SELECT * FROM `chat_logs_si_7788_7789` WHERE  send_time  between 0 and 1666766907000 AND status <=3  And content_type IN (101,106) And (content like '%1%') And send_id IN ("123123")  ORDER BY send_time DESC LIMIT 20 OFFSET 0;
```



- searchMessageByContentType

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| conversationID | string | 会话ID |      |
| contentType                                     | string | 消息类型列表，为整型数组转换后的string|
| senderUserIDList |string| 发送者用户ID列表，为字符串数组转换后的string|
| startTime | number | 开始时间戳 |
| endTime | number | 结束时间戳 |
| offset | number | 偏移数 | 
| count | number | 获取总数 |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | []LocalChatLog（消息表对象数组数据）转换后的string ||

**参考sql语句说明：**

```sql
SELECT * FROM `chat_logs_sg_3123123637673` WHERE send_time between 0 AND 1730271722000 AND status <= 3 AND content_type IN (105) And send_id IN ("91750") ORDER BY send_time DESC LIMIT 20
```




- searchMessageByContentTypeAndKeyword

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| conversationID | string | 会话ID |      |
| contentType                                        | string | 消息类型列表，为整型数组转换后的string|
| senderUserIDList |string|发送者用户ID列表，为字符串数组转换后的string|
| keywordList | string | 关键字列表，为字符串数组转换后的string |
| keywordListMatchType | 0为or匹配, 1为and匹配 |  |
| startTime | number | 开始时间戳 |
| endTime | number | 结束时间戳 |


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | []LocalChatLog（消息表对象数组数据）转换后的string ||

**参考sql语句说明：**

```sql
SELECT * FROM `chat_logs_si_7788_7789` WHERE send_time between 0 and 1666769211000 AND status <=3  And content_type IN (101,106) And (content like '%1%' or content liken '%2%') AND send_id IN ("3377881") ORDER BY send_time DESC
```

- messageIfExists（暂未使用）

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| clientMsgID                                     | string  | id| 

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 |
| data      | bool                                          | 是否存在 |

**参考sql语句说明：**

```sql
SELECT count(*) FROM `chat_logs_si_7788_7789` WHERE client_msg_id == "xxx";
```

- isExistsInErrChatLogBySeq（暂未使用）

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| seq                                     | int  |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 |
| data      | bool                                          | 是否存在 |

**参考sql语句说明：**

```sql
SELECT count(*) FROM `local_err_chat_logs` WHERE seq == 1;
```

- MessageIfExistsBySeq（暂未使用）

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| seq                                     | uint32  | id| 

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 |
| data      | bool                                          | 是否存在 |

**参考sql语句说明：**

```sql
SELECT count(*) FROM `chat_logs_si_7788_7789` WHERE seq == 1;
```

- UpdateGroupMessageHasRead（暂未使用）
| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| msgIDList                                     | string | 消息ID列表转换后的string |
|sessionType | int | 不能为3 |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 |
| data      | string                                          | 可为"" |


```sql
UPDATE `chat_logs_si_7788_7789` SET `is_read`=1 WHERE session_type=2 AND client_msg_id in ("a43fe26849cf4f9225262297967979f1")    
```


- getMultipleMessage（暂未使用）

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| msgIDList                                     | []string  | id| 

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 |
| data      | string                                          |  |

**参考sql语句说明：**
```sql
SELECT * FROM `chat_logs_si_7788_7789` WHERE client_msg_id IN ("a43fe26849cf4f9225262297967979f1") ORDER BY send_time DESC
```

- updateMsgSenderNickname（暂未使用）

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| sendID                                     | string  |  |
| nickname | string |  |
| sType | int | sessionType


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 |
| data      | string                                          | 可为"" |

**参考sql语句说明：**
```sql
UPDATE `chat_logs_si_7788_7789` SET `sender_nick_name`="xx" WHERE send_id = "ss" and session_type = 1 and sender_nick_name != "xx"
```

- updateMsgSenderFaceURL（暂未使用）

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| sendID                                     | string  |  |
| faceURL | string |  |
| sType | int | sessionType


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 |
| data      | string                                          | 可为"" |

**参考sql语句说明：**
```sql
UPDATE `chat_logs_si_7788_7789` SET `sender_face_url`="xx" WHERE send_id = "ss" and session_type = 1 and sender_face_url != "xx"
```

- updateMsgSenderFaceURLAndSenderNickname

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| conversationID | string | 会话ID |      |
| sendID                                     | string  |  |
| faceURL | string |  |
| nickname | string |  |


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 |
| data      | string                                          | 可为"" |

**参考sql语句说明：**
```sql
UPDATE `chat_logs_si_7788_7789` SET `sender_face_url`="xx",`sender_nick_name`="" WHERE send_id = "ss"
```


- getMsgSeqByClientMsgID（暂未使用）

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| clientMsgID                                     | string  | id| 

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 | 获取不到报错|
| errMsg     | string                                          | 详细的err信息 |
| data      | uint32                                          | uint32 |

**参考sql语句说明：**

```sql
SELECT `seq` FROM `chat_logs_si_7788_7789` WHERE client_msg_id="ss"  LIMIT 1
```

- getMsgSeqListByGroupID（暂未使用）

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID                                     | string  | id| 

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 |
| data      | []uint32                                          |  |

**参考sql语句说明：**

```sql
SELECT `seq` FROM `chat_logs_si_7788_7789` WHERE recv_id="ss"
```


- getMsgSeqListByPeerUserID（暂未使用）

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| userID                                     | string  | id| 

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 |
| data      | []uint32                                          |  |

**参考sql语句说明：**

```sql
SELECT `seq` FROM `chat_logs_si_7788_7789` WHERE recv_id="ss" or send_id="ss"
```

- getMsgSeqListBySelfUserID（暂未使用）

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| userID                                     | string  | id| 

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 |
| data      | []uint32                                          |  |

**参考sql语句说明：**

```sql
    SELECT `seq` FROM `chat_logs_si_7788_7789` WHERE recv_id="ss" and send_id="ss"
```




- deleteAllMessage（暂未使用）

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 |
| data      | []uint32                                          |  没有返回空列表|

```sql
UPDATE `chat_logs_si_7788_7789` SET `content`="",`status`=4
```


- getAllUnDeleteMessageSeqList（暂未使用）

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 |
| data      | []uint32                                          |  没有返回空列表|

```sql
SELECT `seq` FROM `chat_logs_si_7788_7789` WHERE status != 4
```



- deleteConversationAllMessages

| 输入参数       | 类型   | 说明   | 备注 |
| -------------- | ------ | ------ | ---- |
| conversationID | string | 会话ID |      |

| 返回参数 | 类型   | 说明                       | 备注 |
| -------- | ------ | -------------------------- | ---- |
| errCode  | number | 自定义即可，0成功，非0失败 |      |
| errMsg   | string | 详细的err信息              |      |
| data     | string | 无                         |      |

**参考sql语句说明：**

```
DELETE FROM `chat_logs_si_7788_7789` WHERE 1=1;
```



+ markDeleteConversationAllMessages

| 输入参数       | 类型   | 说明   | 备注 |
| -------------- | ------ | ------ | ---- |
| conversationID | string | 会话ID |      |

| 返回参数 | 类型   | 说明                       | 备注                     |
| -------- | ------ | -------------------------- | ------------------------ |
| errCode  | number | 自定义即可，0成功，非0失败 | 如果删除失败需要返回错误 |
| errMsg   | string | 详细的err信息              |                          |

**参考sql语句说明：**

```
UPDATE `chat_logs_si_7788_7789` SET `status` = 2 WHERE (1 = 1) AND (`conversation_id` = 'conversationID');
```



+ getUnreadMessage

| 输入参数           | 类型   | 说明 | 备注 |
|----------------| ------ |--| ---- |
| conversationID | string | 会话ID |      |
| loginUserID        | string | 用户ID |      |


| 返回参数    | 类型   | 说明                                  | 备注           |
|---------| ------ |-------------------------------------|--------------|
| errCode | number | 自定义即可，0成功，非0失败                      | 如果找不到不需要返回错误 |
| errMsg  | string | 详细的err信息                            |              |
| data    | string | []LocalChatLog（消息表对象数组数据）转换后的string |              |

**参考sql语句说明：**

```
select * from chat_logs_si_7788_7789 where send_id  != "7788" And is_read = 0;
```



+ markConversationMessageAsReadBySeqs

| 输入参数           | 类型   | 说明 | 备注 |
|----------------| ------ |--| ---- |
| conversationID | string | 会话ID |      |
| seqs  | string | 整型数组转换后的string |      |
| loginUserID        | string | 用户ID |      |


| 返回参数    | 类型     | 说明                | 备注           |
|---------|--------|-------------------|--------------|
| errCode | number | 自定义即可，0成功，非0失败    |  |
| errMsg  | string | 详细的err信息          |              |
| data    | number | 更新影响的行数 |              |

**参考sql语句说明：**

```
UPDATE `chat_logs_si_7788_7789` SET `is_read`=1 WHERE `seq` IN (1,2) And send_id != "7788";
```




+ markConversationMessageAsReadDB

| 输入参数           | 类型   | 说明 | 备注 |
|----------------| ------ |--| ---- |
| conversationID | string | 会话ID |      |
| msgIDs  | string | 消息ID字符串数组转换后的string |      |
| loginUserID        | string | 用户ID |      |


| 返回参数    | 类型     | 说明                | 备注           |
|---------|--------|-------------------|--------------|
| errCode | number | 自定义即可，0成功，非0失败    |  |
| errMsg  | string | 详细的err信息          |              |
| data    | number | 更新影响的行数 |              |

**参考sql语句说明：**

```
UPDATE `chat_logs_si_7788_7789` SET `is_read`=1 WHERE `client_msg_id` IN ("34343434","234234324234") And send_id != "7788";
```



+ updateColumnsMessage

| 输入参数           | 类型   | 说明 | 备注 |
|----------------| ------ |--| ---- |
| conversationID | string | 会话ID |      |
| clientMsgID  | string | 消息ID |      |
| args        | string | map转换成的string |      |


| 返回参数    | 类型     | 说明               | 备注           |
|---------|--------|------------------|--------------|
| errCode | number | 自定义即可，0成功，非0失败   |  |
| errMsg  | string | 详细的err信息         |              |
| data    | string | |              |

**参考sql语句说明：**

```
UPDATE `chat_logs_si_7788_7789` SET `attached_info`="24234" WHERE `client_msg_id` = '2342342343';

```



+ deleteConversationMsgs

| 输入参数       | 类型     | 说明                                | 备注 |
| -------------- |--------|-----------------------------------| ---- |
| conversationID | string | 会话ID                              |      |
| msgIDs         | string | 待删除消息的 clientMsgID 字符串数组转换的string |      |

| 返回参数 | 类型   | 说明                       | 备注 |
| -------- | ------ | -------------------------- | ---- |
| errCode  | number | 自定义即可，0成功，非0失败 |      |
| errMsg   | string | 详细的err信息              |      |
| data     |        |                            |      |

**参考sql语句说明：**

```
DELETE FROM `chat_logs_si_7788_7789` WHERE client_msg_id IN ('063031b86f8e503c6038efb2b835f216', '063031b86f8e503c6038efb2b835f217', '063031b86f8e503c6038efb2b835f218');
```




- updateSingleMessageHasRead(暂未使用)

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| sendID | string | |
| msgIDList | []string | |


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 |
| data      | []uint32                                          |  没有返回空列表|

```sql
UPDATE `chat_logs_si_7788_7789` SET `is_read`=1 WHERE send_id="s"  AND session_type=1 AND client_msg_id in ("sss")
```



- updateGroupMessageHasRead(暂未使用)

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| sessionType | number | |
| msgIDList | []string | |


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 |
| data      | number                                          |  |

```sql
UPDATE `chat_logs_si_7788_7789` SET `is_read`=1 WHERE session_type=3 AND client_msg_id in ("12","ds")
```

- updateMessageStatusBySourceID(暂未使用)

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| sourceID                                     | string  | 关于某人的ID也可能是写扩散模式下群ID|
| status | number |消息状态值 |
| sessionType | number                                     | 会话类型，单聊1、读扩散群2、大群为3      ||
| loginUserID | string | 用户登录ID |需要根据会话的类型和sourceID判断，当sessionType为1并且sourceID为登录者ID时候，sql为 AND|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果没更新到任何一行消息也返回错误|
| errMsg     | string                                          | 详细的err信息 |
| data      | number                                          | 可为"" |

```sql
-- 1、sessionType == 1 && sourceID == d.loginUserID
UPDATE `chat_logs_si_7788_7789` SET `status`=4, WHERE session_type=1 AND send_id= "ss" AND recv_id="ss"
-- 2、
UPDATE `chat_logs_si_7788_7789` SET `status`=4, WHERE session_type=1 AND (send_id= "ss" or recv_id="ss")
```



+ MarkConversationAllMessageAsRead

| 输入参数           | 类型   | 说明 | 备注 |
|----------------| ------ |--| ---- |
| conversationID | string | 会话ID |      |
| loginUserID        | string | 用户ID |      |


| 返回参数    | 类型     | 说明                | 备注           |
|---------|--------|-------------------|--------------|
| errCode | number | 自定义即可，0成功，非0失败    |  |
| errMsg  | string | 详细的err信息          |              |
| data    | number | 更新影响的行数 |              |

**参考sql语句说明：**

```
UPDATE `chat_logs_si_7788_7789` SET `is_read`=1 WHERE `is_read`=0 And send_id != "7788";
```



+ deleteConversationMsgsBySeqs（暂未使用）

| 输入参数       | 类型     | 说明                                                 | 备注 |
| -------------- | -------- | ---------------------------------------------------- | ---- |
| conversationID | string   | 会话ID                                               |      |
| seqs           | []number | 需要删除的消息序列号，可以传入多个序列号，以数组形式 |      |

| 返回参数 | 类型   | 说明                       | 备注 |
| -------- | ------ | -------------------------- | ---- |
| errCode  | number | 自定义即可，0成功，非0失败 |      |
| errMsg   | string | 详细的err信息              |      |
| data     |        |                            |      |

**参考 sql 语句说明：**

```sql
DELETE FROM `chat_logs_si_7788_7789` WHERE seq IN (1, 2, 3);
```



- searchAllMessageByContentType

| 输入参数           | 类型     | 说明   |备注|
|----------------|--------|------|-----------------------|
| conversationID | string | 会话ID ||
| contentType    | number | 消息类型 ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | []LocalChatLog（消息表对象数组数据） |数组转换成string|
**参考sql语句说明：**

```sql
SELECT * FROM `chat_logs_si_7788_7789` WHERE content_type = 114

```


- getLatestActiveMessage

| 输入参数           | 类型                                                         | 说明                  |备注|
|----------------| ------------------------------------------------------------ |---------------------|-----------------------|
| conversationID | string  | 会话ID                |
| isReverse      | boolean | 消息为正向拉取还是反向拉取       |默认情况为false，即为正向拉取（从新消息到老消息），order by 后面的排序规则为send_time DESC 降序排列，当为true的情况，即为反向拉取，order by 后面的排序规则为send_time ASC 升序排列|


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | []LocalChatLog（消息表对象数组数据） |对象转换成string|

**参考sql语句说明：**

```sql
SELECT * FROM `chat_logs_si_7788_7789`
WHERE `status` < 4
ORDER BY `send_time` DESC
LIMIT 1;

```