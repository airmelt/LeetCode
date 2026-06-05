# 3043 Find the Length of the Longest Common Prefix 最长公共前缀的长度

__Description:__

You are given two arrays with __positive__ integers `arr1` and `arr2`.

A __prefix__ of a positive integer is an integer formed by one or more of its digits, starting from its __leftmost__ digit. For example, `123` is a prefix of the integer `12345`, while `234` is __not__.

A __common prefix__ of two integers `a` and `b` is an integer `c`, such that `c` is a prefix of both `a` and `b`. For example, `5655359` and `56554` have common prefixes `565` and `5655` while `1223` and `43456` __do not__ have a common prefix.

You need to find the length of the __longest common prefix__ between all pairs of integers `(x, y)` such that `x` belongs to `arr1` and `y` belongs to `arr2`.

Return _the length of the __longest__ common prefix among all pairs_. _If no common prefix exists among them_, _return_ `0`.

__Example:__

Example 1:

```text
Input: arr1 = [1,10,100], arr2 = [1000]
Output: 3
Explanation: There are 3 pairs (arr1[i], arr2[j]):
```

- The longest common prefix of (1, 1000) is 1.
- The longest common prefix of (10, 1000) is 10.
- The longest common prefix of (100, 1000) is 100.

The longest common prefix is 100 with a length of 3.

Example 2:

```text
Input: arr1 = [1,2,3], arr2 = [4,4,4]
Output: 0
Explanation: There exists no common prefix for any pair (arr1[i], arr2[j]), hence we return 0.
Note that common prefixes between elements of the same array do not count.
```

__Constraints:__

- `1 <= arr1.length, arr2.length <= 5 * 10 ^ 4`
- `1 <= arr1[i], arr2[i] <= 10 ^ 8`

__题目描述:__

给你两个 __正整数__ 数组 `arr1` 和 `arr2` 。

正整数的 __前缀__ 是其 __最左边__ 的一位或多位数字组成的整数。例如， `123` 是整数 `12345` 的前缀，而 `234` __不是__ 。

设若整数 `c` 是整数 `a` 和 `b` 的 __公共前缀__ ，那么 `c` 需要同时是 `a` 和 `b` 的前缀。例如， `5655359` 和 `56554` 有公共前缀 `565` 和 `5655`，而 `1223` 和 `43456` __没有__ 公共前缀。

你需要找出属于 `arr1` 的整数 `x` 和属于 `arr2` 的整数 `y` 组成的所有数对 `(x, y)` 之中最长的公共前缀的长度。

返回所有数对之中最长公共前缀的长度。如果它们之间不存在公共前缀，则返回 `0` 。

__示例:__

示例 1：

```text
输入：arr1 = [1,10,100], arr2 = [1000]
输出：3
解释：存在 3 个数对 (arr1[i], arr2[j]) ：
```

- (1, 1000) 的最长公共前缀是 1 。
- (10, 1000) 的最长公共前缀是 10 。
- (100, 1000) 的最长公共前缀是 100 。

最长的公共前缀是 100 ，长度为 3 。

示例 2：

```text
输入：arr1 = [1,2,3], arr2 = [4,4,4]
输出：0
解释：任何数对 (arr1[i], arr2[j]) 之中都不存在公共前缀，因此返回 0 。
请注意，同一个数组内元素之间的公共前缀不在考虑范围内。
```

__提示：__

- `1 <= arr1.length, arr2.length <= 5 * 10 ^ 4`
- `1 <= arr1[i], arr2[i] <= 10 ^ 8`

__思路:__

```text
暴力
枚举 arr1 中所有元素的前缀
使用除以 10 直到 0 的方式加入所有前缀
检查 arr2 中所有元素的前缀找到最大的元素
返回最大的元素的长度, 使用字符串计算长度即可
时间复杂度为 O((N + M)logU), 空间复杂度为 O(NlogU), U 为数组元素中的最大值
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int longestCommonPrefix(vector<int>& arr1, vector<int>& arr2) 
    {
        int result = 0;
        unordered_set<int> s;
        for (const auto& num : arr1) for (int a = num; a > 0; a /= 10) s.insert(a);
        for (const auto& num : arr2) for (int a = num; a > 0; a /= 10) if (s.count(a)) result = max(result, a);
        return result > 0 ? to_string(result).size() : 0;
    }
};
```

__Java__:

```Java
class Solution {
    public int longestCommonPrefix(int[] arr1, int[] arr2) {
        int result = 0;
        var set = new HashSet<Integer>();
        for (int num : arr1) for (int a = num; a > 0; a /= 10) set.add(a);
        for (int num : arr2) for (int a = num; a > 0; a /= 10) if (set.contains(a)) result = Math.max(result, a);
        return result > 0 ? Integer.toString(result).length() : 0;
    }
}
```

__Python__:

```Python
class Solution:
    def longestCommonPrefix(self, arr1: List[int], arr2: List[int]) -> int:
        s, result = set(), 0
        for num in arr1:
            while num:
                s.add(num)
                num //= 10
        for num in arr2:
            while num:
                if num in s:
                    result = max(result, len(str(num)))
                    break
                num //= 10
        return result
```
