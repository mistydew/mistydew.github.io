---
layout: post
title:  "LeetCode 36. 有效的数独（中等）"
date:   2020-08-08 13:13:10 +0800
author: Coshin
comments: true
category: 力扣题解
tags: LeetCode Medium Hash-Table
excerpt:
  判断一个 `9x9` 的数独是否有效。
  只需**根据以下规则**验证已填入的数字：<br>
  1. 每一行必须包含数字 `1-9` 且不能重复。<br>
  2. 每一列必须包含数字 `1-9` 且不能重复。<br>
  3. 每个 `3x3` 的子网格必须包含数字 `1-9` 且不能重复。<br>
  **注意：**<br>
  * 数独（部分填充）可能有效，但不一定可解。<br>
  * 只有填充的单元格需要根据上述规则进行验证。
---
> ## 36. Valid Sudoku
> 
> Determine if a `9x9` Sudoku board is valid. Only the filled cells need to be
> validated **according to the following rules**:
> 
> 1. Each row must contain the digits `1-9` without repetition.
> 2. Each column must contain the digits `1-9` without repetition.
> 3. Each of the nine `3x3` sub-boxes of the grid must contain the digits `1-9`
> without repetition.
> 
> **Note:**
> 
> * A Sudoku board (partially filled) could be valid but is not necessarily
> solvable.
> * Only the filled cells need to be validated according to the mentioned rules.
> 
> **Example 1:**
> 
> <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png" style="height:250px; width:250px">
> 
> <pre>
> <strong>Input:</strong>
> [
>   ["5","3",".",".","7",".",".",".","."],
>   ["6",".",".","1","9","5",".",".","."],
>   [".","9","8",".",".",".",".","6","."],
>   ["8",".",".",".","6",".",".",".","3"],
>   ["4",".",".","8",".","3",".",".","1"],
>   ["7",".",".",".","2",".",".",".","6"],
>   [".","6",".",".",".",".","2","8","."],
>   [".",".",".","4","1","9",".",".","5"],
>   [".",".",".",".","8",".",".","7","9"]
> ]
> <strong>Output:</strong> true
> </pre>
> 
> **Example 2:**
> 
> <pre>
> <strong>Input:</strong>
> [
>   ["8","3",".",".","7",".",".",".","."],
>   ["6",".",".","1","9","5",".",".","."],
>   [".","9","8",".",".",".",".","6","."],
>   ["8",".",".",".","6",".",".",".","3"],
>   ["4",".",".","8",".","3",".",".","1"],
>   ["7",".",".",".","2",".",".",".","6"],
>   [".","6",".",".",".",".","2","8","."],
>   [".",".",".","4","1","9",".",".","5"],
>   [".",".",".",".","8",".",".","7","9"]
> ]
> <strong>Output:</strong> false
> <strong>Explanation:</strong> Same as Example 1, except with the <strong>5</strong> in the top left corner being 
>     modified to <strong>8</strong>. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
> </pre>
> 
> **Note:**
> 
> * A Sudoku board (partially filled) could be valid but is not necessarily
> solvable.
> * Only the filled cells need to be validated according to the mentioned rules.
> * The given board contain only digits `1-9` and the character `'.'`.
> * The given board size is always `9x9`.

## 解决方案

### 方法一：一次迭代

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        unordered_map<char, int> rows[9], colums[9], boxes[9];
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                char num = board[i][j];
                if (num != '.') {
                    int box_idx = (i / 3) * 3 + j / 3;
                    rows[i][num]++;
                    colums[j][num]++;
                    boxes[box_idx][num]++;
                    if (rows[i][num] > 1 || colums[j][num] > 1 || boxes[box_idx][num] > 1)
                        return false;
                }
            }
        }
        return true;
    }
};
```

复杂度分析：
* 时间复杂度：*O*(1)。
  只迭代了 81 个单元格一遍。
* 空间复杂度：*O*(1)。

## 参考链接

* [Valid Sudoku - LeetCode](https://leetcode.com/problems/valid-sudoku/){:target="_blank"}
