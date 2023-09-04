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

| 输入参数   | 类型       | 说明     | 备注 |
|--------|----------|--------|----|
| userID | []string | 用户ID列表 |    |

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

| 输入参数     | 类型       | 说明                          | 备注               |
|----------|----------|-----------------------------|------------------|
| stranger | []string | 列表对象LocalStranger（陌生人表对象数据） | 对象转换成string      |

| 返回参数    | 类型     | 说明             | 备注 |
|---------|--------|----------------|----|
| errCode | number | 自定义即可，0成功，非0失败 |    |
| errMsg  | string | 详细的err信息       |    |
| data    | string | 可为""           |    |
**参考sql语句说明：**

```sql
DELETE FROM `users` WHERE 1=1;
INSERT INTO `local_stranger`
    (`user_id`, `name`, `face_url`, `create_time`, `app_manger_level`, `ex`, `attached_info`, `global_recv_msg_opt`)
VALUES ('example_user', 'bantanger', 'http://example.com/face.jpg', 1618906879, 18, 'example', 'info', 1);
```

