# SHOW PITR

## 语法说明

`SHOW PITR` 返回当前租户下创建的 PITR 的信息。

## 语法结构

```
> SHOW PITR[WHERE expr]
```

## 示例

```sql
create pitr account_pitr1 range 2 "h";
create pitr db_pitr1 for database db1 range 1 'y';
create pitr tab_pitr1 for database  db1 table t1 range 1 'y';

mysql> show pitr;
+---------------+---------------------+---------------------+------------+--------------+---------------+------------+-------------+-----------+
| PITR_NAME     | CREATED_TIME        | MODIFIED_TIME       | PITR_LEVEL | ACCOUNT_NAME | DATABASE_NAME | TABLE_NAME | PITR_LENGTH | PITR_UNIT |
+---------------+---------------------+---------------------+------------+--------------+---------------+------------+-------------+-----------+
| tab_pitr1     | 2024-10-18 14:28:53 | 2024-10-18 14:28:53 | table      | acc1         | db1           | t1         |           1 | y         |
| db_pitr1      | 2024-10-18 14:26:02 | 2024-10-18 14:26:02 | database   | acc1         | db1           | *          |           1 | y         |
| account_pitr1 | 2024-10-18 14:23:12 | 2024-10-18 14:23:12 | account    | acc1         | *             | *          |           2 | h         |
+---------------+---------------------+---------------------+------------+--------------+---------------+------------+-------------+-----------+
3 rows in set (0.01 sec)

mysql> show pitr where pitr_name='tab_pitr1';
+-----------+---------------------+---------------------+------------+--------------+---------------+------------+-------------+-----------+
| pitr_name | created_time        | modified_time       | pitr_level | account_name | database_name | table_name | pitr_length | pitr_unit |
+-----------+---------------------+---------------------+------------+--------------+---------------+------------+-------------+-----------+
| tab_pitr1 | 2024-10-18 14:28:53 | 2024-10-18 14:28:53 | table      | acc1         | db1           | t1         |           1 | y         |
+-----------+---------------------+---------------------+------------+--------------+---------------+------------+-------------+-----------+
1 row in set (0.00 sec)
```
