# Js实现db接口简要说明(待更新)
## 表结构以及需要实现的接口说明
>所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回
### 消息表(写扩散消息表)
- 表名：local_group_members
```sql
CREATE TABLE "local_group_members" (
  "group_id" varchar(64),
  "user_id" varchar(64),
  "nickname" varchar(255),
  "user_group_face_url" varchar(255),
  "role_level" integer,
  "join_time" integer,
  "join_source" integer,
  "inviter_user_id" text,
  "mute_end_time" integer DEFAULT 0,
  "operator_user_id" varchar(64),
  "ex" varchar(1024),
  "attached_info" varchar(1024),
  PRIMARY KEY ("group_id", "user_id")
)
```


- getGroupMemberInfoByGroupIDUserID

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |   ||
| userID     |string                                       | | |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没有返回空数组不报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" []LocalGroupMember  ||
|**参考sql语句说明：**|||转化为 string 类型|
```sql
SELECT * FROM `local_group_members` WHERE group_id = "x" AND user_id = "x2" LIMIT 1
```

- getAllGroupMemberList

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没有返回空数组不报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" []LocalGroupMember |转化为 string 类型|
|**参考sql语句说明：**||||
```sql
SELECT * FROM `local_group_members`
```

- getAllGroupMemberUserIDList

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没有返回空数组不报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" []LocalGroupMember |转化为 string 类型|
|**参考sql语句说明：**||||
```sql
SELECT * FROM `local_group_members`
```

- getGroupMemberCount

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |   ||


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败||
| errMsg     | string                                          | 详细的err信息 ||
| data      | int                                          |  |转化为 float64 类型|
|**参考sql语句说明：**||||

```sql
SELECT count(*) FROM `local_group_members` WHERE group_id = "x" 
```



- getGroupSomeMemberInfo

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |   ||
| userIDList     |string                                       | userid数组转为String   ||


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没有返回空数组不报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" []LocalGroupMember |转化为 string 类型|

**参考sql语句说明：**
```sql
SELECT * FROM `local_group_members` WHERE group_id = "x" And user_id IN ("1") 
```

- getGroupAdminID

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |   ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没有返回空数组不报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" []string |转化为 string 类型|

**参考sql语句说明：**
```sql
SELECT `user_id` FROM `local_group_members` WHERE group_id = "x" And role_level = 3
```

- getGroupMemberListByGroupID

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |   ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没有返回空数组不报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" []LocalGroupMember |转化为 string 类型|

**参考sql语句说明：**
```sql
SELECT * FROM `local_group_members` WHERE group_id = "x" 
```

- GetGroupMemberListByUserIDs

| 输入参数     | 类型                                                         | 说明 | 备注 |
| --------- | ------------------------------------------------------------ | ----- |--|
| groupID     |string                                       |   |  |
| filter     |int                                       |   | 总共7种取值，下面说明 |
| userIDs  | string |userid数组转为String  | |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没有返回空数组不报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" []LocalGroupMember |转化为 string 类型|

**参考sql语句说明：**
```sql
 -- filter为0: (获取所有的群成员)
SELECT * FROM `local_group_members` 
WHERE group_id = "x" AND user_id IN ("userID1", "userID2") ;

-- filter为1: (获取群主)
SELECT * FROM `local_group_members` 
WHERE group_id = "x" AND role_level = 100 AND user_id IN ("userID1", "userID2") ;

-- filter为2: (获取群管理员)
SELECT * FROM `local_group_members` 
WHERE group_id = "x" AND role_level = 60 AND user_id IN ("userID1", "userID2") ;

-- filter为3: (获取普通成员)
SELECT * FROM `local_group_members` 
WHERE group_id = "x" AND role_level = 20 AND user_id IN ("userID1", "userID2") ;

-- filter为4: (获取群管理员与普通成员)
SELECT * FROM `local_group_members` 
WHERE group_id = "x" AND (role_level = 60 OR role_level = 20) AND user_id IN ("userID1", "userID2") ;

-- filter为5: (获取群主与管理员)
SELECT * FROM `local_group_members` 
WHERE group_id = "x" AND (role_level = 100 OR role_level = 60) AND user_id IN ("userID1", "userID2") ;

```

- getGroupMemberListSplit

| 输入参数     | 类型                                                         | 说明 | 备注 |
| --------- | ------------------------------------------------------------ | ----- |--|
| groupID     |string                                       |   |  |
| filter     |int                                       |   | 总共7种取值，下面说明 |
| offset     |int                                       |  偏移 |  |
| count     |int                                       |  获取总数 |  |
| loginUserID  | string | |登陆者实例ID |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没有返回空数组不报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" []LocalGroupMember |转化为 string 类型|

**参考sql语句说明：**
```sql
  filter为0:(获取所有的群成员)
   SELECT * FROM `local_group_members` WHERE group_id = "x" ORDER BY role_level DESC,join_time ASC LIMIT 20 OFFSET 10
  filter为1:(获取群主) 
   SELECT * FROM `local_group_members` WHERE group_id = "x" And role_level = 100  LIMIT 20 OFFSET 10
  filter为2:(获取群管理员)
   SELECT * FROM `local_group_members` WHERE group_id = "x" And role_level = 60 ORDER BY join_time ASC LIMIT 20 OFFSET 10
  filter为3:(获取普通成员)
   SELECT * FROM `local_group_members` WHERE group_id = "x" And role_level = 20 ORDER BY join_time ASC LIMIT 20 OFFSET 10
  filter为4:(获取群管理员与普通成员)
  SELECT * FROM `local_group_members` WHERE group_id = "x" And (role_level = 60 or role_level = 20) ORDER BY role_level DESC,join_time ASC LIMIT 20 OFFSET 10
  filter为5:(获取群主与管理员)
  SELECT * FROM `local_group_members` WHERE group_id = "x" And (role_level = 100 or role_level = 60) ORDER BY role_level DESC,join_time ASC LIMIT 20 OFFSET 10
  filter为6:(获取群成员不包含自己,其中user_id为登陆者实例ID)
  SELECT * FROM `local_group_members` WHERE group_id = "x" And user_id != xx  LIMIT 20 OFFSET 10

```

- getGroupMemberOwnerAndAdmin

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |   ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没有返回空数组不报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" []LocalGroupMember |转化为 string 类型|

**参考sql语句说明：**
```sql
SELECT * FROM `local_group_members` WHERE group_id = "x" And role_level > 1 ORDER BY join_time DESC
```

- getGroupMemberOwner

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |   ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没有返回空数组不报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" []LocalGroupMember |转化为 string 类型|

**参考sql语句说明：**
```sql
SELECT * FROM `local_group_members` WHERE group_id = "x" And role_level = 2
```

- getGroupMemberListSplitByJoinTimeFilter

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |   ||
| offset      |int                                       |   ||
| count       |int                                       |   ||
| joinTimeBegin     |string                                       |   ||
| joinTimeEnd     |int                                       |   ||
| userIDList     | string                                       | userid数组转为String  ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没有返回空数组不报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" []LocalGroupMember |转化为 string 类型|

**参考sql语句说明：**
```sql
   userIDList为空数组 SELECT * FROM `local_group_members` WHERE group_id = "x" And join_time  between 0 and 100000000000  ORDER BY join_time DESC LIMIT 20 OFFSET 10
   userIDList不为空数组 SELECT * FROM `local_group_members` WHERE group_id = "x" And join_time  between 0 and 100000000000 And user_id NOT IN ("xxxx") ORDER BY join_time DESC LIMIT 20 OFFSET 10
```

- getGroupOwnerAndAdminByGroupID

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |   ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没有返回空数组不报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" []LocalGroupMember |转化为 string 类型|

**参考sql语句说明：**
```sql
SELECT * FROM `local_group_members` WHERE group_id = "x"  AND role_level > 1
```

- getGroupMemberUIDListByGroupID

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |   ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没有返回空数组不报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" []string |转化为 string 类型|

**参考sql语句说明：**
```sql
SELECT `user_id` FROM `local_group_members` WHERE group_id = "x"
```

- insertGroupMember

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupMember     |LocalGroupMember                                       |   ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||

**参考sql语句说明：**
```sql
INSERT INTO `local_group_members` (`group_id`,`user_id`,`nickname`,`user_group_face_url`,`role_level`,`join_time`,`join_source`,`inviter_user_id`,`mute_end_time`,`operator_user_id`,`ex`,`attached_info`) VALUES ("xx","xx","1","2",1,100,120,"23",0,"21312","","")
```


- batchInsertGroupMember

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupMember     | string                                     | []LocalGroupMember 表对象数据  |数组对象转换成String |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|
| errMsg     | string                                          | 详细的err信息 |
| data      | string                                          | 可为""  |

**参考sql语句说明：**
```sql
INSERT INTO `local_group_members` (`group_id`,`user_id`,`nickname`,`user_group_face_url`,`role_level`,`join_time`,`join_source`,`inviter_user_id`,`mute_end_time`,`operator_user_id`,`ex`,`attached_info`) VALUES ("xx","xx","1","2",1,100,120,"23",0,"21312","",""),("xx","xx","1","2",1,100,120,"23",0,"21312","","")
```

- deleteGroupMember

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                      |   | |
| userID     |string                                      |   | |


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||

**参考sql语句说明：**
```sql
DELETE FROM `local_group_members` WHERE group_id="xxx" and user_id="x2"
```

- deleteGroupAllMembers
| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |   ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||

**参考sql语句说明：**
```sql
DELETE FROM `local_group_members` WHERE group_id="x2" 
```

- updateGroupMember

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupMember     |LocalGroupMember                                       |   ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败| 失败报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||

**参考sql语句说明：**

```sql
UPDATE `local_group_members` SET `group_id`="1",`user_id`="1",`nickname`="1",`user_group_face_url`="1",`role_level`=0,`join_time`=0,`join_source`=0,`inviter_user_id`="",`mute_end_time`=0,`operator_user_id`="",`ex`="",`attached_info`="" WHERE `group_id` = "1" AND `user_id` = "1"
```

- updateGroupMemberField

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                      |   | |
| userID     |string                                      |   | |
| args     |object                                       |  更新字段参数对象 |内部是kv，k为字段名，v为需要更新的字段内容|


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败| 失败报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||

**参考sql语句说明：**

```sql
UPDATE `local_group_members` SET `nickname`="x" WHERE `group_id` = "1" AND `user_id` = "1"
```

- searchGroupMembers

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| keyword | string|  |
| groupID | string |
| isSearchMemberNickname | bool |
| isSearchUserID | bool |
| offset | int |
| count|  int |


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败| |
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  []LocalGroupMember||

**参考sql语句说明：**
```sql
groupID不为""SELECT * FROM `local_group_members` WHERE ( user_id like "%1%" or nickname like "%1%"  )  and group_id IN ("sad1")  ORDER BY role_level DESC,join_time DESC LIMIT 20 OFFSET 10
groupID为""  SELECT * FROM `local_group_members` WHERE user_id like "%1%" or nickname like "%1%"  ORDER BY role_level DESC,join_time DESC LIMIT 20 OFFSET 10
SELECT * FROM `local_group_members` WHERE user_id like "%1%"  ORDER BY role_level DESC,join_time DESC LIMIT 20 OFFSET 10
```


- GetUserJoinedGroupIDs

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| userID | string|  |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败| |
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          |   []string||

**参考sql语句说明：**
```sql
select group_id from `local_group_members` where user_id = ?;
```
