# 1643 Kth Smallest Instructions 第K条最小指令

__Description:__

Bob is standing at cell `(0, 0)`, and he wants to reach `destination`: `(row, column)`. He can only travel __right__ and __down__. You are going to help Bob by providing __instructions__ for him to reach `destination`.

The instructions are represented as a string, where each character is either:

- `'H'`, meaning move horizontally (go __right__), or
- `'V'`, meaning move vertically (go __down__).

Multiple __instructions__ will lead Bob to `destination`. For example, if `destination` is `(2, 3)`, both `"HHHVV"` and `"HVHVH"` are valid __instructions__.

However, Bob is very picky. Bob has a lucky number `k`, and he wants the `k ^ th` __lexicographically smallest instructions__ that will lead him to `destination`. `k` is __1-indexed__.

Given an integer array `destination` and an integer `k`, return _the_ `k ^ th` ___lexicographically smallest instructions__ that will take Bob to_ `destination`.

__Example:__

Example 1:

![1643-1](https://assets.leetcode.com/uploads/2020/10/12/ex1.png)

```text
Input: destination = [2,3], k = 1
Output: "HHHVV"
Explanation: All the instructions that reach (2, 3) in lexicographic order are as follows:
["HHHVV", "HHVHV", "HHVVH", "HVHHV", "HVHVH", "HVVHH", "VHHHV", "VHHVH", "VHVHH", "VVHHH"].
```

Example 2:

![1643-2](https://assets.leetcode.com/uploads/2020/10/12/ex2.png)

```text
Input: destination = [2,3], k = 2
Output: "HHVHV"
```

Example 3:

![1643-3](https://assets.leetcode.com/uploads/2020/10/12/ex3.png)

```text
Input: destination = [2,3], k = 3
Output: "HHVVH"
```

__Constraints:__

- `destination.length == 2`
- `1 <= row, column <= 15`
- `1 <= k <= nCr(row + column, row)`, where `nCr(a, b)` denotes `a` choose `b`​​​​​.

__题目描述:__

Bob 站在单元格 `(0, 0)` ，想要前往目的地 `destination` : `(row, column)` 。他只能向 __右__ 或向 __下__ 走。你可以为 Bob 提供导航 __指令__ 来帮助他到达目的地 `destination` 。

指令 用字符串表示，其中每个字符：

- `'H'` ，意味着水平向右移动
- `'V'` ，意味着竖直向下移动

能够为 Bob 导航到目的地 `destination` 的指令可以有多种，例如，如果目的地 `destination` 是 `(2, 3)`， `"HHHVV"` 和 `"HVHVH"` 都是有效 __指令__ 。

然而，Bob 很挑剔。因为他的幸运数字是 `k`，他想要遵循 __按字典序排列后的第 `k` 条最小指令__ 的导航前往目的地 `destination` 。 `k` 的编号 __从 1 开始__ 。

给你一个整数数组 `destination` 和一个整数 `k` ，请你返回可以为 Bob 提供前往目的地 `destination` 导航的 __按字典序排列后的第 `k` 条最小指令__ 。

__示例:__

示例 1：

![1643-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/01/ex1.png)

```text
输入：destination = [2,3], k = 1
输出："HHHVV"
解释：能前往 (2, 3) 的所有导航指令 按字典序排列后 如下所示：
["HHHVV", "HHVHV", "HHVVH", "HVHHV", "HVHVH", "HVVHH", "VHHHV", "VHHVH", "VHVHH", "VVHHH"].
```

示例 2：

![1643-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/01/ex2.png)

```text
输入：destination = [2,3], k = 2
输出："HHVHV"
```

示例 3：

![1643-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/01/ex3.png)

```text
输入：destination = [2,3], k = 3
输出："HHVVH"
```

__提示：__

- `destination.length == 2`
- `1 <= row, column <= 15`
- `1 <= k <= nCr(row + column, row)`，其中 `nCr(a, b)` 表示组合数，即从 `a` 个物品中选 `b` 个物品的不同方案数。

__思路:__

```text
组合数学
从左上角走到右下角需要 destination[0] 个 'V' 和 destination[1] 个 'H'
第 k 个即将上述 'H' 和 'V' 从小到大排序的第 k 个
一共有 C(h + v, h) 种排序方式
如果在第一位放 'H', 那么还剩下 C(h + v - 1, h - 1) 种排序方式
计算组合数可以用加法 C(n, k) = C(n - 1, k - 1) + C(n - 1, k) 递推得到
时间复杂度为 O(MN), 空间复杂度为 O(M ^ 2N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string kthSmallestPath(vector<int>& destination, int k) 
    {
        int v = destination.front(), h = destination.back(), total = h + v;
        vector<vector<int>> comb(total, vector<int>(h));
        comb.front().front() = 1;
        for (int i = 1; i < total; i++)
        {
            comb[i].front() = 1;
            for (int j = 1; j <= i and j < h; j++) comb[i][j] = comb[i - 1][j - 1] + comb[i - 1][j];
        }
        string result;
        for (int i = 0; i < total; i++)
        {
            if (h and k <= comb[h + v - 1][h - 1])
            {
                result += 'H';
                --h;
            }
            else 
            {
                result += 'V';
                k -= h ? comb[h + v-- - 1][h - 1] : 0;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String kthSmallestPath(int[] destination, int k) {
        int v = destination[0], h = destination[1], total = h + v, comb[][] = new int[total][h];
        comb[0][0] = 1;
        for (int i = 1; i < total; i++) {
            comb[i][0] = 1;
            for (int j = 1; j <= i && j < h; j++) comb[i][j] = comb[i - 1][j - 1] + comb[i - 1][j];
        }
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < total; i++) {
            if (h > 0 && k <= comb[h + v - 1][h - 1]) {
                result.append('H');
                --h;
            }
            else {
                result.append('V');
                k -= h > 0 ? comb[h + v-- - 1][h - 1] : 0;
            }
        }
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def kthSmallestPath(self, destination: List[int], k: int) -> str:
        (v, h), result = destination, ""
        for _ in range(h + v):
            if not v or comb(h + v - 1, v) >= k:
                h -= 1
                result += "H"
            else:
                k -= comb(h + v - 1, v)
                v -= 1
                result += "V"
        return result
```
