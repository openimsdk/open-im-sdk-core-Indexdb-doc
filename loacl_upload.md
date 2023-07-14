# Js实现db接口简要说明(待更新)

## 表结构以及需要实现的接口说明

> 所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回

### 文件上传表

- 表名：local_uploads

```sqlite
CREATE TABLE "local_uploads"
(
    "part_hash"   text,
    "upload_id"   varchar(1000),
    "upload_info"        varchar(2000),
    "expire_time" integer,
    "create_time" integer,
    PRIMARY KEY ("part_hash")
);
```

#### 接口说明：

- getUpload

**无输入参数**

| 返回参数    | 类型     | 说明                                 | 备注          |
|---------|--------|------------------------------------|-------------|
| errCode | number | 自定义即可，0成功，非0失败                     |             |
| errMsg  | string | 详细的err信息                           |             |
| data    | string | *model_struct.LocalUpload  （表对象数据） | 对象转换成string |

```sqlite
SELECT *
FROM ` local_uploads `
WHERE part_hash = '6213597e9474e0120349f7fef85af49b'
LIMIT 1
```

- insertUpload

| 输入参数   | 类型                        | 说明 | 备注 |
|--------|---------------------------|----|----|
| upload | *model_struct.LocalUpload |    |    |

| 返回参数    | 类型     | 说明             | 备注 |
|---------|--------|----------------|----|
| errCode | number | 自定义即可，0成功，非0失败 |    |
| errMsg  | string | 详细的err信息       |    |

```sqlite
INSERT INTO ` local_uploads ` ( ` part_hash `, ` upload_id `, ` upload_info `, ` expire_time `, ` create_time ` )
VALUES
    ( "6213597e9474e0120349f7fef85af49b", "jUZG6y1wZxJusIrbBnL3uwjUZG6y1wZxJusIrbBnL3uwjUZG6y1wZxJusIrbBnL3uw", "2d16d625d1cbecc0", 1689266361896, 1689241161896 )
```

- updateUpload

| 输入参数   | 类型                        | 说明 | 备注 |
|--------|---------------------------|----|----|
| upload | *model_struct.LocalUpload |    |    |

| 返回参数    | 类型     | 说明             | 备注 |
|---------|--------|----------------|----|
| errCode | number | 自定义即可，0成功，非0失败 |    |
| errMsg  | string | 详细的err信息       |    |

```sqlite
UPDATE `local_uploads`
SET `upload_id`= "jUZG6y1wZxJusIrbBnL3uwjUZG6y1wZxJusIrbBnL3uwjUZG6y1wZxJusIrbBnL3uw",`upload_info`= "2d16d625d1cbecc0",`expire_time`= 1689266558446,`create_time`= 1689241358446
WHERE
    `part_hash` = "6213597e9474e0120349f7fef85af49b"
```

- deleteUpload

| 输入参数     | 类型     | 说明 | 备注 |
|----------|--------|----|----|
| partHash | string |    |    |

| 返回参数    | 类型     | 说明             | 备注 |
|---------|--------|----------------|----|
| errCode | number | 自定义即可，0成功，非0失败 |    |
| errMsg  | string | 详细的err信息       |    |

**参考sql语句说明：**

```sqlite
DELETE
FROM
    ` local_uploads `
WHERE
    part_hash = "6213597e9474e0120349f7fef85af49b"
```

- deleteExpireUpload

**无输入参数**

| 返回参数    | 类型     | 说明             | 备注 |
|---------|--------|----------------|----|
| errCode | number | 自定义即可，0成功，非0失败 |    |
| errMsg  | string | 详细的err信息       |    |

```sqlite
SELECT
    *
FROM
    ` local_uploads `
WHERE
    expire_time <= 1689241660081
```