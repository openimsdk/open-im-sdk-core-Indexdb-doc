# Js实现db接口简要说明(待更新)
## 表结构以及需要实现的接口说明
>所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回
### 用户表
- 表名：local_stranger
```sqlite
CREATE TABLE `local_stranger`
(
    `user_id`             varchar(64),
    `name`                varchar(255),
    `face_url`            varchar(255),
    `create_time`         integer,
    `app_manger_level`    integer,
    `ex`                  varchar(1024),
    `attached_info`       varchar(1024),
    `global_recv_msg_opt` integer,
    PRIMARY KEY (`user_id`)
)
```

#### 接口说明：
- GetStrangerInfo

| 输入参数   | 类型     | 说明           | 备注 |
|--------|--------|--------------|----|
| userID | string | 用户ID列表(列表对象) |    |

| 返回参数    | 类型     | 说明                 | 备注                |
|---------|--------|--------------------|-------------------|
| errCode | number | 自定义即可，0成功，非0失败     | 如果获取不到用户信息也需要返回错误 |
| errMsg  | string | 详细的err信息           |                   |
| data    | string | LocalUser（用户表对象数据） | 对象转换成string       |

**参考sql语句说明：**

```sql
SELECT * FROM `local_stranger` WHERE user_id in ("3045326383");
```


- SetStrangerInfo

| 输入参数     | 类型     | 说明                          | 备注               |
|----------|--------|-----------------------------|------------------|
| stranger | string | 数组LocalStranger（陌生人表对象数组数据） | 对象转换成string      |

| 返回参数    | 类型     | 说明             | 备注 |
|---------|--------|----------------|----|
| errCode | number | 自定义即可，0成功，非0失败 |    |
| errMsg  | string | 详细的err信息       |    |
| data    | string | 可为""           |    |
**参考sql语句说明：**

```sql
IF EXISTS (SELECT * FROM local_stranger WHERE user_id = "3045326383")
UPDATE local_stranger
SET column1 = value1, column2 = value2, ...
    WHERE user_id = "3045326383";
ELSE 
    INSERT INTO your_table (column1, column2, ...)
    VALUES (value1, value2, ...);
```

