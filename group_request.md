加群申请表
- 表名：local_group_requests
```sql
CREATE TABLE `local_group_requests`
(
    `group_id`        varchar(64),
    `group_name`      text,
    `notification`    varchar(255),
    `introduction`    varchar(255),
    `face_url`        varchar(255),
    `create_time`     integer,
    `status`          integer,
    `creator_user_id` varchar(64),
    `group_type`      integer,
    `owner_user_id`   varchar(64),
    `member_count`    integer,
    `user_id`         varchar(64),
    `nickname`        varchar(255),
    `user_face_url`   varchar(255),
    `handle_result`   integer,
    `req_msg`         varchar(255),
    `handle_msg`      varchar(255),
    `req_time`        integer,
    `handle_user_id`  varchar(64),
    `handle_time`     integer,
    `ex`              varchar(1024),
    `attached_info`   varchar(1024),
    `join_source`     integer,
    `inviter_user_id` text,
    PRIMARY KEY (`group_id`, `user_id`)
)
```



- insertGroupRequest

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupRequest     |string                                       | LocalGroupRequest（用户表对象数据） |对象转换成 string|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败||
| errMsg     | string                                          | 详细的err信息 ||

**参考sql语句说明：**

```sql
INSERT INTO `local_group_requests` (`group_id`,`group_name`,`notification`,`introduction`,`face_url`,`create_time`,`status`,`creator_user_id`,`group_type`,`owner_user_id`,`member_count`,`user_id`,`nickname`,`user_face_url`,`handle_result`,`req_msg`,`handle_msg`,`req_time`,`handle_user_id`,`handle_time`,`ex`,`attached_info`,`join_source`,`inviter_user_id`) VALUES ("x","x","x","x","x",123123123,1,"x",0,"",0,"123","123","132",0,"123","213",0,"",0,"","",0,"")
```



- deleteGroupRequest

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |   ||
| userID     |string                                       |   ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败||
| errMsg     | string                                          | 详细的err信息 ||

**参考sql语句说明：**
```sql
DELETE FROM `local_group_requests` WHERE group_id="x" and user_id="user"
```



- updateGroupRequest

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupRequest | string | LocalGroupRequest（用户表对象数据） |对象转换成 string|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没更新上报错|
| errMsg     | string                                          | 详细的err信息 ||

**参考sql语句说明：**
```sql
 UPDATE `local_group_requests` SET `group_id`="x",`group_name`="x",`notification`="x",`introduction`="x",`face_url`="x",`create_time`=123123123,`status`=1,`creator_user_id`="x",`group_type`=0,`owner_user_id`="",`member_count`=0,`user_id`="123",`nickname`="123",`user_face_url`="132",`handle_result`=0,`req_msg`="123",`handle_msg`="213",`req_time`=0,`handle_user_id`="",`handle_time`=0,`ex`="",`attached_info`="",`join_source`=0,`inviter_user_id`="" WHERE `group_id` = "x" AND `user_id` = "123"
```



- getSendGroupApplication

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败||
| errMsg     | string                                          | 详细的err信息 ||

**参考sql语句说明：**

```sql
SELECT * FROM `local_group_requests` ORDER BY create_time DESC
```





- 表名：local_admin_group_requests

```sqlite
CREATE TABLE `local_admin_group_requests`
(
    `group_id`        varchar(64),
    `group_name`      text,
    `notification`    varchar(255),
    `introduction`    varchar(255),
    `face_url`        varchar(255),
    `create_time`     integer,
    `status`          integer,
    `creator_user_id` varchar(64),
    `group_type`      integer,
    `owner_user_id`   varchar(64),
    `member_count`    integer,
    `user_id`         varchar(64),
    `nickname`        varchar(255),
    `user_face_url`   varchar(255),
    `handle_result`   integer,
    `req_msg`         varchar(255),
    `handle_msg`      varchar(255),
    `req_time`        integer,
    `handle_user_id`  varchar(64),
    `handle_time`     integer,
    `ex`              varchar(1024),
    `attached_info`   varchar(1024),
    `join_source`     integer,
    `inviter_user_id` text,
    PRIMARY KEY (`group_id`, `user_id`)
)
```



- insertAdminGroupRequest

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupRequest | string | LocalGroupRequest（用户表对象数据） |对象转换成 string|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没更新上报错|
| errMsg     | string                                          | 详细的err信息 ||

**参考sql语句说明：**

```sql
INSERT INTO `local_admin_group_requests` (`group_id`,`group_name`,`notification`,`introduction`,`face_url`,`create_time`,`status`,`creator_user_id`,`group_type`,`owner_user_id`,`member_count`,`user_id`,`nickname`,`user_face_url`,`handle_result`,`req_msg`,`handle_msg`,`req_time`,`handle_user_id`,`handle_time`,`ex`,`attached_info`,`join_source`,`inviter_user_id`) VALUES ("1","1","","","",0,0,"",0,"",0,"1","1","1",0,"1","1",0,"",0,"","",0,"")
```



- deleteAdminGroupRequest

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupID     |string                                       |   ||
| userID     |string                                       |   ||

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没更新上报错|
| errMsg     | string                                          | 详细的err信息 ||

**参考sql语句说明：**
```sql
DELETE FROM `local_admin_group_requests` WHERE group_id="x" and user_id="user"
```



- updateAdminGroupRequest

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| groupRequest | string | LocalGroupRequest（用户表对象数据） |对象转换成 string|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没更新上报错|
| errMsg     | string                                          | 详细的err信息 ||

**参考sql语句说明：**
```sql
UPDATE `local_admin_group_requests` SET `group_id`="1",`group_name`="1",`notification`="",`introduction`="",`face_url`="",`create_time`=0,`status`=0,`creator_user_id`="",`group_type`=0,`owner_user_id`="",`member_count`=0,`user_id`="1",`nickname`="1",`user_face_url`="1",`handle_result`=0,`req_msg`="1",`handle_msg`="1",`req_time`=0,`handle_user_id`="",`handle_time`=0,`ex`="",`attached_info`="",`join_source`=0,`inviter_user_id`="" WHERE `group_id` = "1" AND `user_id` = "1"
```



- getAdminGroupApplication


| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|没更新上报错|
| errMsg     | string                                          | 详细的err信息 ||

**参考sql语句说明：**
```sql
SELECT * FROM `local_admin_group_requests` ORDER BY create_time DESC
```