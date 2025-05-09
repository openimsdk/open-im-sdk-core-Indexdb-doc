# Js实现db接口简要说明(待更新)

## 表结构以及需要实现的接口说明

> 所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回

### 黑名单表

- 表名：

```sqlite
create table local_blacks
(
    owner_user_id    varchar(64),
    block_user_id    varchar(64),
    nickname         varchar(255),
    face_url         varchar(255),
    gender           INTEGER,
    create_time      INTEGER,
    add_source       INTEGER,
    operator_user_id varchar(64),
    ex               varchar(1024),
    attached_info    varchar(1024),
    primary key (owner_user_id, block_user_id)
);
```

#### 接口说明：

- getBlackList

**无输入参数**

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | []LocalBlack（表对象数据） |数组对象转换成string|

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_blacks`
```

- getBlackListUserID

**无输入参数**

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string |[]blackListUid | 对象转换成string |

**参考sql语句说明：**

```sqlite
SELECT `block_user_id`
FROM `local_blacks`
```

- getBlackInfoByBlockUserID

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| blockUserID | string | | |
| ownerUserID | string | | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | LocalBlack（表对象数据） |对象转换成string|

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_blacks`
WHERE owner_user_id = "3433303585"
  AND block_user_id = "456"
LIMIT 1
```

- getBlackInfoList

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| blockUserIDList | string |[]string | 黑名单userid数组转为String|

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data | string | []LocalBlack（表对象数据） |数组对象转换成string|

**参考sql语句说明：**

```sqlite
SELECT *
FROM `local_blacks`
WHERE block_user_id IN ("456")
```

- insertBlack

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| LocalBlack | string |（表对象数据） |对象转换成string|

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
INSERT INTO `local_blacks` (`owner_user_id`, `block_user_id`, `nickname`, `face_url`, `gender`, `create_time`,
                            `add_source`, `operator_user_id`, `ex`, `attached_info`)
VALUES ("123", "456", "hello", "", 1, 1666866302, 1, "789", "asdasdasa", "asdasds")
```

- updateBlack

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| LocalBlack | string |（表对象数据） |对象转换成string|

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
UPDATE `local_blacks`
SET `nickname`="hello",
    `gender`=1,
    `create_time`=1666866338,
    `add_source`=1,
    `operator_user_id`="789",
    `ex`="asdasdasa",
    `attached_info`="asdasds"
WHERE `owner_user_id` = "123"
  AND `block_user_id` = "456"
```

- deleteBlack

| 输入参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| blockUserID | string | | |
| ownerUserId | string | |

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |

**参考sql语句说明：**

```sqlite
DELETE
FROM `local_blacks`
WHERE owner_user_id = "3433303585"
  and block_user_id = "456"
```



