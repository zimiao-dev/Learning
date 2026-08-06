## LIKE 模糊匹配

```
WHERE column_name LIKE 'pattern'
```

pattern 用于模式匹配， 可以包含通配符

---

| 通配符 | 作用 |
|------|------|
| % | 零个或多个字符 |
| _ | 一个字符 |

---


## RLIKE REGEXP 正则表达式匹配

```
WHERE column_name RLIKE 'pattern'
```

```
WHERE column_name RLIKE 'pattern'
```

> column_name 需要匹配的列名

> pattern 正则表达式模式

> 默认情况下， MySQL正则表达式不区分大小写，若要区分大小写，则使用关键字BINARY

```
WHERE column_name BINARY 'pattern'
```

---

| 模式 | 作用 |
|---|------|
| ^ | 匹配字符串开头 |
| $ | 匹配字符串结尾 |
| . | 匹配任意单个字符 |
| * | 匹配前一个字符零次或多次 |
| + | 匹配前一个字符一次或多次 |
| {n} | 匹配n次 |
| {m, n} | 最少匹配 m 次，最多匹配 n 次 |
| [] | 匹配括号内的任意一个字符 |
| [^] | 匹配不包含括号内的任意字符 |
| \| | 表示条件或 |
| \w | 匹配数字、字母和下划线 |
| \s | 匹配空白字符，空格、制表符、换页符等 |
| \d | 匹配一个数字字符 相当于 [0-9]

---
