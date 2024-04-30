# SUBVECTOR()

## 函数说明

`SUBVECTOR()` 函数用于从向量中提取子向量。

## 函数语法

```
> SUBVECTOR(vec, pos, len)
```

## 参数释义

|  参数   | 说明  |
|  ----  | ----  |
|vec     |	必需参数。从中提取子向量的源向量|
|pos     |	必需参数。开始提取的位置。向量中的第一个位置是 1，如果 pos 为正，则函数从向量的开头提取。如果 pos 为负，则提取是从向量的末尾开始。|
|len	 |  可选参数。要提取的维度数。默认从位置 pos 开始到向量末尾的子向量。如果 len 小于 1，则返回空向量。 |

## 示例

```sql
mysql> SELECT SUBVECTOR("[1,2,3]", 2);
+-----------------------+
| subvector([1,2,3], 2) |
+-----------------------+
| [2, 3]                |
+-----------------------+
1 row in set (0.01 sec)

mysql> SELECT SUBVECTOR("[1,2,3]",-1,1);
+---------------------------+
| subvector([1,2,3], -1, 1) |
+---------------------------+
| [3]                       |
+---------------------------+
1 row in set (0.00 sec)

mysql> SELECT SUBVECTOR("[1,2,3]",-1,0);
+---------------------------+
| subvector([1,2,3], -1, 0) |
+---------------------------+
| []                        |
+---------------------------+
1 row in set (0.00 sec)
```