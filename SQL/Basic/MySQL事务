# MySQL事务 以及 commit rollback

## 什么是事务 (Transaction)

事务就是一组 SQL 操作，打包成一个整体。

> 核心原则：要么全部成功，要么全部失败回滚，不会出现做一半的情况。

## 关键知识点总结

1. commit()

作用：把内存中的修改，持久化保存到数据库。

使用场景：INSERT、UPDATE、DELETE执行完成之后。

查询SELECT不需要 commit。

2. rollback()

作用：撤销本次事务所有未提交的改动。

使用场景：捕获到异常、业务逻辑失败时回滚。

3. 事务生命周期

```plaintext

开启连接 → 执行若干增删改SQL（内存，未落地）
    ├─没有异常 → commit() → 永久保存
    └─发生异常 → rollback() →全部撤销
关闭连接

```

4. autocommit自动提交模式

```python

conn = pymysql.connect(..., autocommit=True)

```


开启自动提交：每执行一条增删改 SQL，立刻自动 commit，不需要手动写conn.commit()。

> 开发业务项目不建议开启 autocommit，失去事务能力，无法保证一组操作原子性。

5. 写数据库代码标准范式：try‑except‑finally，保证出错回滚，资源一定关闭。

```python

import pymysql

def crud_best_practice():
    conn = pymysql.connect(
        host="127.0.0.1",
        port=3306,
        user="root",
        password="你的密码",
        charset="utf8mb4"
    )
    cur = conn.cursor()
    cur.execute("USE python_demo_db;")

    try:
        # 多条DML写在这里
        cur.execute("INSERT INTO student(name,age) VALUES (%s,%s)", ("A",20))
        cur.execute("UPDATE student SET age=%s WHERE name=%s", (22, "A"))

        conn.commit()
        print("操作完成已提交")
    except Exception as err:
        conn.rollback()
        print(f"操作失败，已回滚：{err}")
    finally:
        cur.close()
        conn.close()


if __name__ == "__main__":
    crud_best_practice()

```