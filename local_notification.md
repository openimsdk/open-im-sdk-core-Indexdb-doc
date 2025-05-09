# Js实现db接口简要说明(待更新)

## 表结构以及需要实现的接口说明

> 所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回

### 通知表

+ 表名：local_notification_seqs

```sqlite
CREATE TABLE `local_notification_seqs`
(
    `conversation_id` char(128),
    `seq`             integer,
    PRIMARY KEY (`conversation_id`)
)
```

#### 接口说明：

+ setNotificationSeq

| 输入参数       | 类型   | 说明 | 备注 |
| -------------- | ------ | ---- | ---- |
| conversationID | string |      |      |
| seq            | int64  |      |      |

| 返回参数 | 类型   | 说明                       | 备注 |
| -------- | ------ | -------------------------- | ---- |
| errCode  | number | 自定义即可，0成功，非0失败 |      |
| errMsg   | string | 详细的err信息              |      |

**参考sql语句说明：**

```sqlite
先：
UPDATE local_notification_seqs set seq = 56 where conversation_id = '10001'
如果一行都未更新到，则：
INSERT INTO `local_notification_seqs` (`conversation_id`, `seq`) VALUES ("10001", "56")
```



+ getNotificationAllSeqs
+ 
  **无输入参数**

| 返回参数 | 类型   | 说明                                        | 备注 |
| -------- | ------ |-------------------------------------------| ---- |
| errCode  | number | 自定义即可，0成功，非0失败                            |      |
| errMsg   | string | 详细的err信息                                  |      |
| data      | string                                          | []local_notification_seqs（通知会话seq表对象数组数据） |对象转换成string|

**参考sql语句说明：**

```sqlite
SELECT * from local_notification_seqs where 1 = 1;
```

- batchInsertNotificationSeq

| 输入参数       | 类型   | 说明 | 备注 |
| -------------- | ------ | ---- | ---- |
| local_notification_seqs | string | []local_notification_seq 表对象数据  |数组对象转换成String|


| 返回参数 | 类型   | 说明                                        | 备注 |
| -------- | ------ |-------------------------------------------| ---- |
| errCode  | number | 自定义即可，0成功，非0失败                            |      |
| errMsg   | string | 详细的err信息                                  |      |

```sql
INSERT INTO `local_notification_seqs` (`conversation_id`, `seq`) 
VALUES ("n_2882899447_69085919821", 2), ("n_2882899447_69085919822", 3);
```