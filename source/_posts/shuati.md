---
title: 刷题总结
date: 2022-01-24 21:54:46
tags: LeetCode
---

目前进展98题 2022.1.24
目前进展139题 2022.2.13

### 数组

1. 二分查找

``` python3
while left < right:
    middle = (left + right) //2
    if target > nums[middle]:
        left = middle + 1
    elif target < nums[middle]:
        right = middle - 1
    else:
        return middle
```

2. 快慢指针
3. 滑动窗口
滑动窗口一般用来解决连续子序列问题，注意目标解是连续子序列，非连续问题需要考虑使用动态规划求解

``` python3
for i in range(len(nums)):
    sum_num += nums[i]
    while sum_num >= target:
        sub = i - left + 1
        res = min(res, sub)
        sum_num -= nums[left]
        left += 1
```

### 链表

1. 虚拟头节点

``` python3
dummy = ListNode(0)
dummy.next = head
cur = dummy
...
return dummy.next
```
    
2. 反转链表

``` python3
pre = None
cur = Head

temp = cur.next
cur.next = pre

pre = cur
cur = temp
```

3. 环形链表

``` python3
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
    if slow == fast:
        p = head
        q = slow
        while p != q:
            p = p.next
            q = q.next
        return p
return None
```

### 哈希表

* 数组：
* set：
* map：要维护红黑树或者符号表，而且还要做哈希函数的运算

### 单调栈

1. 查找滑动窗口内的最大值

```python3
win, ret = [], []
for i, v in enumerate(nums):
    if i >= k and win[0] <= i - k:
        win.pop(0)
    while win and nums[win[-1]] <= v:
        win.pop()
    win.append(i)
    if i >= k - 1:
        ret.append(nums[win[0]])
return ret
```    
在使用单调栈的时候首先要明确如下几点：

单调栈里存放的元素是什么？
单调栈里只需要存放元素的下标i就可以了，如果需要使用对应的元素，直接T[i]就可以获取。

单调栈里元素是递增呢？ 还是递减呢？

### 字符串

KMP：KMP的主要思想是当出现字符串不匹配时，可以知道一部分之前已经匹配的文本内容，可以利用这些信息避免从头再去做匹配了
    
前缀表：起始位置到下标i之前（包括i）的子串中，有多大长度的相同前缀后缀

`str.count('#')` 统计#个数

```python3
"""生成next数组
def getnext(self,a,needle):
    next=['' for i in range(a)]
    j,k=0,-1
    next[0]=k
    while(j < len(needle)-1):
        if k==-1 or needle[k]==needle[j]:
            k+=1
            j+=1
            next[j]=k
        else:
            k=next[k]
    return next
"""匹配字符串
a=len(needle)
b=len(haystack)
if a==0:
    return 0
i=j=0
next=self.getnext(a,needle)
while(i<b and j<a):
    if j==-1 or needle[j]==haystack[i]:
        i+=1
        j+=1
    else:
        j=next[j]
if j==a:
    return i-j
else:
    return -1
```

### 二叉树

深度遍历、广度遍历和回溯一般用来解决路径搜索问题，深度遍历和广度遍历大家熟悉一种即可，一般来说，基于辅助队列的广度优先搜索在算法性能上比递归的深度优先搜索效率更高
    
1. 前序遍历、中序遍历、后序遍历 -- 递归法
    
```python3
def inorderTraversal(self, root: TreeNode) -> List[int]:
    result = []

    def traversal(root: TreeNode):
        if root == None:
            return
        traversal(root.left)    # 左
        result.append(root.val) # 中序
        traversal(root.right)   # 右

    traversal(root)
    return result
```

2. 层序遍历（广度优先遍历）-- 迭代法

```python3
def averageOfLevels(self, root: Optional[TreeNode]) -> List[float]:
    res = []
    stack = [root]
    while stack:
        mean = 0
        count = 0
        for _ in range(len(stack)):
            node = stack.pop(0)
            count += 1
            mean += node.val
            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)
        res.append(mean / count)
    return res
```

总结：
1. 涉及到二叉树的构造，无论普通二叉树还是二叉搜索树一定前序，都是先构造中节点。
2. 求普通二叉树的属性，一般是后序，一般要通过递归函数的返回值做计算
3. 求二叉搜索树的属性，一定是中序了，要不白瞎了有序性了

### 回溯算法

回溯法常用于搜索，判断是否有解。本质是穷举，穷举所有可能，然后选出我们想要的答案。和广度优先搜索适用的场景不同，回溯一般用于判断是否有解，广深法用于求最优解 

```python3
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

### 贪心算法

贪心的本质是选择每一阶段的局部最优，从而达到全局最优。贪心没有状态推导，而是从局部直接选最优的。

    将问题分解为若干个子问题
    找出适合的贪心策略
    求解每一个子问题的最优解
    将局部最优解堆叠成全局最优解、

贪心没套路，就刷题而言，如果感觉好像局部最优可以推出全局最优，然后想不到反例，那就试一试贪心吧！

模拟数组循环一圈

``` python3
while (rest > 0 && index != i) { // 模拟以i为起点行驶一圈
    rest += gas[index] - cost[index];
    index = (index + 1) % cost.size();
}
```

### 动态规划

动态规划中每一个状态一定是由上一个状态推导出来的

    模板：
    1. 确定dp数组（dp table）以及下标的含义
    2. 确定递推公式
    3. dp数组如何初始化
    4. 确定遍历顺序
    5. 举例推导dp数组


1. 1背包

01背包的递推公式为：

``` python3
dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
```

如果使用一维dp数组，物品遍历的for循环放在外层，遍历背包的for循环放在内层，且内层for循环**倒序遍历**！

在求装满背包有几种方法的情况下，dp[0] 初始化为1，递推公式一般为

``` python3
dp[j] += dp[j - nums[i]];
```

2. 完全背包

物品数量无限多

背包容量从小到大去遍历

如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。需要判断背包空间大小是否容得下当前物品

3. 多重背包

可以在循环内增加单类物品个数的循环，转化为01背包问题

总结：

- 问能否能装满背包（或者最多装多少）：dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]); 
- 问装满背包有几种方法：dp[j] += dp[j - nums[i]] 
- 问背包装满最大价值：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]); 
- 问装满背包所有物品的最小个数：dp[j] = min(dp[j - coins[i]] + 1, dp[j]); 

4. 打家劫舍

5. 买卖股票

分析清楚当前有几种状态
将买、卖、冷冻，降维成两个维度：持有股票和未持有股票
可以通过手写，确定下逻辑关系

6. 子序列问题