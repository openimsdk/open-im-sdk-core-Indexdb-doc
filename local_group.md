# Js实现db接口简要说明(待更新)

## 表结构以及需要实现的接口说明

> 所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回

### 群组表

- 表名：local_groups

```sqlite
CREATE TABLE local_groups
(
    group_id                 varchar(64) PRIMARY KEY,
    name                     TEXT,
    notification             varchar(255),
    introduction             varchar(255),
    face_url                 varchar(255),
    create_time              INTEGER,
    status                   INTEGER,
    creator_user_id          varchar(64),
    group_type               INTEGER,
    owner_user_id            varchar(64),
    member_count             INTEGER,
    ex                       varchar(1024),
    attached_info            varchar(1024),
    need_verification        INTEGER,
    look_member_info         INTEGER,
    apply_member_friend      INTEGER,
    notification_update_time INTEGER,
    notification_user_id     TEXT
)
```

#### 接口说明：

- insertGroup

| 输入参数     | 类型     | 说明 | 备注       |
| --------- |--------| ----- |----------|
|LocalGroup   | string | （表对象数据） |对象转换成string|

| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |     |
| errMsg     | string     | 详细的err信息 |     |

**参考sql语句说明：**

```sqlite
INSERT INTO `local_groups` (`group_id`, `name`, `notification`, `introduction`, `face_url`, `create_time`, `status`,
                            `creator_user_id`, `group_type`, `owner_user_id`, `member_count`, `ex`, `attached_info`,
                            `need_verification`, `look_member_info`, `apply_member_friend`, `notification_update_time`,
                            `notification_user_id`)
VALUES ("1234567", "测试1234", "", "", "", 1666777417, 0, "", 0, "", 0, "", "", 0, 0, 0, 0, "")
```

- deleteGroup

| 输入参数     | 类型     | 说明  | 备注  |
| --------- |--------|-----|-----|
|groupID| string |     |     |

| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |     |
| errMsg     | string     | 详细的err信息 |     |

```sqlite
DELETE
FROM `local_conversation_unread_messages`
WHERE conversation_id = "super_group_748402675"
  and send_time <= 0
```

- updateGroup

| 输入参数     | 类型     | 说明 | 备注       |
| --------- |--------| ----- |----------|
|LocalGroup  | string |（表对象数据） |对象转换成string|

| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |     |
| errMsg     | string     | 详细的err信息 |     |

```sqlite
UPDATE `local_groups`
SET `group_id`="1234567",
    `name`="hello",
    `notification`="",
    `introduction`="",
    `face_url`="",
    `create_time`=1666777635,
    `status`=0,
    `creator_user_id`="",
    `group_type`=0,
    `owner_user_id`="",
    `member_count`=0,
    `ex`="",
    `attached_info`="",
    `need_verification`=0,
    `look_member_info`=0,
    `apply_member_friend`=0,
    `notification_update_time`=0,
    `notification_user_id`=""
WHERE `group_id` = "1234567"
```

- getJoinedGroupList

**无输入参数**

| 返回参数    | 类型     | 说明             | 备注  |
|---------|--------|----------------|-----|
| errCode | number | 自定义即可，0成功，非0失败 |     |
| errMsg  | string | 详细的err信息       |     |
| data    | string | []LocalGroup  （表对象数据） |对象转换成string|

```sqlite
SELECT *
FROM `local_groups`
```

- getGroupInfoByGroupID

| 输入参数     | 类型     | 说明 | 备注       |
| --------- |--------| ----- |----------|
|groupID                | string | | |

| 返回参数    | 类型     | 说明             | 备注  |
|---------|--------|----------------|-----|
| errCode | number | 自定义即可，0成功，非0失败 |     |
| errMsg  | string | 详细的err信息       |     |
| data    | string | LocalGroup   （表对象数据） |对象转换成string|

```sqlite
SELECT *
FROM `local_group_members`
WHERE group_id = "748402675"
```

- getAllGroupInfoByGroupIDOrGroupName

| 输入参数     | 类型     | 说明  | 备注  |
| --------- |--------|-----|-----|
|keyword    | string |     |     |
|isSearchGroupID    | boolean   |     |     |
|isSearchGroupName  | boolean |     |     |

| 返回参数    | 类型     | 说明             | 备注  |
|---------|--------|----------------|-----|
| errCode | number | 自定义即可，0成功，非0失败 |     |
| errMsg  | string | 详细的err信息       |     |
| data    | string | []LocalGroup  （表对象数据） |对象转换成string|

```sqlite
SELECT *
FROM `local_groups`
WHERE group_id like "%123%"
   or name like "%123%"
ORDER BY create_time DESC
```