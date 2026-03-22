# 2103 Rings and Rods 环和杆

__Description:__

There are `n` rings and each ring is either red, green, or blue. The rings are distributed __across ten rods__ labeled from `0` to `9`.

You are given a string `rings` of length `2n` that describes the `n` rings that are placed onto the rods. Every two characters in `rings` forms a __color-position pair__ that is used to describe each ring where:

- The __first__ character of the `i ^ th` pair denotes the `i ^ th` ring's __color__ ( `'R'`, `'G'`, `'B'`).
- The __second__ character of the `i ^ th` pair denotes the __rod__ that the `i ^ th` ring is placed on ( `'0'` to `'9'`).

For example, `"R3G2B1"` describes `n == 3` rings: a red ring placed onto the rod labeled 3, a green ring placed onto the rod labeled 2, and a blue ring placed onto the rod labeled 1.

Return the number of rods that have all three colors of rings on them.

__Example:__

Example 1:

![2103-1](https://assets.leetcode.com/uploads/2021/11/23/ex1final.png)

```text
Input: rings = "B0B6G0R6R0R6G9"
Output: 1
Explanation: 
```

- The rod labeled 0 holds 3 rings with all colors: red, green, and blue.
- The rod labeled 6 holds 3 rings, but it only has red and blue.
- The rod labeled 9 holds only a green ring.

Thus, the number of rods with all three colors is 1.

Example 2:

![2103-2](https://assets.leetcode.com/uploads/2021/11/23/ex2final.png)

```text
Input: rings = "B0R0G0R9R0B0G0"
Output: 1
Explanation: 
```

- The rod labeled 0 holds 6 rings with all colors: red, green, and blue.
- The rod labeled 9 holds only a red ring.

Thus, the number of rods with all three colors is 1.

Example 3:

```text
Input: rings = "G4"
Output: 0
Explanation: 
Only one ring is given. Thus, no rods have all three colors.
```

__Constraints:__

- `rings.length == 2 * n`
- `1 <= n <= 100`
- `rings[i]` where `i` is __even__ is either `'R'`, `'G'`, or `'B'` (__0-indexed__).
- `rings[i]` where `i` is __odd__ is a digit from `'0'` to `'9'` (__0-indexed__).

__题目描述:__

总计有 `n` 个环，环的颜色可以是红、绿、蓝中的一种。这些环分别穿在 10 根编号为 `0` 到 `9` 的杆上。

给你一个长度为 `2n` 的字符串 `rings` ，表示这 `n` 个环在杆上的分布。 `rings` 中每两个字符形成一个 __颜色位置对__ ，用于描述每个环:

- 第 `i` 对中的 __第一个__ 字符表示第 `i` 个环的 __颜色__（ `'R'`、 `'G'`、 `'B'`）。
- 第 `i` 对中的 __第二个__ 字符表示第 `i` 个环的 __位置__，也就是位于哪根杆上（ `'0'` 到 `'9'`）。

例如， `"R3G2B1"` 表示:共有 `n == 3` 个环，红色的环在编号为 3 的杆上，绿色的环在编号为 2 的杆上，蓝色的环在编号为 1 的杆上。

找出所有集齐 全部三种颜色 环的杆，并返回这种杆的数量。

__示例:__

示例 1：

![2103-3](https://assets.leetcode.com/uploads/2021/11/23/ex1final.png)

```text
输入：rings = "B0B6G0R6R0R6G9"
输出：1
解释：
```

- 编号 0 的杆上有 3 个环，集齐全部颜色：红、绿、蓝。
- 编号 6 的杆上有 3 个环，但只有红、蓝两种颜色。
- 编号 9 的杆上只有 1 个绿色环。

因此，集齐全部三种颜色环的杆的数目为 1 。

示例 2：

![2103-4](https://assets.leetcode.com/uploads/2021/11/23/ex2final.png)

```text
输入：rings = "B0R0G0R9R0B0G0"
输出：1
解释：
```

- 编号 0 的杆上有 6 个环，集齐全部颜色：红、绿、蓝。
- 编号 9 的杆上只有 1 个红色环。

因此，集齐全部三种颜色环的杆的数目为 1 。

示例 3：

```text
输入：rings = "G4"
输出：0
解释：
只给了一个环，因此，不存在集齐全部三种颜色环的杆。
```

__提示：__

- `rings.length == 2 * n`
- `1 <= n <= 100`
- 如 `i` 是 __偶数__ ，则 `rings[i]` 的值可以取 `'R'`、 `'G'` 或 `'B'`（下标从 __0__ 开始计数）
- 如 `i` 是 __奇数__ ，则 `rings[i]` 的值可以取 `'0'` 到 `'9'` 中的一个数字（下标从 __0__ 开始计数）

__思路:__

```text
位运算
统计 RGB 都有的杆
可以用或运算统计不同的颜色
每个有的杆只统计一次
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countPoints(string rings) 
    {
        int d['Z']{['R'] = 0x1, ['G'] = 0x10, ['B'] = 0x100}, mask[10]{};
        for (int i = 0, n = rings.size(); i < n; i += 2) mask[rings[i + 1] - '0'] |= d[rings[i]];
        return count(mask, mask + 10, 0x111);
    }
};
```

__Java__:

```Java
class Solution {
    private static int FULL = (1 << 'R' - 'A') | (1 << 'G' - 'A') | (1 << 'B' - 'A');
    
    public int countPoints(String rings) {
        int result = 0, poles[] = new int[10], pole = 0;
        for (int i = 0; i < rings.length(); i += 2) {
            poles[pole = rings.charAt(i + 1) - '0'] |= 1 << (rings.charAt(i) - 'A');
            result += poles[pole] == FULL ? 1 : 0;
            if (poles[pole] == FULL) poles[pole] |= 1;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countPoints(self, rings: str) -> int:
        return sum(all(c + str(i) in rings for c in 'RGB') for i in range(10))
```
