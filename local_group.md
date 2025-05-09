# Js实现db接口简要说明(待更新)

## 表结构以及需要实现的接口说明

> 所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回

### 群组表

- 表名：local_groups

```sqlite
CREATE TABLE `local_groups`
(
    `group_id`                 varchar(64),
    `name`                     text,
    `notification`             varchar(255),
    `introduction`             varchar(255),
    `face_url`                 varchar(255),
    `create_time`              integer,
    `status`                   integer,
    `creator_user_id`          varchar(64),
    `group_type`               integer,
    `owner_user_id`            varchar(64),
    `member_count`             integer,
    `ex`                       varchar(1024),
    `attached_info`            varchar(1024),
    `need_verification`        integer,
    `look_member_info`         integer,
    `apply_member_friend`      integer,
    `notification_update_time` integer,
    `notification_user_id`     text,
    `display_is_read`          numeric,
    PRIMARY KEY (`group_id`)
)
```

#### 接口说明：

- insertGroup

| 输入参数     | 类型     | 说明 | 备注       |
| --------- |--------| ----- |----------|
|LocalGroup   | string | （LocalGroup 表对象数据） |对象转换成string|

| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |     |
| errMsg     | string     | 详细的err信息 |     |

**参考sql语句说明：**

```sqlite
INSERT INTO `local_groups` (`group_id`, `name`, `notification`, `introduction`, `face_url`, `create_time`, `status`,
                            `creator_user_id`, `group_type`, `owner_user_id`, `member_count`, `ex`, `attached_info`,
                            `need_verification`, `look_member_info`, `apply_member_friend`, `notification_update_time`,
                            `notification_user_id`, `display_is_read`)
VALUES ("1234567", "测试1234", "", "", "", 1666777417, 0, "", 0, "", 0, "", "", 0, 0, 0, 0, "", true)
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
DELETE FROM `local_groups` WHERE `group_id` = '1728503199';
```



- updateGroup

| 输入参数     | 类型     | 说明 | 备注       |
| --------- |--------| ----- |----------|
|LocalGroup  | string |（LocalGroup 表对象数据） |对象转换成string|

| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |  获取不到报错   |
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
    `notification_user_id`="",
    `display_is_read`=false
WHERE `group_id` = "1234567"
```

- batchInsertGroup

| 输入参数     | 类型     | 说明 | 备注       |
| --------- |--------| ----- |----------|
|LocalGroup   | string | []LocalGroup 表对象数据 | 数组对象转换成string |

| 返回参数     | 类型            | 说明 | 备注  |
| --------- | ------------ | ----- |-----|
| errCode      | number   | 自定义即可，0成功，非0失败 |     |
| errMsg     | string     | 详细的err信息 |     |


```sql
INSERT INTO `local_groups` (`group_id`, `name`, `notification`, `introduction`, `face_url`, `create_time`, `status`, `creator_user_id`, `group_type`, `owner_user_id`, `member_count`, `ex`, `attached_info`, `need_verification`, `look_member_info`, `apply_member_friend`, `notification_update_time`, `notification_user_id`, `display_is_read`)
		VALUES("1234567", "测试1234", "", "", "", 1666777417, 0, "", 0, "", 0, "", "", "", 0, 0, 0, 0, ""), ("1234568", "测试5678", "新的通知", "这是一个测试组", "https://example.com/face.png", 1666777420, 1, "user123", 1, "user456", 10, "ex", "Attach", 1, 1, 1, 1666777425, "user789", true);
```


- DeleteAllGroup

**无输入参数**

| 返回参数    | 类型     | 说明             | 备注  |
|---------|--------|----------------|-----|
| errCode | number | 自定义即可，0成功，非0失败 |     |
| errMsg  | string | 详细的err信息       |     |


```sql
DELETE FROM `local_groups`
```

- getJoinedGroupListDB

**无输入参数**

| 返回参数    | 类型     | 说明             | 备注  |
|---------|--------|----------------|-----|
| errCode | number | 自定义即可，0成功，非0失败 |     |
| errMsg  | string | 详细的err信息       |     |
| data    | string | []LocalGroup  （表对象数据） |对象转换成string|

```sqlite
SELECT * FROM `local_groups`
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
SELECT * FROM `local_groups` WHERE group_id = "1728503199"
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



- subtractMemberCount

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID | string | |


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 |
| data      | number                                          |  |

```sql
UPDATE `local_groups` SET `member_count`=member_count-1 WHERE `group_id` = "groupID"
```




 - addMemberCount

 | 输入参数     | 类型                                                         | 说明 |备注|
 | --------- | ------------------------------------------------------------ | ----- |-----------------------|
 | groupID | string | |


 | 返回参数     | 类型                                                         | 说明 |备注|
 | --------- | ------------------------------------------------------------ | ----- |-----------------------|
 | errCode      | number                                         | 自定义即可，0成功，非0失败 ||
 | errMsg     | string                                          | 详细的err信息 |
 | data      | number                                          |  |

 ```sql
 UPDATE `local_groups` SET `member_count`= member_count+1 WHERE `group_id` = "1728503199"
 ```



+ getGroupMemberAllGroupIDs


| 返回参数 | 类型   | 说明                       | 备注             |
| -------- | ------ | -------------------------- | ---------------- |
| errCode  | number | 自定义即可，0成功，非0失败 |                  |
| errMsg   | string | 详细的err信息              |                  |
| data     | string | []string                   | 对象转换成string |

```sqlite
SELECT DISTINCT group_id FROM local_group_members
```



+ getGroups

 | 输入参数     | 类型                                                         | 说明 |备注|
 | --------- | ------------------------------------------------------------ | ----- |-----------------------|
 | groupIDs | string |群id数组 转换为String |

| 返回参数 | 类型   | 说明                       | 备注             |
| -------- | ------ | -------------------------- | ---------------- |
| errCode  | number | 自定义即可，0成功，非0失败 |                  |
| errMsg   | string | 详细的err信息              |                  |
| data     | string | []string                   | 对象转换成string |

```sqlite
SELECT * FROM local_groups where group_id in ("id1", "id2");
```
