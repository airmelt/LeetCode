# 1539 Kth Missing Positive Number 第k个缺失的正整数

__Description:__

Given an array `arr` of positive integers sorted in a __strictly increasing order__, and an integer `k`.

Return _the_ `k ^ th` ___positive__ integer that is __missing__ from this array._

__Example:__

Example 1:

```text
Input: arr = [2,3,4,7,11], k = 5
Output: 9
Explanation: The missing positive integers are [1,5,6,8,9,10,12,13,...]. The 5th missing positive integer is 9.
```

Example 2:

```text
Input: arr = [1,2,3,4], k = 2
Output: 6
Explanation: The missing positive integers are [5,6,7,...]. The 2nd missing positive integer is 6.
```

__Constraints:__

- `1 <= arr.length <= 1000`
- `1 <= arr[i] <= 1000`
- `1 <= k <= 1000`
- `arr[i] < arr[j]` for `1 <= i < j <= arr.length`

__Follow up:__

Could you solve this problem in less than O(n) complexity?

__题目描述:__

给你一个 __严格升序排列__ 的正整数数组 `arr` 和一个整数 `k` 。

请你找到这个数组里第 `k` 个缺失的正整数。

__示例:__

示例 1：

```text
输入：arr = [2,3,4,7,11], k = 5
输出：9
解释：缺失的正整数包括 [1,5,6,8,9,10,12,13,...] 。第 5 个缺失的正整数为 9 。
```

示例 2：

```text
输入：arr = [1,2,3,4], k = 2
输出：6
解释：缺失的正整数包括 [5,6,7,...] 。第 2 个缺失的正整数为 6 。
```

__提示：__

- `1 <= arr.length <= 1000`
- `1 <= arr[i] <= 1000`
- `1 <= k <= 1000`
- 对于所有 `1 <= i < j <= arr.length` 的 `i` 和 `j` 满足 `arr[i] < arr[j]`

__进阶：__

你可以设计一个时间复杂度小于 O(n) 的算法解决此问题吗？

__思路:__

```text
1. 模拟
遍历数组, 遇到每一个不大于 k 的元素, k 就自增, 直到 k 小于数组当前遍历的元素返回 k
时间复杂度为 O(N), 空间复杂度为 O(1)
2. 二分法
对 arr[i] 来说到 arr[i] 为止缺失的正数的个数为 arr[i] - i - 1
比如 arr[0] = 2, 那么 arr[0] - 0 - 1 = 1, 也就是说有 1 个元素缺失
由于数组是有序的, 可以考虑使用二分法查找
时间复杂度为 O(logN), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findKthPositive(vector<int>& arr, int k) 
    {
        if (arr.front() > k) return k;
        int n = arr.size(), l = 0, r = n, mid, x, MAX = 0x3f3f3f3f;
        while (l < r) 
        {
            if (-(mid = (l + r) >> 1) - 1 + (x = mid < n ? arr[mid] : MAX) >= k) r = mid;
            else l = mid + 1;
        }
        return k + l;
    }
};
```

__Java__:

```Java
class Solution {
    public int findKthPositive(int[] arr, int k) {
        for (int i: arr) if (i <= k) ++k;
        return k;
    }
}
```

__Python__:

```Python
class Solution:
    def findKthPositive(self, arr: List[int], k: int) -> int:
        return list(set(range(2001)) - set(arr))[k]
```
