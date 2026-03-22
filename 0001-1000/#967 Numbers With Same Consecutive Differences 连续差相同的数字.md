# 967 Numbers With Same Consecutive Differences 连续差相同的数字

__Description__:
Return all non-negative integers of length n such that the absolute difference between every two consecutive digits is k.

Note that every number in the answer must not have leading zeros. For example, 01 has one leading zero and is invalid.

You may return the answer in any order.

__Example:__

Example 1:

Input: n = 3, k = 7
Output: [181,292,707,818,929]
Explanation: Note that 070 is not a valid number, because it has leading zeroes.

Example 2:

Input: n = 2, k = 1
Output: [10,12,21,23,32,34,43,45,54,56,65,67,76,78,87,89,98]

Example 3:

Input: n = 2, k = 0
Output: [11,22,33,44,55,66,77,88,99]

Example 4:

Input: n = 2, k = 2
Output: [13,20,24,31,35,42,46,53,57,64,68,75,79,86,97]

__Constraints:__

2 <= n <= 9
0 <= k <= 9

__题目描述__:
返回所有长度为 n 且满足其每两个连续位上的数字之间的差的绝对值为 k 的 非负整数 。

请注意，除了 数字 0 本身之外，答案中的每个数字都 不能 有前导零。例如，01 有一个前导零，所以是无效的；但 0 是有效的。

你可以按 任何顺序 返回答案。

__示例 :__

示例 1：

输入：n = 3, k = 7
输出：[181,292,707,818,929]
解释：注意，070 不是一个有效的数字，因为它有前导零。

示例 2：

输入：n = 2, k = 1
输出：[10,12,21,23,32,34,43,45,54,56,65,67,76,78,87,89,98]

示例 3：

输入：n = 2, k = 0
输出：[11,22,33,44,55,66,77,88,99]

示例 4：

输入：n = 2, k = 2
输出：[13,20,24,31,35,42,46,53,57,64,68,75,79,86,97]

__提示:__

2 <= n <= 9
0 <= k <= 9

__思路__:

动态规划
当只有一位数字时, 即 list(range(10)), 只有这种情况有数字 0
其他情况下对每个数字进行处理令 last = num % 10, 则最后一位数字是 last + k 或者 last - k, 需要保证数字在 0 - 9 范围内
用 set 保存防止重复
时间复杂度为 O(2 ^ n), 空间复杂度为 O(2 ^ n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> numsSameConsecDiff(int n, int k) 
    {
        unordered_set<int> s;
        for (int i = 0; i < 10; i++) s.insert(i);
        for (int i = 1; i < n; i++) 
        {
            unordered_set<int> cur;
            for (const auto& num : s)
            {
                if (!num) continue;
                int last = num % 10;
                if (last - k >= 0) cur.insert(10 * num + last - k);
                if (last + k <= 9) cur.insert(10 * num + last + k);
            }
            s = cur;
        }
        int index = 0;
        vector<int> result(s.size());
        for (const auto& num : s) result[index++] = num;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] numsSameConsecDiff(int n, int k) {
        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < 10; i++) set.add(i);
        for (int i = 1; i < n; i++) {
            Set<Integer> cur = new HashSet<>();
            for (int num : set) {
                if (num == 0) continue;
                int last = num % 10;
                if (last - k >= 0) cur.add(10 * num + last - k);
                if (last + k <= 9) cur.add(10 * num + last + k);
            }
            set = cur;
        }
        int index = 0, result[] = new int[set.size()];
        for (int num : set) result[index++] = num;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numsSameConsecDiff(self, n: int, k: int) -> List[int]:
        dp = set(range(10))
        for i in range(1, n):
            pre, dp = dp, set()
            for num in pre:
                if not num:
                    continue
                if (last := num % 10) - k >= 0:
                    dp.add(num * 10 + last - k)
                if last + k <= 9:
                    dp.add(num * 10 + last + k)
        return list(dp)
```
