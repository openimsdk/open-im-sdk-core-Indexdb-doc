# Js实现db接口简要说明(待更新)

## 表结构以及需要实现的接口说明

> 所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回

### 发送消息临时缓存表

- 表名：local_sending_messages

```sqlite
create table local_sending_messages
(
    conversation_id varchar(128),
    client_msg_id   varchar(64),
    ex              varchar(1024),

    PRIMARY KEY ("conversation_id", "client_msg_id")
);
```

#### 接口说明：

- insertSendingMessage

| 输入参数     | 类型     | 说明 | 备注       |
| --------- |--------| ----- |----------|
|message   | string | （表对象数据） |对象转换成string|

| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |     |
| errMsg     | string     | 详细的err信息 |     |

**参考sql语句说明：**

```sqlite
INSERT INTO `local_sending_messages` (`conversation_id`, `client_msg_id`, `ex`)
VALUES ("123", "123", "")
```

- deleteSendingMessage




| 输入参数     | 类型     | 说明 | 备注       |
| --------- |--------| ----- |----------|
|conversationID| string | 会话ID |     |
|clientMsgID| string | 消息ID |     |




| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |     |
| errMsg     | string     | 详细的err信息 |     |

```sqlite
DELETE
FROM `local_sending_messages`
WHERE conversation_id = "457"
  and client_msg_id = "123"
```


- getAllSendingMessages

**无输入参数**


| 返回参数    | 类型     | 说明                                | 备注  |
|---------|--------|-----------------------------------|-----|
| errCode | number | 自定义即可，0成功，非0失败                    |     |
| errMsg  | string | 详细的err信息                          |     |
| data    | string | []LocalSendingMessages  （表对象数组数据） |对象转换成string|

```sqlite
SELECT *
FROM `local_sending_messages`
```

