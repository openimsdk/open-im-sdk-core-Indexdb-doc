# Js 实现 db 接口简要说明(待更新)

## 表结构以及需要实现的接口说明

> 所有的 db 接口返回值，统一由 `errCode`, `errMsg`, `data` 字段转换为字符串异步返回

### APP SDK 版本表

- 表名：local_app_sdk_version

```sqlite
CREATE TABLE `local_app_sdk_version` (
	`version` varchar(255),
	`installed` numeric,
	PRIMARY KEY (`version`)
)
```

#### 接口说明：

- getAppSDKVersion

**无输入参数**

| 返回参数 | 类型   | 说明                              | 备注                     |
| -------- | ------ | --------------------------------- | ------------------------ |
| errCode  | number | 自定义即可，0 成功，非 0 失败     | 如果获取不到信息需要返回错误 |
| errMsg   | string | 详细的 err 信息                   |                          |
| data     | string | LocalAppSDKVersion （表对象数据） | 对象转换成 string        |

**参考 sql 语句说明：**

```sqlite
SELECT * FROM `local_app_sdk_version` LIMIT 1
```

- setAppSDKVersion

| 输入参数           | 类型   | 说明           | 备注              |
| ------------------ | ------ | -------------- | ----------------- |
| LocalAppSDKVersion | string | （表对象数据） | 对象转换成 string |

| 返回参数 | 类型   | 说明                          | 备注 |
| -------- | ------ | ----------------------------- | ---- |
| errCode  | number | 自定义即可，0 成功，非 0 失败 |      |
| errMsg   | string | 详细的 err 信息               |      |

```sql
-- 若初始化未找到版本，则创建
INSERT INTO `local_app_sdk_version` (`version`) VALUES ("3.8.0");

-- 若已经存在版本，则更新
UPDATE `local_app_sdk_version`
SET `version`= "4.0.0"
WHERE
	`version` = "3.8.0"
```
