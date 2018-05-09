---
title: 算法题 Edit Distance
date: 2018-05-06 14:35:52
---

之前阿里菜鸟的在线编程题，当时只是有个一个思路但是没有实现出来，今天在lintcode上又看到了，那就记录一下吧。

题目如下：

给定两个字符串word1和word2,求出由一个变化到另外一个所需的最小步数，所允许的操作有：

- 插入一个字符
- 删除一个字符
- 替换一个字符

<!--more-->

[lintcode链接](https://www.lintcode.com/problem/edit-distance/description)

解题思路：

动态规划

（说实话，动态规划我并不是很熟悉，我对它的印象类似于递归。经常在这种看上去比较麻烦的编程题用到动态规划的思想，没办法，好好学吧。）

动态规划三要素：转移方程，边界条件，最优子结构

用dp[i][j]表示word1[0,i-1]转化到word2[0,j-1]的步骤，转移方程有两种情况：
- word1[i] == word2[j],则dp[i][j] = dp[i-1][j-1]
- word1[i] != word2[j],则分以上三种情况考虑：
  + 插入，dp[i][j] = dp[i-1][j] + 1
  + 删除，dp[i][j] = dp[i][j-1] + 1
  + 替换，dp[i][j] = dp[i-1][j-1] + 1

python代码如下：
```python

class Solution:
    def editDistince(self, word1, word2):
        m = len(word1)
        n = len(word2)
        # (m+1)*(n+1)二维矩阵，根据定义dp[m+1][n+1]为初始状态，即word1的全部长度转化成word2的全部长度
        dp = [[0 for i in range(n+1)] for j in range(m+1)]

        # 此处即为边界条件，将一个空Stirng（长度为0）转化成另一个长度为i的String所需要的步数自然为i（即i次插入操作）
        for i in range(n+1):
          dp[0][i] = i
        for j in range(m+1):
          dp[j][0] = j

        for i in range(1, m+1):
          for j in range(1, n+1):
            if word1[i-1] == word2[j-1]:
              dp[i][j] = dp[i-1][j-1]
            else:
              dp[i][j] = min(
              dp[i-1][j] + 1
              , dp[i][j-1] + 1
              , dp[i-1][j-1] +1)
        return dp[m][n]

```
