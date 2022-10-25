# Js实现db接口简要说明(待更新)
## 表结构以及需要实现的接口说明
>所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回
### 用户表
- 表名：local_users
```sqlite
CREATE TABLE `local_users` (`user_id` varchar(64),`name` varchar(255),`face_url` varchar(255),`gender` integer,`phone_number` varchar(32),`birth` integer,`email` varchar(64),`create_time` integer,`app_manger_level` integer,`ex` varchar(1024),`attached_info` varchar(1024),`global_recv_msg_opt` integer,PRIMARY KEY (`user_id`))
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
| user                                               | string  |LocalUser（用户表对象数据）|对象转换成string

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败 ||
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为"" ||
**参考sql语句说明：**

```sql
INSERT INTO `local_users` (`user_id`,`name`,`face_url`,`gender`,`phone_number`,`birth`,`email`,`create_time`,`app_manger_level`,`ex`,`attached_info`,`global_recv_msg_opt`) VALUES ("3045326383","Gordon","ic_avatar_01",0,"18349115126",0," ",0,1,"","",0)

```
- updateLoginUserByMap

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| userID     |string                                       |  用户ID ||
| args     |object                                       |  更新字段参数对象 |内部是kv，k为字段名，v为需要更新的字段内容|

| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败|如果没有更新到一行，需要返回失败|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||
**参考sql语句说明：**

```sql
UPDATE `local_users` SET `app_manger_level`=1,`attached_info`="",`birth`=0,`create_time`=0,`email`=" ",`ex`="",`face_url`="ic_avatar_01",`gender`=0,`global_recv_msg_opt`=0,`name`="Gordon111",`phone_number`="18349115126" WHERE `user_id` = "3045326383";
```

