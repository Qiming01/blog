---
title: 回溯算法3:组合总和III
published: 2024-06-03
tags: [回溯, 算法, cpp]
category: 算法
draft: true
---

# 组合总和III

[题目216](https://leetcode.cn/problems/combination-sum-iii/)

找出所有相加之和为 n 的 k 个数的组合。组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。

说明：

- 所有数字都是正整数。
- 解集不能包含重复的组合。

示例 1: 输入: k = 3, n = 7 输出: [[1,2,4]]

示例 2: 输入: k = 3, n = 9 输出: [[1,2,6], [1,3,5], [2,3,4]]

# 思路

最简单的做法是在上面的最简单的组合问题的判断结束条件中修改一下，判断和是否为n即可。

--> 计算和会有额外开销。

解决：将部分和直接作为递归的参数。

终止条件：如果此时 `path.size == k` 就终止，若此时 `sum == n` 就将结果添加到result中。

# 代码

```c++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(int n, int k, int sum, int startIndex) {
        if (sum > n) {  // 部分和已经超过了n，就没有必要继续了
            return;
        }
        if (path.size() == k) {
            if (sum == n) {
                result.push_back(path);
            }
            return;
        }
        for (int i = startIndex; i <= 9 - (k - path.size()) + 1; i++) {
            sum += i;
            path.push_back(i);
            backtracking(n, k, sum, i + 1);
            sum -= i;
            path.pop_back();
        }
    }

public:
    vector<vector<int>> combinationSum3(int k, int n) {
        result.clear();
        path.clear();
        backtracking(n, k, 0, 1);
        return result;
    }
};
```