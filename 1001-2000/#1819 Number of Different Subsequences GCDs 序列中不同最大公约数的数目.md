# 1819 Number of Different Subsequences GCDs 序列中不同最大公约数的数目

__Description:__

You are given an array `nums` that consists of positive integers.

The GCD of a sequence of numbers is defined as the greatest integer that divides all the numbers in the sequence evenly.

- For example, the GCD of the sequence `[4,6,16]` is `2`.

A subsequence of an array is a sequence that can be formed by removing some elements (possibly none) of the array.

- For example, `[2,5,10]` is a subsequence of [1,2,1,__2__,4,1,__5__,__10__].

Return _the __number__ of __different__ GCDs among all __non-empty__ subsequences of_ `nums`.

__Example:__

Example 1:

![1819-1](https://assets.leetcode.com/uploads/2021/03/17/image-1.png)

```text
Input: nums = [6,10,3]
Output: 5
Explanation: The figure shows all the non-empty subsequences and their GCDs.
The different GCDs are 6, 10, 3, 2, and 1.
```

Example 2:

```text
Input: nums = [5,15,40,5,6]
Output: 7
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 2 * 10 ^ 5`

__题目描述:__

给你一个由正整数组成的数组 `nums` 。

数字序列的 最大公约数 定义为序列中所有整数的共有约数中的最大整数。

- 例如，序列 `[4,6,16]` 的最大公约数是 `2` 。

数组的一个 子序列 本质是一个序列，可以通过删除数组中的某些元素（或者不删除）得到。

- 例如， `[2,5,10]` 是 [1,2,1,__2__,4,1,__5__,__10__] 的一个子序列。

计算并返回 `nums` 的所有 __非空__ 子序列中 __不同__ 最大公约数的 __数目__ 。

__示例:__

示例 1：

![1819-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/03/image-1.png)

```text
输入：nums = [6,10,3]
输出：5
解释：上图显示了所有的非空子序列与各自的最大公约数。
不同的最大公约数为 6 、10 、3 、2 和 1 。
```

示例 2：

```text
输入：nums = [5,15,40,5,6]
输出：7
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 2 * 10 ^ 5`

__思路:__

```text
枚举
正面思考需要列出所有子序列的最大公约数, 时间复杂度为 O(2 ^ N)
必定超时
反面思考, 枚举所有的最大公约数, 检查这个数是否为数组中某个子序列的最大公约数
比如枚举 2, 子序列 [6, 10] 的最大公约数为 2, 结果加 1
设置一个访问数组, 将数组中的数都标记为已访问
枚举 [1, max(nums)] 中的每个数, 检查这个数是否为数组中某个子序列的最大公约数
只要遇到已经访问的数就求出当前的最大公约数
如果最大公约数等于当前枚举的数, 结果加 1
时间复杂度为 max(nums) + (max(nums) / 1 + max(nums) / 2 + ... + max(nums) / n) = O(MlogM)
时间复杂度为 O(MlogM), 空间复杂度为 O(N), 其中 M 为数组中的最大值, N 为数组的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countDifferentSubsequenceGCDs(vector<int>& nums) 
    {
        int result = 0, max_value = *max_element(nums.begin(), nums.end());
        bool visited[max_value + 1];
        memset(visited, 0, sizeof visited);
        for (const auto& num : nums) visited[num] = 1;
        for (int i = 1; i <= max_value; i++)
        {
            int cur = 0;
            for (int j = i; j <= max_value and cur != i; j += i) if (visited[j]) cur = gcd(cur, j);
            result += cur == i;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countDifferentSubsequenceGCDs(int[] nums) {
        int result = 0, maxValue = Arrays.stream(nums).max().getAsInt();
        boolean[] visited = new boolean[maxValue + 1];
        for (int num : nums) visited[num] = true;
        for (int i = 1; i <= maxValue; i++) {
            int cur = 0;
            for (int j = i; j <= maxValue && cur != i; j += i) if (visited[j]) cur = gcd(cur, j);
            if (cur == i) ++result;
        }
        return result;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

__Python__:

```Python
class Solution:
    def countDifferentSubsequenceGCDs(self, nums: List[int]) -> int:
        result, visited = 0, [False] * ((m := max(nums)) + 1)
        for num in nums:
            visited[num] = True
        for i in range(1, m + 1):
            cur = 0
            for j in range(i, m + 1, i):
                if visited[j]:
                    if (cur := gcd(cur, j)) == i:
                        break
            result += cur == i
        return result
```
