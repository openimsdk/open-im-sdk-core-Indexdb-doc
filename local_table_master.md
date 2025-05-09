# Js实现db接口简要说明(待更新)

## 表结构以及需要实现的接口说明

> 所有的db接口返回值，统一由errCode,errMsg,data字段转换为字符串异步返回


### 主Table结构表
> 该表为 sqlite 自动生成的表，不需要进行手动创建！

- 表名：`sqlite_master`

```sql
create table sqlite_master
(
    type     TEXT,
    name     TEXT,
    tbl_name TEXT,
    rootpage INT,
    sql      TEXT
);

```

#### 接口说明：

- getExistedTables

**无输入参数**

| 返回参数 | 类型 | 说明 | 备注 |
| --------- |--------| ----- |-----|
| errCode | number | 自定义即可，0成功，非0失败 | |
| errMsg | string | 详细的err信息 | |
| data    | string | []name |数组转换成string|


```sql
SELECT name FROM sqlite_master WHERE type='table'
```