# 2094 Finding 3-Digit Even Numbers 找出 3 位偶数

__Description:__

You are given an integer array `digits`, where each element is a digit. The array may contain duplicates.

You need to find all the unique integers that follow the given requirements:

- The integer consists of the __concatenation__ of __three__ elements from `digits` in __any__ arbitrary order.
- The integer does not have __leading zeros__.
- The integer is __even__.

For example, if the given `digits` were `[1, 2, 3]`, integers `132` and `312` follow the requirements.

Return a sorted array of the unique integers.

__Example:__

Example 1:

```text
Input: digits = [2,1,3,0]
Output: [102,120,130,132,210,230,302,310,312,320]
Explanation: All the possible integers that follow the requirements are in the output array. 
Notice that there are no odd integers or integers with leading zeros.
```

Example 2:

```text
Input: digits = [2,2,8,8,2]
Output: [222,228,282,288,822,828,882]
Explanation: The same digit can be used as many times as it appears in digits. 
In this example, the digit 8 is used twice each time in 288, 828, and 882.
```

Example 3:

```text
Input: digits = [3,7,5]
Output: []
Explanation: No even integers can be formed using the given digits.
```

__Constraints:__

- `3 <= digits.length <= 100`
- `0 <= digits[i] <= 9`

__题目描述:__

给你一个整数数组 `digits` ，其中每个元素是一个数字（ `0 - 9`）。数组中可能存在重复元素。

你需要找出 所有 满足下述条件且 互不相同 的整数：

- 该整数由 `digits` 中的三个元素按 __任意__ 顺序 __依次连接__ 组成。
- 该整数不含 __前导零__
- 该整数是一个 __偶数__

例如，给定的 `digits` 是 `[1, 2, 3]` ，整数 `132` 和 `312` 满足上面列出的全部条件。

将找出的所有互不相同的整数按 递增顺序 排列，并以数组形式返回。

__示例:__

示例 1：

```text
输入：digits = [2,1,3,0]
输出：[102,120,130,132,210,230,302,310,312,320]
解释：
所有满足题目条件的整数都在输出数组中列出。 
注意，答案数组中不含有 奇数 或带 前导零 的整数。
```

示例 2：

```text
输入: digits = [2,2,8,8,2]
输出: [222,228,282,288,822,828,882]
解释: 
同样的数字（0 - 9）在构造整数时可以重复多次，重复次数最多与其在 `digits` 中出现的次数一样。 
在这个例子中，数字 8 在构造 288、828 和 882 时都重复了两次。
```

示例 3：

```text
输入：digits = [3,7,5]
输出：[]
解释：
使用给定的 digits 无法构造偶数。
```

__提示：__

- `3 <= digits.length <= 100`
- `0 <= digits[i] <= 9`

__思路:__

```text
1. 模拟
遍历所有 3 位数
如果大于等于 100 且是偶数, 则加入集合
最后返回集合的所有元素并排序
时间复杂度为 O(N ^ 3), 空间复杂度为 O(N ^ 3)
2. 计数
检查 100 - 999 之间的所有偶数
是否能够由 digits 中的数字构成
时间复杂度为 O(1), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findEvenNumbers(vector<int>& digits) 
    {
        vector<int> result;
        unordered_map<int, int> freq;
        for (const auto& digit: digits) ++freq[digit];
        for (int i = 100, j = i; i < 1000; i += 2, j = i)
        {
            unordered_map<int, int> cur;
            while (j)
            {
                ++cur[j % 10];
                j /= 10;
            }
            if (all_of(cur.begin(), cur.end(), [&](const auto& x) { return freq[x.first] >= cur[x.first]; })) result.emplace_back(i);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findEvenNumbers(int[] digits) {
        Set<Integer> set = new HashSet<>();
        for (int i = 0, n = digits.length; i < n; i++) for (int j = 0; j < n; j++) for (int k = 0; k < n; k++) if (i != j && j != k && i != k) if (digits[i] * 100 + digits[j] * 10 + digits[k] >= 100 && ((digits[i] * 100 + digits[j] * 10 + digits[k]) & 1) != 1) set.add(digits[i] * 100 + digits[j] * 10 + digits[k]);
        List<Integer> list = new ArrayList<>();
        for (int num : set) list.add(num);
        int[] result = new int[set.size()];
        for (int i = 0, m = set.size(); i < m; i++) result[i] = list.get(i);
        Arrays.sort(result);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def findEvenNumbers(self, digits: List[int]) -> List[int]:
        
```
