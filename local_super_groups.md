# Js实现db接口简要说明(待更新)
## 表结构以及需要实现的接口说明
>所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回
### 读扩散大群表
- 表名：local_super_groups
```sqlite
CREATE TABLE `local_super_groups` (`group_id` varchar(64),`name` text,`notification` varchar(255),`introduction` varchar(255),`face_url` varchar(255),`create_time` integer,`status` integer,`creator_user_id` varchar(64),`group_type` integer,`owner_user_id` varchar(64),`member_count` integer,`ex` varchar(1024),`attached_info` varchar(1024),`need_verification` integer,`look_member_info` integer,`apply_member_friend` integer,`notification_update_time` integer,`notification_user_id` text,PRIMARY KEY (`group_id`))
```


#### 接口说明：

- getJoinedSuperGroupList

**无输入参数**

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | LocalGroup（大群表对象数组） |对象转换成string|

**参考sql语句说明：**

```sql
 SELECT * FROM `local_super_groups`;
```

- insertSuperGroup

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupInfo                                               | string  |LocalGroup（大群表对象数据）|对象转换成string

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||
**参考sql语句说明：**

```sql
INSERT INTO `local_super_groups` (`group_id`,`name`,`notification`,`introduction`,`face_url`,`create_time`,`status`,`creator_user_id`,`group_type`,`owner_user_id`,`member_count`,`ex`,`attached_info`,`need_verification`,`look_member_info`,`apply_member_friend`,`notification_update_time`,`notification_user_id`) VALUES ("1225056077","普通","","","",1664348422,0,"4137580800",0,"4137580800",2,"","",0,0,0,0,"");
```

- updateSuperGroup

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |  大群群ID ||
| args     |object                                       |  更新字段参数对象 |内部是kv，k为字段名，v为需要更新的字段内容|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|如果没有更新到一行，需要返回失败|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||
**参考sql语句说明：**

```sql
UPDATE `local_super_groups` SET `group_id`="4280368097",`name`="工作群测试111",`notification`="",`introduction`="",`face_url`="",`create_time`=1664447111,`s`=0,`creator_user_id`="3359303407",`group_type`=2,`owner_user_id`="3359303407",`member_count`=2,`ex`="",`attached_info`="",`need_verification`=0,`look_member_info`=0,`apply_member_friend`=0,`notification_update_time`=0,`notification_user_id`="" WHERE `group_id` = "4280368097";
```

- deleteSuperGroup

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |  大群群ID ||


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||
**参考sql语句说明：**

```sql
DELETE FROM `local_super_groups` WHERE `local_groups`.`group_id` = "4280368097";
```

- getSuperGroupInfoByGroupID

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID      | string                                          | 大群群ID ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |如果获取不到大群信息也需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | LocalGroup（大群表对象数据） |对象转换成string|

**参考sql语句说明：**

```sql
SELECT * FROM `local_super_groups` WHERE group_id = "3045326383"  LIMIT 1;
```

- deleteAllSuperGroup

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||
**参考sql语句说明：**

```sql
DELETE FROM `local_super_groups`
```

- superGroupSearchMessageByKeyword

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| contentType                                     | []int  | 消息类型| 
| keywordList | []string | 关键字列表 |
| keywordListMatchType | int | 关键字匹配类型 |
| sourceID | string | 0为or匹配, 1为and匹配 | 来源id 可以为用户 大群 群id
| startTime | int | 开始时间戳 |
| endTime | int | 结束时间戳 |
| sessionType | int | 会话类型 |
| offset | int | 偏移数 | 
| count | int | 获取总数 |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"", 搜索数据 ||

**参考sql语句说明：**

```sql
SELECT * FROM `local_sg_chat_logs_748402675` WHERE recv_id="748402675" And send_time between 0 and 1666767397000 AND status <=3  And content_type IN (101,106) And (content like '%d%')  ORDER BY send_time DESC LIMIT 20
```

- superGroupSearchMessageByContentType

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| contentType                                     | []int  | 消息类型| 
| sourceID | string | 0为or匹配, 1为and匹配 | 来源id 可以为用户 大群 群id
| startTime | int | 开始时间戳 |
| endTime | int | 结束时间戳 |
| sessionType | int | 会话类型 | supergroup填3
| offset | int | 偏移数 | 
| count | int | 获取总数 |

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 |获取不到的时候返回空数组不需要返回错误|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"", 搜索数据 ||

**参考sql语句说明：**

```sql
SELECT * FROM `local_sg_chat_logs_748402675` WHERE session_type=3 And recv_id=="748402675" And send_time between 0 and 1666768501000 AND status <=3 And content_type IN (101,106) ORDER BY send_time DESC LIMIT 20
```


- getJoinedWorkingGroupIDList

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 |
| data      | number                                          |  |



```sql
SELECT * FROM `local_groups`
找出groupType为2的groupID
```


- getJoinedWorkingGroupList

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 |
| data      | number           

```sql
 SELECT * FROM `local_groups`
找出groupType为2的群对象返回
```