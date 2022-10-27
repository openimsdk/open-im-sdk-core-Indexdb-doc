**参考sql语句说明：**
```sql
CREATE TABLE "temp_cache_local_chat_logs" (
  "client_msg_id" char(64),
  "server_msg_id" char(64),
  "send_id" char(64),
  "recv_id" char(64),
  "sender_platform_id" integer,
  "sender_nick_name" varchar(255),
  "sender_face_url" varchar(255),
  "session_type" integer,
  "msg_from" integer,
  "content_type" integer,
  "content" varchar(1000),
  "is_read" numeric,
  "status" integer,
  "seq" integer DEFAULT 0,
  "send_time" integer,
  "create_time" integer,
  "attached_info" varchar(1024),
  "ex" varchar(1024),
  PRIMARY KEY ("client_msg_id")
);
```

- batchInsertTempCacheMessageList

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| MessageList     |[]TempCacheLocalChatLog                                      |   | |



| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败| 失败报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||

**参考sql语句说明：**

```sql
INSERT INTO `temp_cache_local_chat_logs` (`client_msg_id`,`server_msg_id`,`send_id`,`recv_id`,`sender_platform_id`,`sender_nick_name`,`sender_face_url`,`session_type`,`msg_from`,`content_type`,`content`,`is_read`,`status`,`seq`,`send_time`,`create_time`,`attached_info`,`ex`) VALUES ("1","1","1","1",1,"1","1",1,100,200,"xxx",false,100,0,0,0,"",""),("1","1","1","1",1,"1","1",1,100,200,"xxx",false,100,0,0,0,"","")
```

- InsertTempCacheMessage

| 输入参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| Message     | TempCacheLocalChatLog                                      |   | |


| 返回参数     | 类型                                                         | 说明 |备注|
| --------- | ------------------------------------------------------------ | ----- |-----------------------|
| errCode      | number                                         | 自定义即可，0成功，非0失败| 失败报错|
| errMsg     | string                                          | 详细的err信息 ||
| data      | string                                          | 可为""  ||

**参考sql语句说明：**

```sql
INSERT INTO `temp_cache_local_chat_logs` (`client_msg_id`,`server_msg_id`,`send_id`,`recv_id`,`sender_platform_id`,`sender_nick_name`,`sender_face_url`,`session_type`,`msg_from`,`content_type`,`content`,`is_read`,`status`,`seq`,`send_time`,`create_time`,`attached_info`,`ex`) VALUES ("1","1","1","1",1,"1","1",1,100,200,"xxx",false,100,0,0,0,"","")
```