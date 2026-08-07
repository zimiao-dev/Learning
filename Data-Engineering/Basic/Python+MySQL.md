# Python + MySQL 本地开发环境操作文档

## 文档说明

> 适用：Windows10/11，使用 Python 内置venv虚拟环境，PyMySQL 连接 MySQL 数据库，配套 Git 工程规范（.gitignore、requirements.txt）。
>
> 目标：实现项目依赖隔离、环境可复现，不同项目第三方包互不干扰，项目可分享给他人快速复原运行。

---

## 1. 准备工作

### 前置软件

1. 已安装 Python，配置好系统环境变量，命令行可执行python --version

2. 已安装 MySQL 服务，确认本地端口 3306，知道 root 账号密码

3. VSCode 编辑器

### 新建项目文件夹

1. 在磁盘建立项目目录，示例路径：D:\Code\PycharmProjects\python_mysql_demo

2. 用 VSCode 打开该文件夹。

---

## 2. 创建 Python 虚拟环境 venv

> venv：Python 自带虚拟环境，为本项目创建独立 Python 运行环境，第三方库只安装在此项目内，不污染全局 Python。

1. VSCode 打开CMD 终端（设置终端默认配置为Command Prompt，避免 PowerShell 脚本策略报错）

2. 终端确认当前路径处于项目根目录

```cmd

D:\Code\PycharmProjects\python_mysql_demo>

```

3. 执行命令创建虚拟环境，文件夹名称固定为venv

```cmd

python -m venv venv

```

> 执行完成，项目下生成venv文件夹，里面存放本项目 Python 解释器、pip、第三方包。

### 激活虚拟环境

```cmd

venv\Scripts\activate.bat

```

✅激活成功标志：终端提示符前面出现 (venv)

```cmd

(venv) D:\Code\PycharmProjects\python_mysql_demo>

```

> ⚠️每次新开终端窗口，都需要重新执行激活命令。
> 
> 退出虚拟环境命令：

```cmd

deactivate

```

## 3. 在虚拟环境安装 PyMySQL

保持(venv)激活状态，安装数据库驱动库

```cmd

pip install pymysql

```

验证安装：

```cmd

pip show pymysql

```

## 4. 编写数据库连接测试代码

新建文件 test_mysql.py

```python

import pymysql

def test_conn():
    # 连接本地MySQL
    conn = pymysql.connect(
        host="127.0.0.1",
        port=3306,
        user="root",
        password="你的MySQL密码",
        charset="utf8mb4"
    )
    cur = conn.cursor()
    cur.execute("SELECT VERSION()")
    version = cur.fetchone()
    print(f"✅ MySQL连接成功！MySQL服务版本: {version[0]}")
    cur.close()
    conn.close()

if __name__ == "__main__":
    test_conn()

```

运行代码：

```cmd

python test_mysql.py

```

> 输出：✅ MySQL连接成功！MySQL服务版本: 8.0.40 代表数据库连通正常。
> 
> 【拓展 CRUD 示例】包含建库、建表、增删改查完整代码

```python

import pymysql

def demo_curd():
    conn = pymysql.connect(
        host="127.0.0.1",
        port=3306,
        user="root",
        password="你的MySQL密码",
        charset="utf8mb4"
    )
    cur = conn.cursor()

    # 创建数据库，不存在才创建
    cur.execute("CREATE DATABASE IF NOT EXISTS python_demo_db DEFAULT CHARACTER SET utf8mb4;")
    cur.execute("USE python_demo_db;")

    # 创建学生表
    create_table_sql = """
    CREATE TABLE IF NOT EXISTS student(
        id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(50) NOT NULL,
        age INT
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
    """
    cur.execute(create_table_sql)

    # 插入数据
    insert_sql = "INSERT INTO student(name, age) VALUES (%s, %s)"
    cur.execute(insert_sql, ("张三", 20))
    cur.execute(insert_sql, ("李四", 22))
    conn.commit()   # 增删改必须commit提交事务才会写入数据库

    # 查询
    cur.execute("SELECT * FROM student;")
    result = cur.fetchall()
    print("📋 查询全部学生：")
    for row in result:
        print(row)

    # 更新
    cur.execute("UPDATE student SET age=%s WHERE name=%s", (21, "张三"))
    conn.commit()

    # 删除
    cur.execute("DELETE FROM student WHERE name=%s", ("李四",))
    conn.commit()

    print("\n📋 更新删除后结果：")
    cur.execute("SELECT * FROM student;")
    print(cur.fetchall())

    cur.close()
    conn.close()

if __name__ == "__main__":
    demo_curd()

```

## 5. 配置 Git 工程配套文件

### 5.1 创建 .gitignore 文件

作用：告诉 Git 版本控制系统哪些文件 / 文件夹不要提交到仓库；虚拟环境venv体积很大，禁止上传。

Windows 无法直接图形界面创建以.开头文件，终端项目根目录执行：

```cmd

type nul > .gitignore

```

打开生成的.gitignore，粘贴下面全部内容并保存：

```gitignore

# 虚拟环境目录
venv/
env/
.venv/

# Python编译缓存文件
__pycache__/
*.pyc
*.pyo
*.pyd

# VSCode本地配置
.vscode/
!.vscode/settings.json

# 日志文件
*.log

# 系统生成垃圾文件
.DS_Store
Thumbs.db

```

### 5.2 生成依赖清单 requirements.txt

> 在虚拟环境(venv)激活状态执行，记录项目所有第三方库及版本，用于项目迁移、分享复现环境。

```cmd

pip freeze > requirements.txt

```

> 执行完成项目根目录生成requirements.txt。
> 
> 以后新增 pip 安装库，需要重新执行这条命令更新清单。

### 📌他人复原项目环境命令

拿到你的代码（不需要拷贝 venv 文件夹）：

```cmd

# 1 创建虚拟环境
python -m venv venv

# 2 激活虚拟环境
venv\Scripts\activate

# 3 根据清单一键安装全部依赖
pip install -r requirements.txt

```

---

## 6. 项目最终目录结构

```plaintext

python_mysql_demo/
├─ venv/                 # 虚拟环境，.gitignore忽略，不上传Git
├─ test_mysql.py         # Python业务代码
├─ .gitignore            # Git忽略配置文件
└─ requirements.txt      # 项目依赖清单，需要提交Git

```

## 7. 常用命令速查表

| 操作 | CMD 命令 |
|------|------|
| 创建虚拟环境 | python -m venv venv |
|激活虚拟环境 | venv\Scripts\activate.bat|
|退出虚拟环境 | deactivate|
|安装第三方库 | pip install 库名|
|导出依赖清单 | pip freeze > requirements.txt|
|复原项目依赖 | pip install -r requirements.txt|
|运行 python 脚本 | python test_mysql.py|

## 8. 常见坑点说明

1. 新开终端看不到 (venv)：每打开新终端，必须重新执行激活脚本。

2. PowerShell 报脚本禁止加载：VSCode 设置默认终端为Command Prompt（CMD），避开 PowerShell 执行策略限制。

3. .gitignore不生效：确认文件名不是.gitignore.txt，Windows 默认隐藏后缀。

4. 增删改数据库看不到效果：insert/update/delete操作之后必须写conn.commit()提交事务。

5. 不要拷贝 venv 文件夹分享项目：只分享代码、.gitignore、requirements.txt，对方本地自行生成 venv。

6. 文档可以复制保存为 项目搭建说明.md，以后新建 Python 数据库项目直接参考使用。