# Js实现db接口简要说明(待更新)

## 表结构以及需要实现的接口说明

> 所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回

### 会话表

- 表名：

```sqlite：
create table local_conversations
(
    conversation_id          char(128)
        primary key,
    conversation_type        INTEGER,
    user_id                  char(64),
    group_id                 char(128),
    show_name                varchar(255),
    face_url                 varchar(255),
    recv_msg_opt             INTEGER,
    unread_count             INTEGER,
    group_at_type            INTEGER,
    latest_msg               varchar(1000),
    latest_msg_send_time     INTEGER,
    draft_text               TEXT,
    draft_text_time          INTEGER,
    is_pinned                numeric,
    is_private_chat          numeric,
    is_not_in_group          numeric,
    update_unread_count_time INTEGER,
    attached_info            varchar(1024),
    ex                       varchar(1024),
    `max_seq` integer,
    `min_seq` integer,
    `has_read_seq` integer,
    `msg_destruct_time` integer DEFAULT 604800,
    `is_msg_destruct` numeric DEFAULT false,
);

create index index_latest_msg_send_time
    on local_conversations (latest_msg_send_time);
```

#### 接口说明：

- getConversationByUserID

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| userID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalConversation（表对象数据） |对象转换成string|

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_conversations`
WHERE user_id = "123"
```

- getAllConversationList

**无输入参数**

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalConversation（表对象数据） |对象转换成string|

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_conversations`
WHERE latest_msg_send_time > 0
ORDER BY case
             when is_pinned = 1 then 0
             else 1 end, max(latest_msg_send_time, draft_text_time) DESC
```

- getHiddenConversationList

**无输入参数**

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalConversation（表对象数据） |对象转换成string|

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_conversations`
WHERE latest_msg_send_time = 0
```

- getAllConversationListToSync

**无输入参数**

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalConversation（表对象数据） |对象转换成string|

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_conversations`
```

- findAllUnreadConversationConversationID

**无输入参数**

| 返回参数 | 类型     | 说明               | 备注     |
| --------- |--------|------------------|--------|
| errCode | number | 自定义即可，0成功，非0失败   |        |
| errMsg | string | 详细的err信息         |        |
| data | string | []ConversationID | 会话ID列表 |

**参考sql语句说明：**

```sqlite
SELECT conversation_id
FROM `local_conversations`
WHERE unread_count > 0
```

+ getAllSingleConversationIDList

| 输入参数 | 类型 | 说明 | 备注 |
| -------- | ---- | ---- | ---- |
|          |      |      |      |

| 返回参数 | 类型     | 说明                                         | 备注 |
| -------- | -------- | -------------------------------------------- | ---- |
| errCode  | number   | 自定义即可，0成功，非0失败                   |      |
| errMsg   | string   | 详细的err信息                                |      |
| data     | string | 所有单聊会话的 conversation_id（会话ID）列表 | 会话id数组 转 String     |

参考 SQL 语句说明：

```
SELECT conversation_id FROM `local_conversations` WHERE conversation_type = 1
```



+ getAllConversationIDList

| 输入参数 | 类型 | 说明 | 备注 |
| -------- | ---- | ---- | ---- |
|          |      |      |      |

| 返回参数 | 类型     | 说明                                         | 备注 |
| -------- | -------- | -------------------------------------------- | ---- |
| errCode  | number   | 自定义即可，0成功，非0失败                   |      |
| errMsg   | string   | 详细的err信息                                |      |
| data     | string | 所有单聊会话的 conversation_id（会话ID）列表 | 单聊会话id数组 转 String     |

参考 SQL 语句说明：

```
SELECT conversation_id FROM `local_conversations`;
```



- getConversationListSplit

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| offset | int | | |
| count | int | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalConversation（表对象数据） |对象转换成string|

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_conversations`
WHERE latest_msg_send_time > 0
ORDER BY case
             when is_pinned = 1 then 0
             else 1 end, max(latest_msg_send_time, draft_text_time) DESC
LIMIT 2 OFFSET 1
```

- batchInsertConversationList

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| LocalConversation | string |（表对象数据） |对象转换成string|

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
INSERT INTO `local_conversations` (`conversation_id`, `conversation_type`, `user_id`, `group_id`, `show_name`,
                                   `face_url`, `recv_msg_opt`, `unread_count`, `group_at_type`, `latest_msg`,
                                   `latest_msg_send_time`, `draft_text`, `draft_text_time`, `is_pinned`,
                                   `is_private_chat`, `is_not_in_group`, `update_unread_count_time`, `attached_info`,
                                   `ex`)
VALUES ("123141", 0, "123", "12", "1213121", "", 0, 0, 0, "", 1666851360, "", 1666851360, true, true, true, 1666851360,
        "", "")
```



- UpdateOrCreateConversations(暂时忽略)

| 输入参数         | 类型                     | 说明             | 备注 |
| ---------------- | ------------------------ | ---------------- | ---- |
| conversationList | []LocalConversation 对象 | 会话列表对象数组 |      |

| 返回参数 | 类型   | 说明            | 备注               |
| -------- | ------ | --------------- | ------------------ |
| errCode  | number | 自定义即可      | 0为成功，非0为失败 |
| errMsg   | string | 详细的 err 信息 |                    |
| data     | string | 无              | 对象转换成string   |



**参考sql语句说明：**

```sql
-- 获取所有 conversation_id
SELECT conversation_id FROM `local_conversations` ;

-- 插入会话列表
INSERT INTO `local_conversations` (`conversation_id`,`session_type`,`source_id`,`target_id`,`unread_count`,`update_time`) VALUES (?,?,?,?,?,?) ON DUPLICATE KEY UPDATE `unread_count`=VALUES(`unread_count`);
```



- insertConversation

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| LocalConversation | string |（表对象数据） |对象转换成string|

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
INSERT INTO `local_conversations` (`conversation_id`, `conversation_type`, `user_id`, `group_id`, `show_name`,
                                   `face_url`, `recv_msg_opt`, `unread_count`, `group_at_type`, `latest_msg`,
                                   `latest_msg_send_time`, `draft_text`, `draft_text_time`, `is_pinned`,
                                   `is_private_chat`, `is_not_in_group`, `update_unread_count_time`, `attached_info`,
                                   `ex`)
VALUES ("123141", 0, "123", "12", "1213121", "", 0, 0, 0, "", 1666851229, "", 1666851229, true, true, true, 1666851229,
        "", "")
```

- deleteConversation

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
DELETE
FROM `local_conversations`
WHERE conversation_id = "123141"
```

- deleteAllConversation

**无输入参数**

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**


```sql
DELETE FROM local_conversations;
```


- getConversation

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalConversation（表对象数据） |对象转换成string|

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_conversations`
WHERE conversation_id = "123141"
LIMIT 1
```

- updateConversation

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| LocalConversation | string |（表对象数据） |对象转换成string|

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----| errCode | number | 自定义即可，0成功，非0失败 |获取不到报错 |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
UPDATE `local_conversations`
SET `user_id`="123",
    `group_id`="12",
    `show_name`="1213121",
    `latest_msg_send_time`=1666851460,
    `draft_text_time`=1666851460,
    `is_pinned`= true,
    `is_private_chat`= true,
    `is_not_in_group`= true,
    `update_unread_count_time`=1666851460
WHERE `conversation_id` = "123141"
```

- updateConversationForSync

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| LocalConversation | string |（表对象数据） |对象转换成string|

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 |获取不到报错 |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
UPDATE `local_conversations`
SET `attached_info`="",
    `ex`="",
    `group_at_type`=0,
    `is_not_in_group`= true,
    `is_pinned`= true,
    `is_private_chat`= true,
    `recv_msg_opt`=0,
    `update_unread_count_time`=1666851499
WHERE conversation_id = "123141"
```

- batchUpdateConversationList

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| LocalConversation | string |（表对象数据） |对象转换成string|

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
UPDATE `local_conversations`
SET `user_id`="123",
    `group_id`="12",
    `show_name`="1213121",
    `latest_msg_send_time`=1666851623,
    `draft_text_time`=1666851623,
    `is_pinned`= true,
    `is_private_chat`= true,
    `is_not_in_group`= true,
    `update_unread_count_time`=1666851623
WHERE `conversation_id` = "123141"
```

- conversationIfExists

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | bool | | |

**参考sql语句说明：**

```sqlite
SELECT count(*)
FROM `local_conversations`
WHERE conversation_id = "123141"
```

- resetConversation

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationID | string | | |

| 返回参数 | 类型 | 说明 | 备注      |
| --------- |--------| ----- |---------|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错  |
| errMsg | string | 详细的err信息 |         |

**参考sql语句说明：**

```sqlite
UPDATE `local_conversations`
SET `unread_count`=0,
    `latest_msg`="",
    `latest_msg_send_time`=0,
    `draft_text`="",
    `draft_text_time`=0
WHERE `conversation_id` = "123141"
```

- resetAllConversation

**无输入参数**

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 |获取不到报错 |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
UPDATE `local_conversations`
SET `unread_count`=0,
    `latest_msg`="",
    `latest_msg_send_time`=0,
    `draft_text`="",
    `draft_text_time`=0
```

- clearConversation

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 |获取不到报错 |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
UPDATE `local_conversations`
SET `unread_count`=0,
    `latest_msg`="",
    `draft_text`="",
    `draft_text_time`=0
WHERE `conversation_id` = "123141"
```

- cleaAllConversation

**无输入参数**

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错|
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
UPDATE `local_conversations`
SET `unread_count`=0,
    `latest_msg`="",
    `draft_text`="",
    `draft_text_time`=0
```

- setConversationDraftDB

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationID | string | | |
| draftText | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 |获取不到报错 |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
update local_conversations
set draft_text="1asd",
    draft_text_time=1666851824615,
    latest_msg_send_time=case when latest_msg_send_time = 0 then 1666851824615 else latest_msg_send_time end
where conversation_id = "123141"
```

- removeConversationDraft

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationID | string | | |
| draftText | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错|
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
UPDATE `local_conversations`
SET `draft_text`="1asd",
    `draft_text_time`=0
WHERE `conversation_id` = "123141"
```

- unPinConversation

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationID | string | | |
| isPinned | int | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错|
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
update local_conversations
set is_pinned=1,
    draft_text_time=case when draft_text = "" then 0 else draft_text_time end
where conversation_id = "123141"
```

- updateColumnsConversation

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationID | string | | |
| args | map[string]interface{} | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |--|
| errCode | number | 自定义即可，0成功，非0失败 | 未更新到一行需要报错 |
| errMsg | string | 详细的err信息 |  |

**参考sql语句说明：**

```sqlite
UPDATE `local_conversations`
SET `attachedInfo`="",
    `conversationID`="123141",
    `conversationType`=0.000000,
    `draftText`="",
    `draftTextTime`=1666851955.000000,
    `ex`="",
    `faceURL`="",
    `groupAtType`=0.000000,
    `groupID`="12",
    `isNotInGroup`= true,
    `isPinned`= true,
    `isPrivateChat`= true,
    `latestMsg`="",
    `latestMsgSendTime`=1666851955.000000,
    `recvMsgOpt`=0.000000,
    `showName`="1213121",
    `unreadCount`=0.000000,
    `updateUnreadCountTime`=1666851955.000000,
    `userID`="123"
WHERE `conversation_id` = "123141"
```

- updateAllConversation

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| LocalConversation | string |（表对象数据） |对象转换成string|

| 返回参数 | 类型 | 说明 | 备注      |
| --------- |--------| ----- |---------|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错  |
| errMsg | string | 详细的err信息 |         |

**参考sql语句说明：**

```sqlite
UPDATE `local_conversations`
SET `user_id`="123",
    `group_id`="12",
    `show_name`="1213121",
    `latest_msg_send_time`=1666852402,
    `draft_text_time`=1666852402,
    `is_pinned`= true,
    `is_private_chat`= true,
    `is_not_in_group`= true,
    `update_unread_count_time`=1666852402
```

- incrConversationUnreadCount

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationID | string | | |

| 返回参数 | 类型 | 说明 | 备注  |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错    |
| errMsg | string | 详细的err信息 |     |

**参考sql语句说明：**

```sqlite
UPDATE `local_conversations`
SET `unread_count`=unread_count + 1
WHERE `conversation_id` = "123141"
```

- getTotalUnreadMsgCount

**无输入参数**

| 返回参数    | 类型 | 说明 | 备注 |
|---------|--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg  | string | 详细的err信息 | |
| data    | number | | |

**参考sql语句说明：**

```sqlite
SELECT `unread_count`
FROM `local_conversations`
WHERE recv_msg_opt < 2 and latest_msg_send_time > 0
```

- setMultipleConversationRecvMsgOpt

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationIDList | []string | | |
| opt | int | | |

| 返回参数 | 类型 | 说明 | 备注  |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错    |
| errMsg | string | 详细的err信息 |     |
| data | errerror | |     |

**参考sql语句说明：**

```sqlite
UPDATE `local_conversations`
SET `recv_msg_opt`=1
WHERE conversation_id IN ("123141")
```

- getMultipleConversation

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationIDList | []string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalConversation（表对象数据） |对象转换成string|| data | errerror | | |

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_conversations`
WHERE conversation_id IN ("123141")
```

- decrConversationUnreadCount

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| conversationID | string | | |
| count | int64 | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 |获取不到报错 |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_conversations`
WHERE conversation_id = "123141"
  AND `local_conversations`.`conversation_id` = "123141"
LIMIT 1
```

- getAllConversations

**无输入参数**

| 返回参数 | 类型 | 说明                         | 备注 |
| --------- |--------|----------------------------|-----|
| errCode | number | 自定义即可，0成功，非0失败             |获取不到报错 |
| errMsg | string | 详细的err信息                   | |
| data | string | []LocalConversation（表对象数据） |对象转换成string|


**参考sql语句说明：**

```sqlite
select * from local_conversations;
```






