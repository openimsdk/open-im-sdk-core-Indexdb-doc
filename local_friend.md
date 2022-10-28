# Js实现db接口简要说明(待更新)

## 表结构以及需要实现的接口说明

> 所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回

### 好友表

- 表名：local_friends

```sqlite
create table local_friends
(
    owner_user_id    varchar(64),
    friend_user_id   varchar(64),
    remark           varchar(255),
    create_time      INTEGER,
    add_source       INTEGER,
    operator_user_id varchar(64),
    name             varchar(255),
    face_url         varchar(255),
    gender           INTEGER,
    phone_number     varchar(32),
    birth            INTEGER,
    email            varchar(64),
    ex               varchar(1024),
    attached_info    varchar(1024),
    primary key (owner_user_id, friend_user_id)
);


```

#### 接口说明：

- insertFriend

| 输入参数     | 类型     | 说明 | 备注       |
| --------- |--------| ----- |----------|
|LocalFriend   | string | （表对象数据） |对象转换成string|

| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |     |
| errMsg     | string     | 详细的err信息 |     |

**参考sql语句说明：**

```sqlite
INSERT INTO `local_friends` (`owner_user_id`, `friend_user_id`, `remark`, `create_time`, `add_source`,
                             `operator_user_id`, `name`, `face_url`, `gender`, `phone_number`, `birth`, `email`, `ex`,
                             `attached_info`)
VALUES ("123", "456", "hello", 1666778999, 0, "789", "hhhh", "", 1, "13000000000", 1666778999, "123@qq.com", "", "")
```

- deleteFriend

| 输入参数     | 类型     | 说明  | 备注  |
| --------- |--------|-----|-----|
|friendUserID| string |     |     |
|ownerUserID| string | | |


| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |     |
| errMsg     | string     | 详细的err信息 |     |

```sqlite
DELETE FROM `local_friends` WHERE owner_user_id="3433303585" and friend_user_id="x"
```

- updateFriend

| 输入参数     | 类型     | 说明 | 备注       |
| --------- |--------| ----- |----------|
|LocalFriend  | string |（表对象数据） |对象转换成string|

| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |   获取不到报错  |
| errMsg     | string     | 详细的err信息 |     |

```sqlite
UPDATE `local_friends`
SET `owner_user_id`="123",
    `friend_user_id`="456",
    `remark`="hello",
    `create_time`=1666779080,
    `add_source`=0,
    `operator_user_id`="789",
    `name`="hhhh",
    `face_url`="",
    `gender`=1,
    `phone_number`="13000000000",
    `birth`=1666779080,
    `email`="123@qq.com",
    `ex`="",
    `attached_info`=""
WHERE `owner_user_id` = "123"
  AND `friend_user_id` = "456"
```

- getAllFriendList

| 输入参数     | 类型     | 说明  | 备注  |
| --------- |--------|-----|-----|
|ownerUserID    | string |     |     |

**无输入参数**

| 返回参数    | 类型     | 说明             | 备注  |
|---------|--------|----------------|-----|
| errCode | number | 自定义即可，0成功，非0失败 |     |
| errMsg  | string | 详细的err信息       |     |
| data    | string | []LocalFriend  （表对象数据） |对象转换成string|

```sqlite
SELECT *
FROM `local_friends`
WHERE owner_user_id = "3433303585"
```

- searchFriendList

| 输入参数     | 类型     | 说明  | 备注  |
| --------- |--------|-----|-----|
|keyword    | string |     |     |
|isSearchUserID    | boolean   |     |     |
|isSearchNickname  | boolean |     |     |
|isSearchRemark  | boolean |     |     |

| 返回参数    | 类型     | 说明                     | 备注  |
|---------|--------|------------------------|-----|
| errCode | number | 自定义即可，0成功，非0失败         |     |
| errMsg  | string | 详细的err信息               |     |
| data    | string | []LocalGroup   （表对象数据） |对象转换成string|

```sqlite
SELECT *
FROM `local_friends`
WHERE friend_user_id like "%123%"
   or name like "%123%"
   or remark like "%123%"
ORDER BY create_time DESC
```

- getFriendInfoByFriendUserID

| 输入参数              | 类型     | 说明  | 备注  |
|-------------------|--------|-----|-----|
| friendUserID      | string |     |     |
| ownerUserID | string | | |

| 返回参数    | 类型     | 说明             | 备注  |
|---------|--------|----------------|-----|
| errCode | number | 自定义即可，0成功，非0失败 |     |
| errMsg  | string | 详细的err信息       |     |
| data    | string | LocalGroup  （表对象数据） |对象转换成string|

```sqlite
SELECT *
FROM `local_friends`
WHERE owner_user_id = "3433303585"
  AND friend_user_id = "123"
LIMIT 1
```

- getFriendInfoList

| 输入参数     | 类型       | 说明  | 备注  |
| --------- |----------|-----|-----|
|friendUserIDList    | []string |     |     |

| 返回参数    | 类型     | 说明             | 备注  |
|---------|--------|----------------|-----|
| errCode | number | 自定义即可，0成功，非0失败 |     |
| errMsg  | string | 详细的err信息       |     |
| data    | string | []LocalFriend  （表对象数据） |对象转换成string|

```sqlite
SELECT *
FROM `local_friends`
WHERE friend_user_id IN ("123")
```