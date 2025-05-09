# Js实现db接口简要说明(已废弃)

## 表结构以及需要实现的接口说明

> 所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回

### 超级群聊日志表

- 表名：local_sg_chat_logs

```sqlite：
create table local_sg_chat_logs
(
    client_msg_id      char(64)
        primary key,
    server_msg_id      char(64),
    send_id            char(64),
    recv_id            char(64),
    sender_platform_id INTEGER,
    sender_nick_name   varchar(255),
    sender_face_url    varchar(255),
    session_type       INTEGER,
    msg_from           INTEGER,
    content_type       INTEGER,
    content            varchar(1000),
    is_read            numeric,
    status             INTEGER,
    seq                INTEGER default 0,
    send_time          INTEGER,
    create_time        INTEGER,
    attached_info      varchar(1024),
    ex                 varchar(1024)
);
```

#### 接口说明：

- superGroupBatchInsertMessageList

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| LocalChatLog | string |（表对象数据） |数组对象转换成string|
| groupID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
INSERT INTO `local_sg_chat_logs_123456` (`client_msg_id`, `server_msg_id`, `send_id`, `recv_id`, `sender_platform_id`,
                                         `sender_nick_name`, `sender_face_url`, `session_type`, `msg_from`,
                                         `content_type`, `content`, `is_read`, `status`, `seq`, `send_time`,
                                         `create_time`, `attached_info`, `ex`)
VALUES ("llasdaa", "sdfsdfsd", "1231", "1235", 1, "hello", "", 1, 1, 1, "", true, 1, 1, 1666855648, 1666855648, "", "")
```

- superGroupInsertMessage

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| LocalChatLog | string |（表对象数据） |对象转换成string|
| groupID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
INSERT INTO `local_sg_chat_logs_123456` (`client_msg_id`, `server_msg_id`, `send_id`, `recv_id`, `sender_platform_id`,
                                         `sender_nick_name`, `sender_face_url`, `session_type`, `msg_from`,
                                         `content_type`, `content`, `is_read`, `status`, `seq`, `send_time`,
                                         `create_time`, `attached_info`, `ex`)
VALUES ("llasdaa", "sdfsdfsd", "1231", "1235", 1, "hello", "", 1, 1, 1, "", true, 1, 1, 1666855858, 1666855858, "", "")
```

- superGroupDeleteAllMessage

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| groupID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
DELETE
FROM `local_sg_chat_logs_123456`
```

- superGroupSearchMessageByKeyword

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| contentType | []int | | |
| keywordList | []string | | |
| keywordListMatchType | int | | |
| sourceID | string | | |
| startTime | int64 | | |
| endTime | int64 | | |
| sessionType | int | | |
| offset | int | | |
| count | int | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalChatLog（表对象数据） |对象转换成string|| data | errerror | | |

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_sg_chat_logs_`
WHERE recv_id = ""
  And send_time between 1667460883 and 1666856083
  AND status <= 3
  And content_type IN (1, 2)
ORDER BY send_time DESC
LIMIT 1 OFFSET 1
```

- superGroupSearchMessageByContentType

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| contentType | []int | | |
| sourceID | string | | |
| startTime | int64 | | |
| endTime | int64 | | |
| sessionType | int | | |
| offset | int | | |
| count | int | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalChatLog（表对象数据） |对象转换成string|| data | errerror | | |

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_sg_chat_logs_123`
WHERE session_type = 1
  And recv_id == "123"
  And send_time between 1667460977 and 1666856177
  AND status <= 3
  And content_type IN (1, 2)
ORDER BY send_time DESC
LIMIT 2 OFFSET 1
```

- superGroupSearchMessageByContentTypeAndKeyword

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| contentType | []int | | |
| keywordList | []string | | |
| keywordListMatchType | int | | |
| startTime | int64 | | |
| endTime | int64 | | |
| groupID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalChatLog（表对象数据） |对象转换成string|| data | errerror | | |

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_sg_chat_logs_123456`
WHERE send_time between 1667461071 and 1666856271
  AND status <= 3
  And content_type IN (1, 2)
  And (content like '%123%' and content like '%456%')
ORDER BY send_time DESC
```

- superGroupBatchUpdateMessageList

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| LocalChatLog | string |（表对象数据） |对象转换成string|

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
UPDATE `local_sg_chat_logs_`
SET `status`=1,
    `seq`=1
WHERE `client_msg_id` = "llasdaa"
```

- superGroupMessageIfExists

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| ClientMsgID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | bool | | |

**参考sql语句说明：**

```sqlite
SELECT count(*)
FROM `local_chat_logs`
WHERE client_msg_id = "llasdaa"
```

- superGroupIsExistsInErrChatLogBySeq

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| seq | int64 | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | bool | | |

**参考sql语句说明：**

```sqlite
Not used
```

- superGroupMessageIfExistsBySeq

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| seq | int64 | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | bool | | |

**参考sql语句说明：**

```sqlite
SELECT count(*)
FROM `local_chat_logs`
WHERE seq = 1666856491
```

- superGroupGetMessage

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| MsgStruct | string |（表对象数据） |对象转换成string|

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalChatLog（表对象数据） |对象转换成string|

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_sg_chat_logs_123334`
WHERE client_msg_id = "34521"
LIMIT 1
```

- superGroupGetAllUnDeleteMessageSeqList

**无输入参数**

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | number | | |

**参考sql语句说明：**

```sqlite
SELECT `seq`
FROM `local_chat_logs`
WHERE status != 4
```

- superGroupUpdateColumnsMessage

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| ClientMsgID | string | | |
| groupID | string | | |
| args | map[string]interface{} | | |

| 返回参数 | 类型 | 说明 | 备注      |
| --------- |--------| ----- |---------|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错  |
| errMsg | string | 详细的err信息 |         |

**参考sql语句说明：**

```sqlite
UPDATE `local_sg_chat_logs_123456`
SET `attachedInfo`="",
    `clientMsgID`="llasdaa",
    `content`="",
    `contentType`=1.000000,
    `createTime`=1666856725.000000,
    `ex`="",
    `isRead`= true,
    `msgFrom`=1.000000,
    `recvID`="1235",
    `sendID`="1231",
    `sendTime`=1666856725.000000,
    `senderFaceURL`="",
    `senderNickname`="hello",
    `senderPlatformID`=1.000000,
    `seq`=1.000000,
    `serverMsgID`="sdfsdfsd",
    `sessionType`=1.000000,
    `status`=1.000000
WHERE client_msg_id = "llasdaa"
```

- superGroupUpdateMessage

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| LocalChatLog | string |（表对象数据） |对象转换成string|

| 返回参数 | 类型 | 说明 | 备注      |
| --------- |--------| ----- |---------|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错  |
| errMsg | string | 详细的err信息 |         |

**参考sql语句说明：**

```sqlite
UPDATE `local_sg_chat_logs_1235`
SET `server_msg_id`="sdfsdfsd",
    `send_id`="1231",
    `recv_id`="1235",
    `sender_platform_id`=1,
    `sender_nick_name`="hello",
    `session_type`=1,
    `msg_from`=1,
    `content_type`=1,
    `is_read`= true,
    `status`=1,
    `seq`=1,
    `send_time`=1666856770,
    `create_time`=1666856770
WHERE `client_msg_id` = "llasdaa"
```

- superGroupUpdateMessageStatusBySourceID

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| sourceID | string | | |
| status | int32 | | |
| sessionType | int32 | | |

| 返回参数 | 类型 | 说明 | 备注  |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错    |
| errMsg | string | 详细的err信息 |     |

**参考sql语句说明：**

```sqlite
UPDATE `local_sg_chat_logs_547563`
SET `status`=1
WHERE (send_id = "547563" or recv_id = "547563")
  AND session_type = 2
```

- superGroupUpdateMessageTimeAndStatus

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| MsgStruct | string |（表对象数据） |对象转换成string|

| 返回参数 | 类型 | 说明 | 备注  |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错    |
| errMsg | string | 详细的err信息 |     |

**参考sql语句说明：**

```sqlite
UPDATE `local_sg_chat_logs_123334`
SET `server_msg_id`="504093",
    `status`=1,
    `send_time`=1666856957
WHERE client_msg_id = "34521"
  And seq = 0
```

- superGroupGetMessageList

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| sourceID | string | | |
| sessionType | int | | |
| count | int | | |
| startTime | int64 | | |
| isReverse | bool | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalChatLog（表对象数据） |对象转换成string|| data | errerror | | |

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_sg_chat_logs_123`
WHERE recv_id = "123"
  AND status <= 3
  And session_type = 1
  And send_time > 1666252228
ORDER BY send_time ASC
LIMIT 1
```

- superGroupGetMessageListNoTime

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| sourceID | string | | |
| sessionType | int | | |
| count | int | | |
| isReverse | bool | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalChatLog（表对象数据） |对象转换成string|| data | errerror | | |

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_sg_chat_logs_123`
WHERE recv_id = "123"
  AND status <= 3
  And session_type = 1
ORDER BY send_time ASC
LIMIT 1
```

- superGroupGetSendingMessageList

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| groupID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalChatLog（表对象数据） |对象转换成string|| data | errerror | | |

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_sg_chat_logs_123456`
WHERE status = 1
```

- superGroupUpdateGroupMessageHasRead

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| msgIDList | []string | | |
| groupID | string | | |

| 返回参数 | 类型 | 说明 | 备注      |
| --------- |--------| ----- |---------|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错  |
| errMsg | string | 详细的err信息 |         |

**参考sql语句说明：**

```sqlite
UPDATE `local_sg_chat_logs_123625`
SET `is_read`=1
WHERE client_msg_id in ("123")
```

- superGroupGetMultipleMessage

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationIDList | []string | | |
| groupID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalChatLog（表对象数据） |对象转换成string|| data | errerror | | |

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_sg_chat_logs_46422`
WHERE client_msg_id IN ("123")
ORDER BY send_time DESC
```

- superGroupGetNormalMsgSeq

**无输入参数**

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | number | | |

**参考sql语句说明：**

```sqlite
SELECT IFNULL(max(seq), 0)
FROM `local_chat_logs`
```

- superGroupGetNormalMinSeq

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| groupID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | number | | |

**参考sql语句说明：**

```sqlite
SELECT IFNULL(min(seq), 0)
FROM `local_sg_chat_logs_123456`
WHERE seq > 0
```

- superGroupGetTestMessage

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| seq | uint32 | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalChatLog（表对象数据） |对象转换成string|

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_chat_logs`
WHERE seq = 1666857283
```

- superGroupUpdateMsgSenderNickname

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| sendID | string | | |
| nickname | string | | |
| sType | int | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
UPDATE `local_chat_logs`
SET `sender_nick_name`="hello"
WHERE send_id = "14305"
  and session_type = 1
  and sender_nick_name != "hello" 
```

- superGroupUpdateMsgSenderFaceURL

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| sendID | string | | |
| faceURL | string | | |
| sType | int | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
UPDATE `local_chat_logs`
SET `sender_face_url`="hello.png"
WHERE send_id = "14305"
  and session_type = 1
  and sender_face_url != "hello.png"
```

- superGroupUpdateMsgSenderFaceURLAndSenderNickname

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| sendID | string | | |
| faceURL | string | | |
| nickname | string | | |
| sessionType | int | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
UPDATE `local_chat_logs`
SET `sender_face_url`="",
    `sender_nick_name`="hello"
WHERE send_id = "12674"
  and session_type = 1
```

- superGroupGetMsgSeqByClientMsgID

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| clientMsgID | string | | |
| groupID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | number | | |

**参考sql语句说明：**

```sqlite
SELECT seq
FROM `local_sg_chat_logs_243953`
WHERE client_msg_id = "289342"
LIMIT 1
```

- superGroupGetMsgSeqListByGroupID

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| groupID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | number | | |

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_group_members`
WHERE group_id = "1826384574"
```

- superGroupGetMsgSeqListByPeerUserID

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| userID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | number | | |

**参考sql语句说明：**

```sqlite
SELECT `seq`
FROM `local_chat_logs`
WHERE recv_id = "123"
   or send_id = "123"
```

- superGroupGetMsgSeqListBySelfUserID

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| userID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | number | | |

**参考sql语句说明：**

```sqlite
SELECT `seq`
FROM `local_chat_logs`
WHERE recv_id = "1234"
  and send_id = "1234"
```
- superGroupSearchAllMessageByContentType

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID | string | 群ID| |
| contentType    | number                                          | 消息类型 ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | []LocalChatLog（消息表对象数组数据） |数组转换成string|
**参考sql语句说明：**

```sql
SELECT * FROM `local_sg_chat_logs_243953` WHERE content_type = 114

```


