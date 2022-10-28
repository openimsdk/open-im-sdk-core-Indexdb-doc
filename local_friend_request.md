# Js实现db接口简要说明(待更新)

## 表结构以及需要实现的接口说明

> 所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回

### 好友申请表

- 表名：local_friend_requests

```sqlite
create table local_friend_requests
(
    from_user_id    varchar(64),
    from_nickname   varchar(255),
    from_face_url   varchar(255),
    from_gender     INTEGER,
    to_user_id      varchar(64),
    to_nickname     varchar(255),
    to_face_url     varchar(255),
    to_gender       INTEGER,
    handle_result   INTEGER,
    req_msg         varchar(255),
    create_time     INTEGER,
    handler_user_id varchar(64),
    handle_msg      varchar(255),
    handle_time     INTEGER,
    ex              varchar(1024),
    attached_info   varchar(1024),
    primary key (from_user_id, to_user_id)
);
```

#### 接口说明：

- insertFriendRequest

| 输入参数     | 类型     | 说明 | 备注       |
| --------- |--------| ----- |----------|
|LocalFriendRequest   | string | （表对象数据） |对象转换成string|

| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |     |
| errMsg     | string     | 详细的err信息 |     |

**参考sql语句说明：**

```sqlite
INSERT INTO `local_friend_requests` (`from_user_id`, `from_nickname`, `from_face_url`, `from_gender`, `to_user_id`,
                                     `to_nickname`, `to_face_url`, `to_gender`, `handle_result`, `req_msg`,
                                     `create_time`, `handler_user_id`, `handle_msg`, `handle_time`, `ex`,
                                     `attached_info`)
VALUES ("123", "123", "", 1, "457", "457", "", 1, 0, "", 1666838764, "", "", 1666838764, "", "")
```

- deleteFriendRequestBothUserID

| 输入参数     | 类型     | 说明  | 备注  |
| --------- |--------|-----|-----|
|fromUserID| string |     |     |
|toUserID| string |     |     |

| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |     |
| errMsg     | string     | 详细的err信息 |     |

```sqlite
DELETE
FROM `local_friend_requests`
WHERE from_user_id = "457"
  and to_user_id = "123"
```

- updateFriendRequest

| 输入参数     | 类型     | 说明 | 备注       |
| --------- |--------| ----- |----------|
|LocalFriendRequest  | string |（表对象数据） |对象转换成string|

| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |     |
| errMsg     | string     | 详细的err信息 |     |

```sqlite
UPDATE `local_friend_requests`
SET `from_user_id`="123",
    `from_nickname`="123",
    `from_face_url`="",
    `from_gender`=1,
    `to_user_id`="457",
    `to_nickname`="457",
    `to_face_url`="",
    `to_gender`=1,
    `handle_result`=0,
    `req_msg`="",
    `create_time`=1666838873,
    `handler_user_id`="",
    `handle_msg`="",
    `handle_time`=1666838873,
    `ex`="",
    `attached_info`=""
WHERE `from_user_id` = "123"
  AND `to_user_id` = "457"
```

- getRecvFriendApplication

**无输入参数**
| 返回参数    | 类型     | 说明             | 备注  |
|---------|--------|----------------|-----|
| toUserID | string | 自定义即可，0成功，非0失败 |     |


| 返回参数    | 类型     | 说明             | 备注  |
|---------|--------|----------------|-----|
| errCode | number | 自定义即可，0成功，非0失败 |     |
| errMsg  | string | 详细的err信息       |     |
| data    | string | []LocalFriendRequest  （表对象数据） |对象转换成string|

```sqlite
SELECT *
FROM `local_friend_requests`
WHERE to_user_id = "3433303585"
ORDER BY create_time DESC
```

- getSendFriendApplication

| 输入参数     | 类型     | 说明 | 备注       |
| --------- |--------| ----- |----------|
|from_user_id                | string | | |

| 返回参数    | 类型     | 说明                             | 备注  |
|---------|--------|--------------------------------|-----|
| errCode | number | 自定义即可，0成功，非0失败                 |     |
| errMsg  | string | 详细的err信息                       |     |
| data    | string | []LocalFriendRequest   （表对象数据） |对象转换成string|

```sqlite
SELECT *
FROM `local_friend_requests`
WHERE from_user_id = "3433303585"
ORDER BY create_time DESC
```

- getFriendApplicationByBothID

| 输入参数     | 类型     | 说明  | 备注  |
| --------- |--------|-----|-----|
|fromUserID    | string |     |     |
|toUserID    | boolean   |     |     |

| 返回参数    | 类型     | 说明             | 备注  |
|---------|--------|----------------|-----|
| errCode | number | 自定义即可，0成功，非0失败 |     |
| errMsg  | string | 详细的err信息       |     |
| data    | string | LocalFriendRequest  （表对象数据） |对象转换成string|

```sqlite
SELECT *
FROM `local_friend_requests`
WHERE from_user_id = "457"
  AND to_user_id = "123"
LIMIT 1
```