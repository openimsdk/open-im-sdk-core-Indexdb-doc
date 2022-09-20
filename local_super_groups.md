# Js实现db接口简要说明(待更新)
## 表结构以及需要实现的接口说明
>所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回
### 读扩散大群表
- 表名：local_super_groups
```sqlite
CREATE TABLE `local_super_groups` (`group_id` varchar(64),`name` text,`notification` varchar(255),`introduction` varchar(255),`face_url` varchar(255),`create_time` integer,`status` integer,`creator_user_id` varchar(64),`group_type` integer,`owner_user_id` varchar(64),`member_count` integer,`ex` varchar(1024),`attached_info` varchar(1024),`need_verification` integer,`look_member_info` integer,`apply_member_friend` integer,`notification_update_time` integer,`notification_user_id` text,PRIMARY KEY (`group_id`))
```


#### 接口说明：
GetJoinedSuperGroupList
InsertSuperGroup
UpdateSuperGroup
DeleteSuperGroup
GetReadDiffusionGroupIDList
GetSuperGroupInfoByGroupID
