# 1497 Check If Array Pairs Are Divisible by k 检查数组对是否可以被k整除

__Description:__

Given an array of integers `arr` of even length `n` and an integer `k`.

We want to divide the array into exactly `n / 2` pairs such that the sum of each pair is divisible by `k`.

Return `true` _If you can find a way to do that or_ `false` _otherwise_.

__Example:__

Example 1:

```text
Input: arr = [1,2,3,4,5,10,6,7,8,9], k = 5
Output: true
Explanation: Pairs are (1,9),(2,8),(3,7),(4,6) and (5,10).
```

Example 2:

```text
Input: arr = [1,2,3,4,5,6], k = 7
Output: true
Explanation: Pairs are (1,6),(2,5) and(3,4).
```

Example 3:

```text
Input: arr = [1,2,3,4,5,6], k = 10
Output: false
Explanation: You can try all possible pairs to see that there is no way to divide arr into 3 pairs each with sum divisible by 10.
```

__Constraints:__

- `arr.length == n`
- `1 <= n <= 10 ^ 5`
- `n` is even.
- `-10 ^ 9 <= arr[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 5`

__题目描述:__

给你一个整数数组 `arr` 和一个整数 `k` ，其中数组长度是偶数，值为 `n` 。

现在需要把数组恰好分成 `n / 2` 对，以使每对数字的和都能够被 `k` 整除。

如果存在这样的分法，请返回 True ；否则，返回 False 。

__示例:__

示例 1：

```text
输入：arr = [1,2,3,4,5,10,6,7,8,9], k = 5
输出：true
解释：划分后的数字对为 (1,9),(2,8),(3,7),(4,6) 以及 (5,10) 。
```

示例 2：

```text
输入：arr = [1,2,3,4,5,6], k = 7
输出：true
解释：划分后的数字对为 (1,6),(2,5) 以及 (3,4) 。
```

示例 3：

```text
输入：arr = [1,2,3,4,5,6], k = 10
输出：false
解释：无法在将数组中的数字分为三对的同时满足每对数字和能够被 10 整除的条件。
```

__提示：__

- `arr.length == n`
- `1 <= n <= 10 ^ 5`
- `n` 为偶数
- `-10 ^ 9 <= arr[i] <= 10 ^ 9`
- `1 <= k <= 10 ^ 5`

__思路:__

```text
模拟
统计每个数除 k 的余数
首先判断除 k 的余数为 0 是否为偶数
然后判断除 k 的余数两两配对是否相等
时间复杂度为 O(N), 空间复杂度为 O(K)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    bool canArrange(vector<int>& arr, int k) 
    {
        vector<int> count(k);
        for (const auto& num: arr) ++count[(num % k + k) % k];
        if (count[0] & 1) return false;
        for (int i = 1, n = k >> 1; i <= n; i++) if (count[i] != count[k - i]) return false;
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canArrange(int[] arr, int k) {
        int[] count = new int[k];
        for (int num: arr) ++count[(num % k + k) % k];
        if ((count[0] & 1) == 1) return false;
        for (int i = 1, n = k >> 1; i <= n; i++) if (count[i] != count[k - i]) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def canArrange(self, arr: List[int], k: int) -> bool:
        return not ((mod := Counter(num % k for num in arr))[0] & 1) and not any(mod[i] != mod[k - i] for i in range(1, (k >> 1) + 1))
```
