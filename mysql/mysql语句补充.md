## `LIMIT` 限制查询结果
`LIMIT` 在 MySQL 里主要用来**限制查询结果返回的行数**，最常用于：
1. **取前 N 条数据**
    
2. **分页查询**
    
3. **限制 `UPDATE` / `DELETE` 影响的行数**
    
### 基本语法

```mysql
-- 只取前 10 条
SELECT * FROM users LIMIT 10;
-- 跳过前 20 条，再取 10 条
SELECT * FROM users LIMIT 20, 10;
-- 等价写法：跳过 20 条，取 10 条
SELECT * FROM users LIMIT 10 OFFSET 20;
```
注意：`LIMIT offset, count` 中，**offset 从 0 开始**。  
所以 `LIMIT 20, 10` 表示跳过前 20 行，返回第 21 到第 30 行。
 