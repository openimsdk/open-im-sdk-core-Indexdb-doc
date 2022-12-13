# Js实现db接口简要说明(待更新)
## 表结构以及需要实现的接口说明
>所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回
### 消息扩展信息表
- 表名：local_chat_log_reaction_extensions
```sqlite
CREATE TABLE `local_chat_log_reaction_extensions` (`client_msg_id` char(64),`local_reaction_extensions` blob,PRIMARY KEY (`client_msg_id`))
```

- 表结构特别说明





#### 接口说明：
- getMessageReactionExtension

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| clientMsgID      | string                                          | 消息ID ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果获取不到消息也需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | local_chat_log_reaction_extensions（消息扩展表对象） |对象转换成string|
**参考sql语句说明：**

```sql
SELECT * FROM `local_chat_log_reaction_extensions` WHERE client_msg_id = "063031b86f8e503c6038efb2b835f216" LIMIT 1

```


- insertMessageReactionExtension

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| messageReactionExtension                                     | string  | local_chat_log_reaction_extensions（消息扩展表对象）|对象转换成string

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sql
 INSERT INTO `local_chat_log_reaction_extensions` (`client_msg_id`,"");
```

- getAndUpdateMessageReactionExtension

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| clientMsgID | string |消息ID | |
| m | map[string]KeyValue|类型为map转换后的string其中KeyValue为obj,{"typeKey":"122","value":"test","latestUpdateTime":1670916659627} |
typeKey和value的类型为string，latestUpdateTime为整型，单位为毫秒 |

| 返回参数 | 类型 | 说明 | 备注      |
| --------- |--------| ----- |---------|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错  |
| errMsg | string | 详细的err信息 |         |
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sqlite

```

- deleteAndUpdateMessageReactionExtension

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| clientMsgID | string |消息ID | |
| m | map[string]KeyValue|类型为map转换后的string其中KeyValue为obj,{"typeKey":"122","value":"test","latestUpdateTime":1670916659627} |
typeKey和value的类型为string，latestUpdateTime为整型，单位为毫秒 |

| 返回参数 | 类型 | 说明 | 备注      |
| --------- |--------| ----- |---------|
| errCode | number | 自定义即可，0成功，非0失败 | 获取不到报错  |
| errMsg | string | 详细的err信息 |         |
| data      | string                                          | 可为"" ||

**参考sql语句说明：**

```sqlite

```
