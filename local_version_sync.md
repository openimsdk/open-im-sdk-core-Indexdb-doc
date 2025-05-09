# Js 实现 db 接口简要说明(待更新)

## 表结构以及需要实现的接口说明

> 所有的 db 接口返回值，统一由 errCode,errMsg,data 字段转换为字符串异步返回

### 版本同步记录表

- 表名：local_sync_version

```sqlite
CREATE TABLE `local_sync_version` (
	`table` varchar(255),
	`entity_id` varchar(255),
	`version_id` text,
	`version` integer,
	`create_time` integer,
	`id_list` text,
	PRIMARY KEY (`table`, `entity_id`)
    );
```

#### 接口说明：

- getVersionSync

| 输入参数  | 类型   | 说明 | 备注 |
| --------- | ------ | ---- | ---- |
| tablename | string | 如果为`local_friends`，表示本地好友版本变动。<br> 如果为`local_groups`，表示本地群组版本变动。<br> 如果为`local_group_entities_version`，表示群组内部版本变动。    | 变动类型版本表     |
| entityID  | string | 如果为`local_friends` 和 `local_groups`，entityID为当前用户ID。<br> 如果为`local_group_entities_version`，entityID为群组ID。    | 变动类型版本表    | 版本表对应的实体ID     |

| 返回参数 | 类型   | 说明                            | 备注              |
| -------- | ------ | ------------------------------- | ----------------- |
| errCode  | number | 自定义即可，0 成功，非 0 失败   | 如果获取不到信息需要返回错误    |
| errMsg   | string | 详细的 err 信息                 |                   |
| data     | string | LocalVersionSync （表对象数据） | 对象转换成 string |

**参考 sql 语句说明：**

```sqlite
SELECT
	*
FROM
	`local_sync_version`
WHERE
	`table` = local_friends
	AND `entity_id` = 12345323
LIMIT 1
```

- setVersionSync

| 输入参数    | 类型   | 说明           | 备注              |
| ----------- | ------ | -------------- | ----------------- |
| LocalVersionSync | string | （表对象数据） | 对象转换成 string |

| 返回参数 | 类型   | 说明                          | 备注         |
| -------- | ------ | ----------------------------- | ------------ |
| errCode  | number | 自定义即可，0 成功，非 0 失败 | 获取不到报错 |
| errMsg   | string | 详细的 err 信息               |              |

```sql
-- 第一次设置版本表时为创建
INSERT INTO `local_sync_version` (`table`, `entity_id`, `version_id`, `version`, `create_time`, `id_list`)
		VALUES('local_group_entities_version', '1076204769', '667aabe3417b67f0f0d3cdee', 1076204769, 0, '["8879166186","1695766238","2882899447","5292156665"]');

-- 第二次调用时为更新
UPDATE
	`local_sync_version`
SET
	`table` = "local_group_entities_version",
	`entity_id` = "1076204769",
	`version_id` = "667aabe3417b67f0f0d3cdee",
	`version` = 1076204769,
	`id_list` = "[\"8879166186\",\"1695766238\",\"2882899447\",\"5292156665\"]"
WHERE
	`table` = "local_group_entities_version"
	AND `entity_id` = "1076204769";
```

- deleteVersionSync

| 输入参数     | 类型   | 说明 | 备注 |
| ------------ | ------ | ---- | ---- |
| tablename | string | 如果为`local_friends`，表示本地好友版本变动。<br> 如果为`local_groups`，表示本地群组版本变动。<br> 如果为`local_group_entities_version`，表示群组内部版本变动。    | 变动类型版本表     |
| entityID  | string | 如果为`local_friends` 和 `local_groups`，entityID为当前用户ID。<br> 如果为`local_group_entities_version`，entityID为群组ID。    | 变动类型版本表    | 版本表对应的实体ID     |

| 返回参数 | 类型   | 说明                          | 备注 |
| -------- | ------ | ----------------------------- | ---- |
| errCode  | number | 自定义即可，0 成功，非 0 失败 |      |
| errMsg   | string | 详细的 err 信息               |      |

```sqlite
DDELETE FROM `local_sync_version`
WHERE (`local_sync_version`.`table`, `local_sync_version`.`entity_id`)
	IN(('local_group_entities_version', '3378458183'));
```
