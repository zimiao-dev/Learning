# Backtracking（回溯）

## 一、什么是回溯？

回溯（Backtracking）本质上是一种 **DFS（深度优先搜索）**。

它通过不断尝试一种选择，当发现当前路径不满足要求时，就**撤销这一步选择（回溯）**，继续尝试其他可能。

核心思想：

```
做选择
    ↓
进入下一层递归
    ↓
撤销选择（Backtrack）
```

一句话理解：

> 回溯 = DFS + 撤销选择

---

## 二、什么时候想到回溯？

当题目要求：

- 求所有方案
- 求所有组合
- 求所有排列
- 求所有子集
- 枚举所有可能
- 能否组成某个结果

优先考虑回溯。

典型关键词：

```
All
Every
Combination
Permutation
Subset
所有可能
组合
排列
子集
```

---

## 三、回溯树思想

回溯本质是在一棵搜索树上进行 DFS。

例如：

```
        []
      /    \
    (       )
```

每进入一层递归，就是向下一层搜索。

每执行一次 `pop()`，就是回到父节点继续尝试其他分支。

理解"回溯树"比死记代码更重要。

---

## 四、回溯模板

```python
res = []
path = []

def dfs(...):

    if 满足结束条件:
        res.append(path[:])      # 注意复制
        return

    for 当前选择 in 所有可选项:

        path.append(当前选择)    # 做选择

        dfs(...)                 # 进入下一层

        path.pop()               # 撤销选择
```

记忆：

```
append

↓

dfs

↓

pop
```

---

## 五、DFS() 参数表示什么？（★★★★★）

写 DFS 前，不要急着写代码。

先问自己：

> **下一层递归需要知道哪些信息？**

这些信息，就是 DFS 的参数（递归状态）。

因此：

> **DFS 的参数表示"当前递归状态（State）"，而不是剪枝条件。**

递归进入下一层，就是状态发生了变化。

---

### 常见状态

#### ① 当前搜索位置

例如：

```python
dfs(i, j)
```

表示：

当前搜索到二维网格中的 `(i, j)`。

代表题：

- LC79 单词搜索
- 岛屿数量

---

#### ② 当前起始位置

例如：

```python
dfs(start)
```

表示：

当前从 `start` 开始选择元素。

代表题：

- LC78 子集
- LC39 组合总和

---

#### ③ 当前路径和

例如：

```python
dfs(start, path_sum)
```

表示：

当前组合已经累加到 `path_sum`。

利用该状态可以进行剪枝：

```python
if path_sum > target:
    return
```

代表题：

- LC39 组合总和

---

#### ④ 当前左右括号数量

例如：

```python
dfs(left, right)
```

表示：

当前已经使用：

- left 个左括号
- right 个右括号

利用状态进行剪枝：

```python
if left > n:
    return

if left < right:
    return
```

代表题：

- LC22 括号生成

---

#### ⑤ 当前深度

例如：

```python
dfs(depth)
```

表示：

当前递归到了第几层。

常用于：

- 树
- 图
- 回溯搜索

---

#### ⑥ 当前二维坐标 + 匹配进度

例如：

```python
dfs(row, col, index)
```

表示：

当前递归状态包括：

- row：当前所在行
- col：当前所在列
- index：当前匹配到 word 的第几个字符

代表题：

- LC79 单词搜索

---

#### ⑦ 当前处理层数

例如：

```python
dfs(row)
```

表示：

当前正在处理第 row 行。

虽然棋盘状态保存在外部变量：

```python
board
cols
diag1
diag2
```

但是 row 是递归过程中必须知道的信息。

代表题：

- LC51 N 皇后

理解：

DFS 参数不一定包含全部状态。

只需要包含：

> 下一层递归必须知道的信息。

---

### 如何确定 DFS 参数？

可以按照下面三个问题思考：

① 下一层递归需要知道什么？

↓

② 哪些信息会随着递归发生变化？

↓

③ 把这些变化的信息作为参数传递。

例如：

LC39：

```
下一层需要知道：

当前已经选到哪里？

↓

start

----------------

当前已经累加多少？

↓

path_sum
```

因此：

```python
dfs(start, path_sum)
```

---

LC22：

```
下一层需要知道：

已经用了多少左括号？

↓

left

----------------

已经用了多少右括号？

↓

right
```

因此：

```python
dfs(left, right)
```

---

### 参数 ≠ 剪枝

很多时候容易误认为：

> DFS 参数就是剪枝条件。

实际上：

DFS 参数表示的是**状态（State）**。

剪枝只是**利用这些状态进行判断**。

例如：

状态：

```python
dfs(left, right)
```

剪枝：

```python
if left > n:
    return

if left < right:
    return
```

再例如：

状态：

```python
dfs(start, path_sum)
```

剪枝：

```python
if path_sum > target:
    return
```

因此：

> **状态决定搜索过程，剪枝利用状态减少搜索。**

---

## 六、常见回溯类型

回溯题通常可以按照搜索目标进行分类。

做题时先判断：

> 我要搜索的是组合？排列？子集？还是路径？

不同类型对应不同状态设计。

---

## ① 子集（Subset）

特点：

- 每个元素都有"选 / 不选"两种状态
- 结果包含所有可能集合
- 通常不关心元素顺序

常见参数：

```python
dfs(start)
```

或者：

```python
dfs(index)
```

代表题：

- LC78 子集

核心：

```
选择当前元素

↓

继续搜索剩余元素
```

---

## ② 组合（Combination）

特点：

- 顺序不重要
- `[1,2]` 和 `[2,1]` 视为同一种
- 通常需要 start 控制搜索范围

常见参数：

```python
dfs(start)
```

代表题：

- LC39 组合总和
- LC77 组合

关键区别：

元素可以重复：

```python
dfs(i)
```

元素不能重复：

```python
dfs(i+1)
```

---

## ③ 排列（Permutation）

特点：

- 顺序重要
- `[1,2]` 和 `[2,1]` 是不同结果
- 需要记录哪些元素已经使用

常见状态：

```python
visited
```

代表题：

- LC46 全排列

模板：

```python
if visited[i]:
    continue
```

---
## ④ 多叉搜索（Multi-way Search）

特点：

- 每一层都有多个选择
- 每层选择来自当前状态对应的候选集合
- 不关注元素是否重复，而关注搜索层级

常见状态：

```python
dfs(index)
```

表示：

当前处理到第几个位置。

代表题：

- LC17 电话号码的字母组合

核心：

```
当前层选择一个选项

↓

进入下一层

↓

直到达到结束条件
```

---

## ⑤ 路径搜索（Path Search）

特点：

- 搜索空间通常是二维网格或者图
- 状态包含当前位置
- 需要记录访问过的位置

常见参数：

```python
dfs(row, col)
```

或者：

```python
dfs(node)
```

代表题：

- LC79 单词搜索

核心：

```
当前位置

↓

下一步方向搜索

↓

撤销访问状态
```

---

## ⑥ 约束满足问题（Constraint Backtracking）

特点：

- 每一步选择都会影响后续是否合法
- 需要维护额外状态快速判断冲突
- 通过剪枝减少搜索空间

代表题：

- LC22 括号生成
- LC51 N 皇后

核心：

```
选择一个状态

↓

判断是否合法

↓

继续搜索

↓

撤销状态
```

---

### LC51 N 皇后

每一层：

代表放置一行皇后。

DFS 参数：

```python
dfs(row)
```

表示：

当前准备处理第 row 行。

状态：

需要记录已经被占用的位置：

```
列

↓

cols


主对角线

↓

row - col


副对角线

↓

row + col
```

判断冲突：

```python
if col in cols 
or row-col in diag1
or row+col in diag2:
    continue
```

如果合法：

加入状态：

```python
cols.add(col)
diag1.add(row-col)
diag2.add(row+col)
```

递归：

```python
dfs(row+1)
```

回溯：

```python
remove()
```

---

## 七、常见剪枝

剪枝（Pruning）的目的：

> 尽早结束不可能得到答案的搜索。

常见方式：

### ① 超出边界

例如：

```python
if left > n:
    return
```

---

### ② 当前状态已经非法

例如：

```python
if left < right:
    return
```

因为右括号数量不能超过左括号。

---

### ③ 当前和已经超过目标

例如：

```python
if cur_sum > target:
    return
```

代表题：

- LC39 Combination Sum

---

### ④ 已满足答案

例如：

```python
if left == n and right == n:
    res.append(...)
    return
```

---

## 八、目前遇到的易错点

### ① 保存答案必须复制

错误：

```python
res.append(path)
```

正确：

```python
res.append(path[:])
```

否则 path 后续变化会影响结果。

---

### ② 一定记得回溯

```python
path.append(...)

dfs(...)

path.pop()
```

容易漏写 `pop()`。

---

### ③ start 和 visited 不要混

组合：

使用 start

排列：

使用 visited

---

### ④ dfs(i) 还是 dfs(i+1)

元素允许重复：

```python
dfs(i)
```

例如：

LC39 Combination Sum

因为当前元素还能继续选择。

---

元素不能重复：

```python
dfs(i + 1)
```

例如：

LC77 组合

---

### ⑤ 剪枝越早越好

不要等递归结束再判断是否合法。

应在递归过程中立即剪掉非法状态。

例如：

```python
if left > n:
    return

if left < right:
    return
```

这样可以减少大量无效搜索。

---

### ⑥ 修改状态后一定恢复

例如：

```python
visited[row][col] = True

...

visited[row][col] = False
```

如果提前返回或忘记恢复，

其他搜索路径就无法再次访问该位置。

这是二维回溯最容易犯的错误。

---

## 九、复杂度

通常属于指数级搜索。

时间复杂度一般：

```
O(2^n)
```

或者更高。

不用死记具体复杂度。

重点理解：

**回溯是在枚举所有可能。**

---

## 十、代表题

| 题号 | 学到什么 |
|------|----------|
| LC17 | 多叉搜索 |
| LC22 | 剪枝回溯 |
| LC39 | 元素可重复（dfs(i)） |
| LC46 | visited |
| LC51 | 约束满足问题，列和对角线判断 |
| LC78 | 子集模板 |
| LC79 | 二维网格搜索（dfs(i,j,index)） |

---

## 十一、回溯模型（★★★★★）

回溯问题本质：

> 在搜索空间中不断尝试选择，通过状态记录和剪枝减少无效搜索。

一个完整的回溯过程包含：

```
定义状态

↓

选择（Choose）

↓

递归搜索（Explore）

↓

撤销选择（Unchoose）
```

---

## 回溯三个核心问题

### ① 搜索空间是什么？

不同搜索空间决定 DFS 形式。


数组：

```python
dfs(start)
```

例如：

- LC39 组合总和


二维矩阵：

```python
dfs(row, col)
```

例如：

- LC79 单词搜索


棋盘：

```python
dfs(row)
```

例如：

- LC51 N皇后


---

### ② 当前状态是什么？

DFS 参数表示当前递归状态。

例如：

LC39：

```python
dfs(start, path_sum)
```

状态：

- 当前选择位置
- 当前路径和


LC22：

```python
dfs(left, right)
```

状态：

- 左括号数量
- 右括号数量


LC79：

```python
dfs(row, col, index)
```

状态：

- 当前坐标
- 匹配字符位置


LC51：

```python
dfs(row)
```

状态：

- 当前处理第几行


---

### ③ 如何减少无效搜索？

通过剪枝。


例如：

LC22：

```python
if left < right:
    return
```


LC39：

```python
if path_sum > target:
    return
```


LC51：

```python
if col in cols:
    continue
```


---

## 不同回溯类型对应模型

```
                    回溯

                      |
        --------------------------------
        |              |               |
     选择模型       状态设计        剪枝优化

        |              |               |

     LC39          DFS参数          LC22

     LC46          状态集合          LC51

     LC78          坐标状态          LC79

     LC17
```

---

目前理解：

回溯题不是记模板。

真正需要设计的是：

1. 搜索空间是什么？
2. 当前状态是什么？
3. 如何判断非法状态？
4. 如何撤销选择？

代码只是这个模型的实现。

---

## 十二、做题思考顺序（★★★★★）

看到一道题，先问自己：

① 是否要求所有方案？

↓

是 → 考虑回溯。

---

② 顺序是否重要？

- 是 → 排列
- 否 → 组合 / 子集

---

③ 元素是否可以重复使用？

- 可以 → dfs(i)
- 不可以 → dfs(i+1)

---

④ 是否存在非法状态？

如果有：

尽早剪枝。

---

⑤ 搜索空间是什么？

数组？

↓

dfs(start)

二维矩阵？

↓

dfs(row, col)

树？

↓

dfs(node)

图？

↓

dfs(node)

---

## 十三、我的总结

回溯不是一种固定算法，而是一种搜索思想。

真正需要掌握的是：

- 如何建树
- 如何做选择
- 如何撤销选择
- 如何剪枝

以后看到"所有可能"，优先想到回溯。

---

## 十四、知识更新日志

### 2026-07-31

#### LC39 Combination Sum

新增理解：

组合问题中：

元素可重复

↓

递归：

```python
dfs(i)
```

不是：

```python
dfs(i + 1)
```

---

#### LC22 Generate Parentheses

新增理解：

带约束条件的回溯。

合法状态：

```python
left <= n
left >= right
```

剪枝应尽早进行：

```python
if left > n:
    return

if left < right:
    return
```

不要等递归结束再判断是否合法。

---

#### 关于 DFS 参数

今天第一次意识到：

DFS() 的参数并不是随便设计的。

它表示的是**当前递归状态（State）**。

设计 DFS 时，不应该先想剪枝，而应该先问自己：

> 下一层递归需要知道哪些信息？

这些信息，就是 DFS 的参数。

剪枝则是在递归过程中，根据这些状态提前结束无效搜索。

---

### 2026-08-02

#### LC79 Word Search

新增理解：

搜索空间从数组变成二维矩阵。

DFS 参数：

```python
dfs(row, col, index)
```

其中：

- row、col 表示当前位置
- index 表示当前匹配到 word 的第几个字符

修改状态后必须恢复：

```python
visited[row][col] = True

...

visited[row][col] = False
```

否则会影响其他搜索路径。

---

#### LC51 N Queens

新增理解：

N 皇后属于：

约束满足问题（Constraint Backtracking）。

核心不是枚举，而是在搜索过程中不断判断状态是否合法。


DFS 参数：

```python
dfs(row)
```

表示当前处理第几行。


为了快速判断皇后冲突：

使用集合记录：

列：

```python
col
```

主对角线：

```python
row-col
```

副对角线：

```python
row+col
```


新增理解：

回溯问题的关键：

1. 定义当前状态
2. 判断是否合法
3. 做选择
4. 递归
5. 撤销选择