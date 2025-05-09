# Js实现db接口简要说明(待更新)
## 表结构以及需要实现的接口说明
>所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回
### 用户表
- 表名：local_users
```sqlite
CREATE TABLE `local_users`
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
- getLoginUser

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| userID      | string                                          | 用户ID ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果获取不到用户信息也需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | LocalUser（用户表对象数据） |对象转换成string|

**参考sql语句说明：**

```sql
SELECT * FROM `local_users` WHERE user_id = "3045326383"  LIMIT 1;
```


- insertLoginUser

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| user                                               | string  |LocalUser（用户表对象数据）|对象转换成string|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||
**参考sql语句说明：**

```sql
INSERT INTO `local_users` (`user_id`, `name`, `face_url`, `create_time`, `app_manger_level`, `ex`, `attached_info`, `global_recv_msg_opt`) 
VALUES ('example_user', 'bantanger', 'http://example.com/face.jpg', 1618906879, 18, 'example', 'info', 1);
```


- updateLoginUser

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| user     | string | LocalUser（用户表对象数据） | 对象转换成 string<br />没有变化的字段不传入即可 |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|如果没有更新到一行，需要返回失败|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||
**参考sql语句说明：**

```sql
UPDATE `local_users` SET `name`="test",`app_manger_level` = 18 WHERE `user_id` = "7204255074"
```



+ updateLoginUserByMap（暂时废弃）
