# **JSON_EXTRACT_STRING()**

## **函数说明**

`JSON_EXTRACT_STRING()` 用于从 JSON 数据中提取指定路径的字符串值。
  
如果在 where 条件中进行比较：

- 如果类型为 json 对象比较，使用 [JSON EXTRACT()](./json_extract.md)；
- 如果类型是字符串类型，使用 JSON_EXTRACT_STRING()；
- 如果类型为 float 或者 int，使用 [`JSON_EXTRACT_FLOAT64()`](./json_extract_float64.md)。

## **语法结构**

```sql
select col_name from tab_name where json_extract_string(jsonDoc, pathExpression)= number;
```

## **参数释义**

|  参数   | 说明 |
|  ----  | ----  |
| jsonDoc  | 这是包含 JSON 数据的列或表达式。|
| pathExpression  | 表示在 JSON 文档中访问某个值的路径。一次只能接收一个路径，路径以 `$` 开始，表示 JSON 文档的根，后面可以跟随点号 `.` 和键名或用方括号 [ ] 访问数组的元素。|
| number  | 指定 JSON 数据中要提取的值，为数值类型。 |

路径表达式必须以 `$` 字符开头：

- `,` 后跟键名，使用给定键命名对象中的成员。键名需要使用双引号包含。

- `[N]`：选择数组的 *path* 后，将数组中位置 `N` 处的值命名。数组位置是从零开始的整数。如果数组是负数，则产生报错。

- 路径可以包含 `*` 或 `**` 通配符：

   + `.[*]` 计算 JSON 对象中所有成员的值。

   + `[*]` 计算 JSON 数组中所有元素的值。

   + `prefix**suffix`：计算以命名前缀开头并以命名后缀结尾的所有路径。

- 文档中不存在的路径（或不存在的数据）评估为 `NULL`。

## **示例**

```sql
-- 创建测试表
CREATE TABLE student(
    id INT,
    info JSON
);

-- 插入测试数据
INSERT INTO student VALUES
    (1, '{"name": "tom", "age": 18, "scores": [85, 90, 78]}'),
    (2, '{"name": "bob", "age": 20, "scores": [75, 82, 91]}');

-- 提取字符串值
mysql> SELECT id FROM student WHERE json_extract_string(info, '$.name') = 'tom';
+------+
| id   |
+------+
|    1 |
+------+
1 row in set (0.01 sec)

-- 不存在的路径返回 NULL
mysql> SELECT json_extract_string(info, '$.address') FROM student;
+--------------------------------------+
| json_extract_string(info, $.address) |
+--------------------------------------+
| NULL                                 |
| NULL                                 |
+--------------------------------------+
2 rows in set (0.00 sec)
```