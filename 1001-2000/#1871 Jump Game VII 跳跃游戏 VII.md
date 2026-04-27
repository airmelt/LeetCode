# 1871 Jump Game VII 跳跃游戏 VII

__Description:__

You are given a __0-indexed__ binary string `s` and two integers `minJump` and `maxJump`. In the beginning, you are standing at index `0`, which is equal to `'0'`. You can move from index `i` to index `j` if the following conditions are fulfilled:

- `i + minJump <= j <= min(i + maxJump, s.length - 1)`, and
- `s[j] == '0'`.

Return `true` _if you can reach index_ `s.length - 1` _in_ `s`_, or_ `false` _otherwise._

__Example:__

Example 1:

```text
Input: s = "011010", minJump = 2, maxJump = 3
Output: true
Explanation:
In the first step, move from index 0 to index 3. 
In the second step, move from index 3 to index 5.
```

Example 2:

```text
Input: s = "01101110", minJump = 2, maxJump = 3
Output: false
```

__Constraints:__

- `2 <= s.length <= 10 ^ 5`
- `s[i]` is either `'0'` or `'1'`.
- `s[0] == '0'`
- `1 <= minJump <= maxJump < s.length`

__题目描述:__

给你一个下标从 __0__ 开始的二进制字符串 `s` 和两个整数 `minJump` 和 `maxJump` 。一开始，你在下标 `0` 处，且该位置的值一定为 `'0'` 。当同时满足如下条件时，你可以从下标 `i` 移动到下标 `j` 处:

- `i + minJump <= j <= min(i + maxJump, s.length - 1)` 且
- `s[j] == '0'`.

如果你可以到达 `s` 的下标 __`s.length - 1` 处，请你返回 `true` ，否则返回 `false` 。

__示例:__

示例 1：

```text
输入：s = "011010", minJump = 2, maxJump = 3
输出：true
解释：
第一步，从下标 0 移动到下标 3 。
第二步，从下标 3 移动到下标 5 。
```

示例 2：

```text
输入：s = "01101110", minJump = 2, maxJump = 3
输出：false
```

__提示：__

- `2 <= s.length <= 10 ^ 5`
- `s[i]` 要么是 `'0'` ，要么是 `'1'`
- `s[0] == '0'`
- `1 <= minJump <= maxJump < s.length`

__思路:__

```text
1. 模拟
将 [minJump, maxJump] 当作滑动窗口
遍历字符串每次遍历到能够跳跃过来的点就将滑动窗口中的所有值更新成能够跳跃的点
最后检查能否到达 s 的末尾
时间复杂度为 O(NM), 空间复杂度为 O(1), M 为 maxJump - minJump
2. 前缀和 ➕ 差分数组
思路 1 中大量更新非常耗时
可以使用差分数组来优化
记录 [minJump, maxJump] 区间内的值为 1, 其他位置为 0
所以 diff[i + minJump] + 1, diff[i + maxJump + 1] - 1
每次更新时将 diff[i - 1] 累加到 diff[i] 上
最后检查 diff[n - 1] 是否大于 0 即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canReach(string s, int minJump, int maxJump) 
    {
        int n = s.size(), limit = 0;
        if (s.back() != '0') return false;
        s.front() = '2';
        for (int i = 0; i < n; i++) if (s[i] == '2') for (limit = max(limit, i + minJump); limit <= min(n - 1, i + maxJump); limit++) if (s[limit] == '0') s[limit] = '2';
        return s.back() == '2';
    }
};

```

__Java__:

```Java
class Solution {
    public boolean canReach(String s, int minJump, int maxJump) {
        int n = s.length();
        if (s.charAt(n - 1) == '1') return false;
        Map<Integer, Integer> diff = new HashMap<>();
        for (int i = 0; i < n; i++) {
            diff.put(i, diff.getOrDefault(i, 0) + diff.getOrDefault(i - 1, 0));
            if (s.charAt(i) == '0' && (i == 0 || diff.getOrDefault(i, 0) > 0)) {
                diff.merge(i + minJump, 1, Integer::sum);
                diff.merge(i + maxJump + 1, -1, Integer::sum);
            }
        }
        return diff.get(n - 1) > 0;
    }
}
```

__Python__:

```Python
class Solution:
    def canReach(self, s: str, minJump: int, maxJump: int) -> bool:
        if s[-1] == '1':
            return False
        diff, n = defaultdict(int), len(s)
        for i, c in enumerate(s):
            diff[i] += diff[i - 1]
            if c == '0' and (not i or diff[i] > 0):
                diff[i + minJump] += 1
                diff[i + maxJump + 1] -= 1
        return diff[n - 1] > 0
```
