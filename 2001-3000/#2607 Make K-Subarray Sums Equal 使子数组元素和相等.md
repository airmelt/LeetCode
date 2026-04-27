# 2607 Make K-Subarray Sums Equal 使子数组元素和相等

__Description:__

You are given a __0-indexed__ integer array `arr` and an integer `k`. The array `arr` is circular. In other words, the first element of the array is the next element of the last element, and the last element of the array is the previous element of the first element.

You can do the following operation any number of times:

- Pick any element from `arr` and increase or decrease it by `1`.

Return _the minimum number of operations such that the sum of each __subarray__ of length_ `k` _is equal_.

A subarray is a contiguous part of the array.

__Example:__

Example 1:

```text
Input: arr = [1,4,1,3], k = 2
Output: 1
Explanation: we can do one operation on index 1 to make its value equal to 3.
The array after the operation is [1,3,1,3]
```

- Subarray starts at index 0 is [1, 3], and its sum is 4
- Subarray starts at index 1 is [3, 1], and its sum is 4
- Subarray starts at index 2 is [1, 3], and its sum is 4
- Subarray starts at index 3 is [3, 1], and its sum is 4

Example 2:

```text
Input: arr = [2,5,5,7], k = 3
Output: 5
Explanation: we can do three operations on index 0 to make its value equal to 5 and two operations on index 3 to make its value equal to 5.
The array after the operations is [5,5,5,5]
```

- Subarray starts at index 0 is [5, 5, 5], and its sum is 15
- Subarray starts at index 1 is [5, 5, 5], and its sum is 15
- Subarray starts at index 2 is [5, 5, 5], and its sum is 15
- Subarray starts at index 3 is [5, 5, 5], and its sum is 15

__Constraints:__

- `1 <= k <= arr.length <= 10 ^ 5`
- `1 <= arr[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `arr` 和一个整数 `k` 。数组 `arr` 是一个循环数组。换句话说，数组中的最后一个元素的下一个元素是数组中的第一个元素，数组中第一个元素的前一个元素是数组中的最后一个元素。

你可以执行下述运算任意次：

- 选中 `arr` 中任意一个元素，并使其值加上 `1` 或减去 `1` 。

执行运算使每个长度为 `k` 的 __子数组__ 的元素总和都相等，返回所需要的最少运算次数。

子数组 是数组的一个连续部分。

__示例:__

示例 1：

```text
输入：arr = [1,4,1,3], k = 2
输出：1
解释：在下标为 1 的元素那里执行一次运算，使其等于 3 。
执行运算后，数组变为 [1,3,1,3] 。
```

- 0 处起始的子数组为 [1, 3] ，元素总和为 4
- 1 处起始的子数组为 [3, 1] ，元素总和为 4
- 2 处起始的子数组为 [1, 3] ，元素总和为 4
- 3 处起始的子数组为 [3, 1] ，元素总和为 4

示例 2：

```text
输入：arr = [2,5,5,7], k = 3
输出：5
解释：在下标为 0 的元素那里执行三次运算，使其等于 5 。在下标为 3 的元素那里执行两次运算，使其等于 5 。
执行运算后，数组变为 [5,5,5,5] 。
```

- 0 处起始的子数组为 [5, 5, 5] ，元素总和为 15
- 1 处起始的子数组为 [5, 5, 5] ，元素总和为 15
- 2 处起始的子数组为 [5, 5, 5] ，元素总和为 15
- 3 处起始的子数组为 [5, 5, 5] ，元素总和为 15

__提示：__

- `1 <= k <= arr.length <= 10 ^ 5`
- `1 <= arr[i] <= 10 ^ 9`

__思路:__

```text
数学
首先如果 sum(arr[i:i + k]) == sum(arr[i + 1:i + k + 1]), 那么
展开可得 arr[i] + arr[i + 1] + ... + arr[i + k - 1] == arr[i + 1] + arr[i + 2] + ... + arr[i + k]
即 arr[i] == arr[i + k]
因此可以将 arr 分成 gcd(k, n) 组, 每组的元素相等
然后对每组进行排序, 找到中位数 mid, 可以使用快速排序找到中位数
对于每组中的每个元素 x, 需要将其变成 mid, 操作次数为 abs(x - mid)
最后将每组的操作次数相加即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long makeSubKSumEqual(vector<int>& arr, int k) 
    {
        int n = arr.size(), total = gcd(k, n);
        vector<vector<int>> g(total);
        for (int i = 0; i < n; i++) g[i % total].emplace_back(arr[i]);
        long long result = 0LL;
        for (auto& b : g)
        {
            nth_element(b.begin(), b.begin() + ((int)b.size() >> 1), b.end());
            for (const auto& x : b) result += abs(x - b[b.size() >> 1]);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long makeSubKSumEqual(int[] arr, int k) {
        long result = 0L;
        for (int n = arr.length, total = gcd(k, n), i = 0; i < total; i++) {
            var b = new ArrayList<Integer>();
            for (int j = i; j < n; j += total) b.add(arr[j]);
            Collections.sort(b);
            for (int x : b) result += Math.abs(x - b.get(b.size() >>> 1));
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
    def makeSubKSumEqual(self, arr: List[int], k: int) -> int:
        result, k = 0, gcd(k, len(arr))
        for i in range(k):
            mid = (b := sorted(arr[i::k]))[len(b) >> 1]
            result += sum(abs(x - mid) for x in b)
        return result
```
