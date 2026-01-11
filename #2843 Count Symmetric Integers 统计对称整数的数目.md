# 2843 Count Symmetric Integers 统计对称整数的数目

__Description:__

You are given two positive integers `low` and `high`.

An integer `x` consisting of `2 * n` digits is __symmetric__ if the sum of the first `n` digits of `x` is equal to the sum of the last `n` digits of `x`. Numbers with an odd number of digits are never symmetric.

Return _the __number of symmetric__ integers in the range_ `[low, high]`.

__Example:__

Example 1:

```text
Input: low = 1, high = 100
Output: 9
Explanation: There are 9 symmetric integers between 1 and 100: 11, 22, 33, 44, 55, 66, 77, 88, and 99.
```

Example 2:

```text
Input: low = 1200, high = 1230
Output: 4
Explanation: There are 4 symmetric integers between 1200 and 1230: 1203, 1212, 1221, and 1230.
```

__Constraints:__

- `1 <= low <= high <= 10 ^ 4`

__题目描述:__

给你两个正整数 `low` 和 `high` 。

对于一个由 `2 * n` 位数字组成的整数 `x` ，如果其前 `n` 位数字之和与后 `n` 位数字之和相等，则认为这个数字是一个对称整数。

返回在 `[low, high]` 范围内的 __对称整数的数目__ 。

__示例:__

示例 1：

```text
输入：low = 1, high = 100
输出：9
解释：在 1 到 100 范围内共有 9 个对称整数：11、22、33、44、55、66、77、88 和 99 。
```

示例 2：

```text
输入：low = 1200, high = 1230
输出：4
解释：在 1200 到 1230 范围内共有 4 个对称整数：1203、1212、1221 和 1230 。
```

__提示：__

- `1 <= low <= high <= 10 ^ 4`

__思路:__

```text
1. 模拟
计算每一个数字
如果是偶数长度
就比较前一半之和是否等于后一半
时间复杂度为 O((H - L)log(H)), 空间复杂度为 O(log(H)), H 和 L 分别为 high 和 low 的值
2. 数位 DP
dfs(start, left, diff, limit_low, limit_high)
从 start 开始
如果 left 比一半要小那就计入正值, 否则计入负值
diff 记录前半和后半的差值
合理的构造需要 diff 为 0
limit 表示是否受到上下界约束
如果没填数字, 这里可以用 left 初始值为 -1 判断, 且剩余位置为奇数就不能填数字, 直接进入下一轮
此时如果到达下界, 但是不合法, 直接返回 0
其他情况直接枚举能填入的数字
如果 diff < 0, 可以提前终止, 因为到右边已经不能再加上正值了
最后 start 到字符串结尾时, 判断是否合法
递归入口为 dfs(0, -1, 0, true, true)
从第 0 位开始递归, 一开始没填数字, 差值初始化为 0, 要受到上下限约束
时间复杂度为 O(N ^ 3), 空间复杂度为 O(N ^ 3)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countSymmetricIntegers(int low, int high) 
    {
        string l = to_string(low), h = to_string(high);
        int n = h.size(), result = 0, d = n - l.size();
        vector memo(n, vector(d + 1, vector<int>((n >> 1) * 9 + 1, -1)));
        auto dfs = [&](this auto&& dfs, int start, int left, int diff, bool limit_low, bool limit_high) -> int 
        {
            if (diff < 0) return 0;
            if (start == n) return diff == 0;
            if (left != -1 and !limit_low and !limit_high and memo[start][left][diff] != -1) return memo[start][left][diff];
            int lo = limit_low and start >= d ? l[start - d] - '0' : 0, hi = limit_high ? h[start] - '0' : 9, ans = 0;
            if (left < 0 and ((n - start) & 1)) return lo > 0 ? 0 : dfs(start + 1, left, diff, true, false);
            for (int c = lo; c <= hi; c++) ans += dfs(start + 1, left < 0 and c ? start : left, diff + ((left < 0 or start < ((left + n) >> 1)) ? c : -c), limit_low and c == lo, limit_high and c == hi);
            return (left != -1 and !limit_low and !limit_high) ? (memo[start][left][diff] = ans) : ans;
        };
        return dfs(0, -1, 0, true, true);
    }
};
```

__Java__:

```Java
class Solution {
    public int countSymmetricIntegers(int low, int high) {
        int result = 0;
        for (int i = low, n = Integer.toString(i).toCharArray().length, diff = 0; i <= high; n = Integer.toString(++i).toCharArray().length, diff = 0) {
            if ((n & 1) == 0) {
                char[] s = Integer.toString(i).toCharArray();
                for (int j = 0; j < n; j++) if (j < (n >> 1)) diff += s[j]; else diff -= s[j];
                if (diff == 0) ++result;
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countSymmetricIntegers(self, low: int, high: int) -> int:
        return sum(not (len(str(i)) & 1) and sum(map(int, str(i)[:len(str(i)) // 2])) == sum(map(int, str(i)[len(str(i)) // 2:])) for i in range(low, high + 1))
```
