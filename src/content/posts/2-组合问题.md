---
title: 回溯算法2:组合
published: 2024-06-03
tags: [回溯, 算法, cpp]
category: 算法
draft: true
---

# 组合

给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。

示例: 输入: n = 4, k = 2 输出: [ [2,4], [3,4], [2,3], [1,2], [1,3], [1,4], ]

# 思路

最简单的做法；两层for循环。

--> 如果n为100，k为50呢，那就50层for循环 。。。

虽然想暴力搜索，但是用for循环嵌套连暴力都写不出来！

回溯法就用递归来解决嵌套层数的问题。

递归来做层叠嵌套（可以理解是开k层for循环），每一次的递归中嵌套一个for循环，那么递归就可以用于解决多层嵌套循环的问题了。

![树形结构](https://code-thinking-1253855093.file.myqcloud.com/pics/20201123195223940.png)

可以看出这棵树，一开始集合是 1，2，3，4， 从左向右取数，取过的数，不再重复取。

第一次取1，集合变为2，3，4 ，因为k为2，我们只需要再取一个数就可以了，分别取2，3，4，得到集合[1,2] [1,3] [1,4]，以此类推。

每次从集合中选取元素，可选择的范围随着选择的进行而收缩，调整可选择的范围。

图中可以发现n相当于树的宽度，k相当于树的深度。

那么如何在这个树上遍历，然后收集到我们要的结果集呢？

图中每次搜索到了叶子节点，我们就找到了一个结果。

相当于只需要把达到叶子节点的结果收集起来，就可以求得 n个数中k个数的组合集合。

# 代码

```c++
class Solution {
private:
    vector<vector<int>> result;     // 存放符合条件结果的集合
    vector<int> path;               // 存放符合条件的结果

    void backtracking(int n, int k, int startIndex) {
        if (path.size() == k) {
            result.push_back(pah);
            return;
        }
        for (int i = startIndex; i <= n; i++) {
            path.push_back(i);      // 处理节点
            backtracking(n, k, i + 1); // 递归
            path.pop_back();        // 回溯，撤销处理的节点
        }
    }
public:
    vector<vector<int>> combine(int n, int k) {
        backtracking(n, k, 1);
        return result;
    }
};
```

- 时间复杂度: O(n * 2^n)
- 空间复杂度: O(n)

# 剪枝优化

遍历范围可以剪枝优化。

来举一个例子，n = 4，k = 4的话，那么第一层for循环的时候，从元素2开始的遍历都没有意义了。 在第二层for循环，从元素3开始的遍历都没有意义了。

就是说 `(n - i + 1) >= k - path.size()`, 即 `i <= n - (k - path.size()) + 1`。

所以优化之后的for循环是：`for (int i = startIndex; i <= n - (k - path.size()) + 1; i++) // i为本次搜索的起始位置`

整体代码：

```c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(int n, int k, int startIndex) {
        if (path.size() == k) {
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i <= n - (k - path.size()) + 1; i++) { // 优化的地方
            path.push_back(i); // 处理节点
            backtracking(n, k, i + 1);
            path.pop_back(); // 回溯，撤销处理的节点
        }
    }
    
public:
    vector<vector<int>> combine(int n, int k) {
        backtracking(n, k, 1);
        return result;
    }
};
```