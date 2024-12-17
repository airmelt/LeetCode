# 2379 Minimum Recolors to Get K Consecutive Black Blocks 得到 K 个黑块的最少涂色次数

__Description:__

You are given a __0-indexed__ string `blocks` of length `n`, where `blocks[i]` is either `'W'` or `'B'`, representing the color of the `i ^ th` block. The characters `'W'` and `'B'` denote the colors white and black, respectively.

You are also given an integer `k`, which is the desired number of __consecutive__ black blocks.

In one operation, you can recolor a white block such that it becomes a black block.

Return _the __minimum__ number of operations needed such that there is at least __one__ occurrence of_ `k` _consecutive black blocks._

__Example:__

Example 1:

```text
Input: blocks = "WBBWWBBWBW", k = 7
Output: 3
Explanation:
One way to achieve 7 consecutive black blocks is to recolor the 0th, 3rd, and 4th blocks
so that blocks = "BBBBBBBWBW". 
It can be shown that there is no way to achieve 7 consecutive black blocks in less than 3 operations.
Therefore, we return 3.
```

Example 2:

```text
Input: blocks = "WBWBBBW", k = 2
Output: 0
Explanation:
No changes need to be made, since 2 consecutive black blocks already exist.
Therefore, we return 0.
```

__Constraints:__

- `n == blocks.length`
- `1 <= n <= 100`
- `blocks[i]` is either `'W'` or `'B'`.
- `1 <= k <= n`

__题目描述:__

给你一个长度为 `n` 下标从 __0__ 开始的字符串 `blocks` ， `blocks[i]` 要么是 `'W'` 要么是 `'B'` ，表示第 `i` 块的颜色。字符 `'W'` 和 `'B'` 分别表示白色和黑色。

给你一个整数 `k` ，表示想要 __连续__ 黑色块的数目。

每一次操作中，你可以选择一个白色块将它 涂成 黑色块。

请你返回至少出现 __一次__ 连续 `k` 个黑色块的 __最少__ 操作次数。

__示例:__

示例 1：

```text
输入：blocks = "WBBWWBBWBW", k = 7
输出：3
解释：
一种得到 7 个连续黑色块的方法是把第 0 ，3 和 4 个块涂成黑色。
得到 blocks = "BBBBBBBWBW" 。
可以证明无法用少于 3 次操作得到 7 个连续的黑块。
所以我们返回 3 。
```

示例 2：

```text
输入：blocks = "WBWBBBW", k = 2
输出：0
解释：
不需要任何操作，因为已经有 2 个连续的黑块。
所以我们返回 0 。
```

__提示：__

- `n == blocks.length`
- `1 <= n <= 100`
- `blocks[i]` 要么是 `'W'` ，要么是 `'B'` 。
- `1 <= k <= n`

__思路:__

```text
1. 暴力法
计算所有长度为 k 的子串中白块的个数, 返回最小值
时间复杂度为 O(NK), 空间复杂度为 O(1)
2. 滑动窗口
计算前 k 个块中白块的个数, 之后每次移动窗口, 更新白块的个数
这里由于 'W' 的 ASCII 码为 87, 'B' 的 ASCII 码为 66, 所以可以直接用位运算来更新白块的个数
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumRecolors(string blocks, int k) 
    {
        int c = count_if(blocks.begin(), blocks.begin() + k, [](char c){ return c =='W'; }), result = c, n = blocks.size();
        for (int i = k; i < n; i++) result = min(result, c = c + (blocks[i] & 1) - (blocks[i - k] & 1));
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumRecolors(String blocks, int k) {
        int c = (int)blocks.substring(0, k).chars().filter(ch -> ch == 'W').count(), result = c, n = blocks.length();
        for (int i = k; i < n; i++) result = Math.min(result, c = c + (blocks.charAt(i) & 1) - (blocks.charAt(i - k) & 1));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumRecolors(self, blocks: str, k: int) -> int:
        return min(blocks[i:i + k].count("W") for i in range(len(blocks) - k + 1))
```
