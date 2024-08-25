# 2222 Number of Ways to Select Buildings 选择建筑的方案数

__Description:__

You are given a __0-indexed__ binary string `s` which represents the types of buildings along a street where:

- `s[i] = '0'` denotes that the `i ^ th` building is an office and
- `s[i] = '1'` denotes that the `i ^ th` building is a restaurant.

As a city official, you would like to select 3 buildings for random inspection. However, to ensure variety, no two consecutive buildings out of the selected buildings can be of the same type.

- For example, given `s = "001101"`, we cannot select the `1 ^ st`, `3 ^ rd`, and `5 ^ th` buildings as that would form `"011"` which is __not__ allowed due to having two consecutive buildings of the same type.

Return the number of valid ways to select 3 buildings.

__Example:__

Example 1:

```text
Input: s = "001101"
Output: 6
Explanation: 
The following sets of indices selected are valid:
```

- [0,2,4] from "001101" forms "010"
- [0,3,4] from "001101" forms "010"
- [1,2,4] from "001101" forms "010"
- [1,3,4] from "001101" forms "010"
- [2,4,5] from "001101" forms "101"
- [3,4,5] from "001101" forms "101"

No other selection is valid. Thus, there are 6 total ways.

Example 2:

```text
Input: s = "11100"
Output: 0
Explanation: It can be shown that there are no valid selections.
```

__Constraints:__

- `3 <= s.length <= 10 ^ 5`
- `s[i]` is either `'0'` or `'1'`.

__题目描述:__

给你一个下标从 __0__ 开始的二进制字符串 `s` ，它表示一条街沿途的建筑类型，其中:

- `s[i] = '0'` 表示第 `i` 栋建筑是一栋办公楼，
- `s[i] = '1'` 表示第 `i` 栋建筑是一间餐厅。

作为市政厅的官员，你需要随机 选择 3 栋建筑。然而，为了确保多样性，选出来的 3 栋建筑 相邻 的两栋不能是同一类型。

- 比方说，给你 `s = "001101"` ，我们不能选择第 `1` ， `3` 和 `5` 栋建筑，因为得到的子序列是 `"011"` ，有相邻两栋建筑是同一类型，所以 __不合__ 题意。

请你返回可以选择 3 栋建筑的 有效方案数 。

__示例:__

示例 1：

```text
输入：s = "001101"
输出：6
解释：
以下下标集合是合法的：
```

- [0,2,4] ，从 "001101" 得到 "010"
- [0,3,4] ，从 "001101" 得到 "010"
- [1,2,4] ，从 "001101" 得到 "010"
- [1,3,4] ，从 "001101" 得到 "010"
- [2,4,5] ，从 "001101" 得到 "101"
- [3,4,5] ，从 "001101" 得到 "101"

没有别的合法选择，所以总共有 6 种方法。

示例 2：

```text
输入：s = "11100"
输出：0
解释：没有任何符合题意的选择。
```

__提示：__

- `3 <= s.length <= 10 ^ 5`
- `s[i]` 要么是 `'0'` ，要么是 `'1'` 。

__思路:__

```text
计数
选择 3 个建筑，不能有相邻的建筑类型相同
只有两种情况可以选择 101 或者 010
遍历字符串，统计 0 的个数
记当前位置之前 0 的个数为 cur0, 1 的个数为 cur1
则如果当前位置为 1, 可以选择的方案数为 cur0 * (zero - cur0)
如果当前位置为 0, 可以选择的方案数为 cur1 * (n - zero - cur1)
其中 cur1 = i - cur0
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution {
    public long numberOfWays(String s) {
        long zero = 0L, cur0 = 0L, cur1 = 0L, result = 0L;
        for (char c : s.toCharArray()) zero += c == '0' ? 1 : 0;
        for (int i = 0, n = s.length(); i < n; i++) {
            result += (s.charAt(i) == '1' ? 1 : 0) * cur0 * (zero - cur0) + (s.charAt(i) == '0' ? 1 : 0) * (cur1 = i - cur0) * (n - zero - cur1);
            cur0 += s.charAt(i) == '0' ? 1 : 0;
        }
        return result;
    }
}
```

__Java__:

```Java
class Solution {
    public long numberOfWays(String s) {
        long zero = 0L, cur0 = 0L, cur1 = 0L, result = 0L;
        for (char c : s.toCharArray()) zero += c == '0' ? 1 : 0;
        for (int i = 0, n = s.length(); i < n; i++) {
            result += (s.charAt(i) == '1' ? 1 : 0) * cur0 * (zero - cur0) + (s.charAt(i) == '0' ? 1 : 0) * (cur1 = i - cur0) * (n - zero - cur1);
            cur0 += s.charAt(i) == '0' ? 1 : 0;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfWays(self, s: str) -> int:
        n, zero, cur0, result = len(s), s.count('0'), 0, 0
        for i, c in enumerate(s):
            result += (c == '1') * cur0 * (zero - cur0) + (c == '0') * (cur1 := i - cur0) * (n - zero - cur1)
            cur0 += c == '0'
        return result
```
