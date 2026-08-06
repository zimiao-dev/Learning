# 数据库中 JOIN 的用法

JOIN 能够使不同表中的数据通过特定条件连接起来。

主要类型包括：INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL JOIN.

## INNER JOIN 内连接

INNER JOIN 返回2个表中均有匹配的行，如果2个表中都至少存在一个匹配，则返回行。

---

## LEFT JOIN 左连接

LEFT JOIN 会返回左表中所有的行，即使右表中没有匹配。

如果右表中没有匹配，结果对应行的右表部分将填充为NULL。

---

## RIGHT JOIN 右连接

与 LEFT JOIN 相反， RIGHT JOIN 会返回右表中所有的行，即使左表中没有匹配。

---

## FULL JOIN 全连接

FULL JOIN 返回左表和右表中所有的行。当左表和右表在JOIN条件下匹配时，就会返回完整的行。

MySQL 不直接支持 FULL JOIN，但支持通过UNION关键字结合LEFT JOIN 和 RIGTH JOIN 来模拟。

UNION 会合并结果集中的重复行， UNION ALL 不会。