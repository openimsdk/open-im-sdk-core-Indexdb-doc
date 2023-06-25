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
UPDATE local_notification_seqs set seq = 56 where conversation_id = '10001'
```



+ getNotificationAllSeqs
+ 
  **无输入参数**

| 返回参数 | 类型   | 说明                       | 备注 |
| -------- | ------ | -------------------------- | ---- |
| errCode  | number | 自定义即可，0成功，非0失败 |      |
| errMsg   | string | 详细的err信息              |      |

**参考sql语句说明：**

```sqlite
SELECT * from local_notification_seqs where 1 = 1;
```

